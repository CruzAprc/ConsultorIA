# Memória do Agente

## Regras globais (ler SEMPRE)
- `memory/CLAUDE.md`
- `memory/memoria/regras/restricoes.md`
- `memory/memoria/regras/preferencias.md`
- `memory/memoria/regras/glossario.md`

## Copywriter (ler quando criar copy)
- `memory/agents/copywriter/copywriter.md`
- `memory/agents/copywriter/modules/` (11 módulos)

## Patterns validados
- `memory/memoria/patterns/criativos-estaticos.md`
- `memory/memoria/patterns/ctas.md`
- `memory/memoria/patterns/headlines.md`
- `memory/memoria/patterns/vsl.md`

## ConsultorIA (dieta + treino)

### Configuração e Regras (ler SEMPRE na 1ª mensagem)
- `config.md` — configuração principal + fluxo completo (Modo A/B, coleta, webhook)
- `memory/agents/consultoria-agent/regras-dieta.md` — regras obrigatórias de montagem (arroz branco, legumes/salada separados, salada à vontade, formato JSON webhook)

### Engine (fórmulas e cálculos)
- `engine/formulas.md` — TMB (Mifflin-St Jeor), TDEE (FA), macros por objetivo, peso ajustado
- `engine/distribuicao.md` — split de refeições + timing + regras de distribuição
- `engine/suplementacao.md` — protocolos suplementação (whey, creatina, cafeína, etc.)
- `engine/series-reps.md` — séries, reps e RPE por nível/objetivo
- `engine/volume-semanal.md` — volume semanal por grupo muscular
- `engine/nutricao-avancada.md` — nutrição avançada (periodização de carb, etc.)
- `engine/nutricao-dia-off.md` — ajustes nutricionais para dias sem treino

### Decisão (triagem e segurança)
- `decisao/triagem.md` — red flags: IMC extremo, idade <16/>60, condições graves
- `decisao/seguranca.md` — lesões, medicações, condições médicas, AE
- `decisao/ajuste-feedback.md` — ajustes baseados em feedback do aluno
- `decisao/renovacao-30dias.md` — protocolo de renovação mensal

### Database (alimentos e substitutos)
- `database/alimentos.json` — base de alimentos com macros
- `database/substitutos.json` — substitutos por grupo alimentar
- `database/refeicoes-template.json` — templates de refeições

### Treinos (exercícios e divisões)
- `treinos/exercicios.json` — base de exercícios com grupo muscular
- `treinos/divisoes.json` — divisões (PPL, Upper/Lower, Full Body, ABC)
- `treinos/metodos.json` — métodos intensificadores (drop set, rest-pause, etc.)
- `treinos/adaptacoes-lesao.json` — adaptações para lesões específicas
- `treinos/periodizacao.md` — periodização de treino

### Output (templates de entrega)
- `output/template-dieta.md` — template formatado de dieta
- `output/template-treino.md` — template formatado de treino
- `output/template-output.json` — estrutura JSON para webhook n8n

### Fluxo n8n (webhook)
- URL: `https://wb.caos.app.br/webhook/get-messages`
- `body.id` = lead_id (não submission!)
- "📋 Nova Consulta" = dados completos → processar
- "✅ Protocolo Inserido" = confirmação (não reprocessar)
- Formato: `opcao_principal` = strings, `substitutos` = strings, `dia` = "TREINO A — Nome (Dia)"

### Aprendizados consolidados (regras validadas em 50+ protocolos)
- **Peso Ajustado obrigatório se IMC ≥30** (fórmula em formulas.md)
- **AE + Emagrecimento = proteína 2.4g/kg PA** (déficit + AE = risco catabolismo)
- **Ex-AE SEMPRE pedir exames** (testo, LH, FSH, estradiol, hepatograma, lipídico)
- **Crohn/IBD:** +10% conservador, legumes COZIDOS, whey ISOLADO, glutamina obrigatória, RPE max 7-8
- **Energético diário = cortado** (overdose cafeína → sono ruim)
- **IF + treino 6x = contraindicado** (volume alto precisa energia constante)
- **Cintura=0 ou negativa = dado inválido** (ignorar, registrar como "não medido")
- **Nordestinos:** cuscuz como carb base preferencial
- **Testo <500 ng/dL + ex-AE → encaminhar endocrinologista**
- **Convergência:** ±30kcal, ±3g prot, ±5g carb, ±2g gord
- **Hidratação:** 35ml/kg/dia

## Kallaway (social content)
- `memory/agents/kallaway/` (5 arquivos)
