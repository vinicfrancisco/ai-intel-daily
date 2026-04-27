AI Intel Daily — Bamse
Repositório de suporte para a routine Daily AI Intel — Bamse. Serve como memória de longo prazo entre execuções e centraliza a configuração de fontes.
Como funciona
A cada execução, a routine:

Lê state.json para saber o que já foi reportado nos últimos 7 dias.
Coleta novidades das fontes listadas em sources.yml.
Filtra, ranqueia e gera um relatório.
Posta no canal #ia do Slack via connector.
Atualiza state.json e commita em uma branch claude/daily-intel-YYYY-MM-DD.

Arquivos

state.json — Histórico dos itens já reportados, com expiração de 7 dias.
sources.yml — Lista de fontes oficiais + queries do GitHub (single source of truth).
CLAUDE.md — Este arquivo.

Princípios editoriais

Sinal sobre ruído: máximo 6 itens. É melhor 3 ótimos do que 8 medianos.
Acionável para devs: o filtro mental é "isso muda como o time da Bamse trabalha?". Lançamento de modelo, nova feature de Claude Code/Codex, novo SDK, novo padrão (MCP, skills, etc.) → entra. Notícia de funding, opinion piece, drama de CEO → fica de fora.
Fontes primárias: o link sempre aponta para a fonte original (blog oficial, repo, release notes), nunca para agregadores tipo TechCrunch reescrevendo um post da Anthropic.
Honestidade > volume: se o dia tá fraco, a mensagem deve ser "Sem novidades relevantes hoje" + 1-2 itens menores como contexto. Nunca inventar relevância.

Vídeos (YouTube)

Vídeos entram na seção do tópico (lab/tool sendo discutido), não na seção do canal. Use prefixo 🎥 no bullet.
Tier do canal define o nível de filtro: official_lab passa direto se for anúncio/demo; technical_community só se cobrir release < 72h; high_noise exige segunda fonte confirmando o lançamento.
Tutoriais sobre tech estável (>30 dias sem release novo associado) ficam de fora — não são novidade.
Shorts e vídeos de reaction/clickbait são sempre rejeitados (ver reject_title_patterns em sources.yml).

Categorias do relatório
Ordem fixa, omitindo seções vazias:

🟧 Anthropic (incluindo Claude Code)
🟦 OpenAI (incluindo Codex)
🟥 Google DeepMind
⚫ Outros labs e frameworks (LangChain, LlamaIndex, Vercel AI, Microsoft, GitHub, a16z)
🛠 Ferramentas dev (Cursor, Windsurf, Cognition/Devin)
⭐ GitHub trending (repositórios de agentes em alta)
💬 Comunidade (Hacker News — só itens com >150 pontos e relevância clara)

Não fazer

Não traduzir títulos. Manter no idioma original (em geral inglês), descrição em português.
Não usar emojis decorativos no corpo do texto, só os de seção acima.
Não citar mais de 15 palavras de qualquer fonte (copyright).
Não incluir o mesmo item duas vezes na semana, mesmo que reapareça em fontes diferentes.
