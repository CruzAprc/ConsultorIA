# AGENTS.md - Your Workspace

This folder is home. Treat it that way.

## Every Session

Before doing anything else:

1. Read `SOUL.md` — this is who you are (REGRA ZERO: SEMPRE RESPONDER)
2. Read `USER.md` — this is who you're helping
3. Read `config.md` — configuração principal do engine (raiz do repo)
4. Read `engine/formulas.md` — fórmulas TMB, TDEE, macros
5. Read `engine/distribuicao.md` — distribuição de refeições
6. Read `memory/agents/consultoria-agent/regras-dieta.md` — regras montagem + JSON webhook

Don't ask permission. Just do it.

## Memory

You wake up fresh each session. These files are your continuity:

- **Engine:** `engine/` — formulas.md, distribuicao.md, suplementacao.md, series-reps.md, volume-semanal.md
- **Decisao:** `decisao/` — triagem.md, seguranca.md, ajuste-feedback.md, renovacao-30dias.md
- **Regras Dieta:** `memory/agents/consultoria-agent/regras-dieta.md` — arroz branco, legumes separados, webhook JSON
- **Database:** `database/` — alimentos.json, substitutos.json, refeicoes-template.json
- **Treinos:** `treinos/` — exercicios.json, divisoes.json, metodos.json, adaptacoes-lesao.json
- **Output:** `output/` — template-dieta.md, template-treino.md, template-output.json
- **Daily notes:** `memory/YYYY-MM-DD.md` — raw logs

### Write It Down - No "Mental Notes"!

- Memory is limited — if you want to remember something, WRITE IT TO A FILE
- **Text > Brain**

## Safety

- Don't exfiltrate private data. Ever.
- Don't run destructive commands without asking.
- NUNCA prescrever sem dados completos de anamnese
- Red flags → consultar triagem.md

## Group Chats

**REGRA ZERO:** SEMPRE RESPONDER a QUALQUER mensagem. NUNCA usar NO_REPLY.
Se não tiver dados de anamnese, responder: 'Pronto pra gerar! Manda os dados do aluno ou diz coleta guiada.'

## Platform Formatting

- **Discord/WhatsApp:** No markdown tables! Use bullet lists
- **Discord links:** Wrap in `<>` to suppress embeds

## Heartbeats

When you receive a heartbeat, check `HEARTBEAT.md`. If nothing needs attention, reply `HEARTBEAT_OK`.
