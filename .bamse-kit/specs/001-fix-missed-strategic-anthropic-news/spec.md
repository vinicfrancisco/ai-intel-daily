---
title: "Fix: anúncios estratégicos de labs oficiais sendo descartados pelo filtro"
description: "Garantir que posts oficiais de Anthropic/OpenAI/DeepMind cujo título descreve o vertical (e não o mecanismo técnico) cheguem ao relatório quando contiverem novos conectores, MCP servers, integrações ou expansão de produto."
version: 1.0.0
created: 2026-05-02
last_updated: 2026-05-02
last_change_summary: "Criação da spec leve"
mode: fast
---

# Fix: anúncios estratégicos de labs oficiais sendo descartados pelo filtro

## Contexto

Em 28/04/2026 a Anthropic publicou "Claude for Creative Work" anunciando o primeiro **MCP connector oficial** (Blender) e 7 outras integrações nativas. O post não entrou no relatório de 29/04/2026, apesar de cair na janela de 24h e da fonte (`https://www.anthropic.com/news`) estar configurada em `sources.yml`. A causa raiz é uma combinação de duas falhas em camadas: (1) a heurística "fetch só se o título sugerir relevância" do Subagente A descartou o post pelo título focado em vertical criativo, sem nunca ler o conteúdo; (2) o filtro editorial subsequente confirmou o descarte por operar sobre `{title, summary}` empobrecidos. O mesmo padrão vai falhar de novo com qualquer "Claude for X" (Healthcare, Legal, etc.) ou anúncio análogo de OpenAI/DeepMind.

## O que será feito

1. Para os blogs dos 3 labs oficiais (`blogs.anthropic`, `blogs.openai`, `blogs.google_deepmind`), o Subagente A deve fazer fetch completo de **todos** os posts publicados nas últimas 24h, sem filtragem por título.
2. Para os demais blogs (`dev_tools`, `frameworks`, `microsoft_github`, `vc_thought`), manter a heurística atual de fetch por relevância de título.
3. Adicionar à seção "Princípios editoriais" do `CLAUDE.md` uma regra explícita classificando como "entra" os anúncios oficiais de labs sobre novos conectores, integrações oficiais, MCP servers oficiais ou expansão para novos verticais ("Claude for X"), mesmo quando o vertical não é dev. Manter explicitamente fora: posts de liderança/hiring, funding, opinion pieces, policy.

## Abordagem escolhida

Foco cirúrgico em duas camadas complementares — ambas necessárias, nenhuma suficiente sozinha:

- **Camada 1 (entrada de dados):** alterar a regra de fetch do Subagente A em `routine-prompt.md` para forçar leitura do conteúdo completo dos posts dos labs oficiais. Sem isso, o ângulo técnico (MCP, SDK, connector) nunca chega ao filtro.
- **Camada 2 (decisão editorial):** adicionar regra em `CLAUDE.md` que reconhece "estratégia de produto" como sinal válido. Sem isso, mesmo lendo o conteúdo, o filtro editorial pode descartar via "isso muda como o time trabalha?" + "em caso de dúvida, exclua".

Volume adicional estimado: ~3-5 fetches/dia entre os 3 labs (custo desprezível). A regra de "máx. 6 itens" continua aplicada — o filtro fica menos agressivo no descarte, não bypassa o cap de volume.

## Edge Cases

- **Post de liderança/hiring/funding/policy de lab oficial:** continua sendo descartado pelo filtro editorial, mesmo com fetch completo. A nova regra em `CLAUDE.md` lista explicitamente o que NÃO entra.
- **Mais de 6 candidatos fortes num dia:** sem mudança — segue a regra atual de priorizar diversidade de categorias até o cap de 6.
- **Post de lab oficial republicado/revisitado dentro de 7 dias:** continua sendo deduplicado por `state.json`, sem mudança.
- **Lab oficial com listagem extensa (>10 posts em 24h):** improvável, mas se acontecer, fetch completo em todos eles é aceitável (volume ainda baixo).

## Fora de escopo

- Whitelist de keywords (`connector`, `MCP`, `SDK`, etc.) forçando fetch nos blogs não-oficiais (`dev_tools`, `frameworks`, etc.). Foi descartado nesta iteração para manter mudança mínima.
- Audit log de itens rejeitados pelo Subagente A (`.context/last-run-rejects.json`) para revisão humana opcional. Foi descartado nesta iteração.
- Revisão da cobertura de fontes (ex.: adicionar `@AnthropicAI` no X, blog do AWS Bedrock, paginação da página `/news`).
- Revisão geral do filtro editorial além da regra específica adicionada.
