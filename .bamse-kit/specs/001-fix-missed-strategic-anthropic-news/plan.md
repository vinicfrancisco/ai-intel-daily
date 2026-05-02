---
title: "Fix: anúncios estratégicos de labs oficiais — Plano de Implementação"
description: "Plano de 2 tasks: ajustar regra de fetch do Subagente A e adicionar princípio editorial sobre estratégia de produto."
version: 1.0.0
created: 2026-05-02
last_updated: 2026-05-02
last_change_summary: "Criação do plano"
---

# Fix: anúncios estratégicos de labs oficiais — Plano de Implementação

**Objetivo:** Garantir que anúncios oficiais de Anthropic/OpenAI/DeepMind sobre conectores, MCP servers, integrações ou expansões de produto cheguem ao relatório, mesmo quando o título descreve o vertical (ex.: "Claude for X") em vez do mecanismo técnico.
**Abordagem:** Duas mudanças textuais coordenadas em arquivos de prompt/config: (1) trocar a regra de fetch do Subagente A em `routine-prompt.md` para forçar leitura completa de todo post de lab oficial das últimas 24h; (2) adicionar um novo princípio editorial em `CLAUDE.md` que reconhece "estratégia de produto" como sinal válido de relevância. Sem código a executar — só edições de texto. `sources.yml` não é modificado.
**Spec leve:** `.bamse-kit/specs/001-fix-missed-strategic-anthropic-news/spec.md`

---

### Task 1: Atualizar regra de fetch do Subagente A em `routine-prompt.md`

**Arquivos:** Modify `routine-prompt.md`

- [ ] Localizar o bloco "Subagente A — Blogs oficiais" (linha 25-26 hoje), cujo texto atual é:
  > "Use WebFetch nas URLs de `sources.yml > blogs` para identificar posts publicados nas últimas 24h. Para cada blog, pegue apenas a página de listagem; só faça fetch do post completo se o título sugerir relevância clara para o time."

- [ ] Substituir esse parágrafo por:
  > "Use WebFetch nas URLs de `sources.yml > blogs` para identificar posts publicados nas últimas 24h. A política de fetch do post completo varia por origem:
  > - **Labs oficiais (`blogs.anthropic`, `blogs.openai`, `blogs.google_deepmind`):** faça fetch completo de **todos** os posts publicados nas últimas 24h, sem filtragem por título. Razão: o título pode descrever o vertical (ex.: 'Claude for Creative Work', 'Claude for X') e esconder o ângulo técnico real (novo MCP connector, SDK, skill, integração oficial). Sem ler o conteúdo, esses anúncios são silenciosamente perdidos.
  > - **Demais blogs (`blogs.dev_tools`, `blogs.frameworks`, `blogs.microsoft_github`, `blogs.vc_thought`):** mantenha a heurística — pegue só a página de listagem e só faça fetch do post completo se o título sugerir relevância clara para o time."

### Task 2: Adicionar novo princípio editorial em `CLAUDE.md`

**Arquivos:** Modify `CLAUDE.md`

- [ ] Localizar a seção "Princípios editoriais" (linha 13). **Importante: o arquivo NÃO usa bullets markdown** — cada princípio é uma linha de prosa começando com `Nome do princípio: descrição.` (ex.: linha 15 `Sinal sobre ruído: máximo 6 itens...`, linha 16 `Acionável para devs: o filtro mental é...`). Sem `-`, `*` ou `•` no início. Mantenha esse formato.

- [ ] Inserir uma nova linha em prosa **logo após a linha de "Acionável para devs"** (e antes da linha de "Fontes primárias"), separada por uma linha em branco (mesmo padrão das siblings). O conteúdo da nova linha deve ser exatamente:
  > `Estratégia conta como sinal: anúncios oficiais de labs (Anthropic/OpenAI/DeepMind) sobre novos conectores, integrações oficiais com aplicações externas, MCP servers oficiais ou expansão para novos verticais (ex.: "Claude for X") entram, mesmo quando o vertical não é dev. Razão: revelam para onde o ecossistema vai e podem virar trabalho do time amanhã. Continua de fora: posts de liderança/hiring, funding, opinion pieces e policy.`

- [ ] Verificar que a ordem final das linhas de princípio ficou: `Sinal sobre ruído` → `Acionável para devs` → `Estratégia conta como sinal` → `Fontes primárias` → `Honestidade > volume`, todas em prosa, sem prefixo de bullet, separadas por linha em branco.
