# ConsultorIA

Sistema de IA para prescricao automatica de **dieta + treino personalizado**, construido a partir de **3.106 consultorias reais** de 14 consultorias fitness.

---

## Como Funciona — Passo a Passo

O ConsultorIA segue um pipeline de **8 etapas obrigatorias** para gerar o plano completo. Nenhuma etapa pode ser pulada.

```
ANAMNESE ──> TRIAGEM ──> CALCULOS ──> DIETA ──> TREINO ──> REVISAO ──> OUTPUT ──> RENOVACAO
   1            2           3          4          5          6          7           8
```

---

### ETAPA 1 — Coleta de Anamnese

A IA coleta **23 campos obrigatorios** do paciente. Pode receber tudo de uma vez (webhook/formulario) ou coletar campo por campo.

**Dados Pessoais**
- Nome completo
- Data de nascimento (idade 14-80)
- Sexo (M/F) — muda formula TMB + macros

**Antropometria**
- Peso atual (kg) — valida: 30-250kg
- Altura (cm) — valida: 100-220cm
- IMC — calculado automaticamente. Se <16 ou >35: red flag

**Objetivo**
- Texto livre do paciente — normalizado para 1 das 11 categorias

**Experiencia de Treino**
- Tempo de treino — classifica nivel (Iniciante/Intermediario/Avancado)
- Frequencia semanal (1-7x)
- Tempo por sessao (30-180 min)
- Horario do treino (manha/tarde/noite) — impacta distribuicao de refeicoes

**Saude**
- Doencas cronicas — verificar contra regras de seguranca
- Medicacoes — verificar interacoes
- Lesoes ou limitacoes — mapear adaptacoes de exercicios

**Alimentacao**
- Restricoes (lactose, gluten, vegano, vegetariano)
- Alimentos que NAO gosta — serao excluidos
- Alimentos obrigatorios — serao incluidos
- Numero de refeicoes por dia (3-6)
- Horarios disponiveis para comer
- Primeira refeicao: solida ou liquida?

**Suplementacao**
- Suplementos que ja usa
- Usa ergogenicos/esteroides? (S/N) — ajusta macros

**Detalhes do Treino**
- Academia ou casa? — impacta exercicios disponiveis
- Grupo muscular prioritario — recebe +2-3 series
- Hora que acorda — define horario da 1a refeicao

**Regra:** Ao final, a IA exibe um resumo completo de TODOS os campos e so avanca apos confirmacao do paciente.

> Arquivo: `config.md`

---

### ETAPA 2 — Triagem e Classificacao

A IA analisa os dados coletados e toma 4 decisoes:

**2.1 — Normalizar o objetivo (60+ variantes -> 11 categorias)**

| # | Categoria | N na base | Kcal/kg | Tipo |
|---|-----------|-----------|---------|------|
| 1 | Bulking | 1.127 | 41.0 | Superavit |
| 2 | Emagrecimento | 848 | 25.0 | Deficit |
| 3 | Cutting | 251 | 24.7 | Deficit |
| 4 | Definicao muscular | 229 | 28.1 | Deficit |
| 5 | Perda de peso | 194 | 22.3 | Deficit |
| 6 | Ganho de massa | 81 | 39.4 | Superavit |
| 7 | Perder peso | 79 | 21.5 | Deficit |
| 8 | Hipertrofia | 20 | 37.0 | Superavit |
| 9 | Ganhar peso | 12 | 45.9 | Superavit |
| 10 | Secar | 6 | 29.6 | Deficit |
| 11 | Recomposicao | 4 | 29.4 | Neutro |
| — | Manutencao | — | ~33 | Neutro |

Cada categoria tem macros (Prot/Gord/Carb em g/kg) extraidos da media real das consultorias.

**2.2 — Classificar nivel de experiencia**

| Tempo de treino | Nivel | Volume (series/sem) | Metodos permitidos |
|-----------------|-------|--------------------|--------------------|
| < 6 meses | Iniciante | 10-12 | Convencional, Piramide |
| 6m - 2 anos | Intermediario | 14-18 | + Drop-set, Rest-Pause, Pico |
| > 2 anos | Avancado | 18-24 | + Bi-set, FST-7, Cluster |

**2.3 — Validar objetivo vs IMC**

O objetivo do paciente e soberano. A IA so intervem em casos extremos:

| IMC | Acao |
|-----|------|
| 18.5-25 | Zero ajuste, respeitar 100% |
| 25-30 | Bulking: limitar superavit a +10-15% |
| 30-35 | Bulking: limitar a +5-10% + nota sugerindo recomposicao |
| > 35 | Bulking: redirecionar para Recomposicao |
| > 40 | Encaminhar medico, qualquer objetivo |
| < 16 | Encaminhar medico |

**2.4 — Identificar Red Flags**

| Encaminhar Medico (nao prescrever) | Prescrever com Cautela |
|------------------------------------|------------------------|
| Doenca renal | Diabetes T2 controlada → carbs <= 3.0 g/kg |
| Diabetes T1 sem controle | Hipotireoidismo → TDEE -10% |
| Cardiopatias graves | SOP → carbs <= 2.5 g/kg, baixo IG |
| IMC < 16 ou > 40 | Hipertensao → sem cafeina, sodio < 2000mg |
| Idade < 14 | Lesoes ortopedicas → adaptar treino |
| Gestantes / lactantes | Idade > 55 → prot min 1.8 g/kg |
| Transtornos alimentares | Metformina → suplementar B12 |

> Arquivo: `decisao/triagem.md`, `decisao/seguranca.md`

---

### ETAPA 3 — Calculos

A IA executa os calculos nesta ordem:

**3.1 — TMB (Taxa Metabolica Basal) — 2 formulas**

```
HARRIS-BENEDICT (primaria):
  Homem:  88.362 + (13.397 x peso) + (4.799 x altura) - (5.677 x idade)
  Mulher: 447.593 + (9.247 x peso) + (3.098 x altura) - (4.330 x idade)

MIFFLIN-ST JEOR (validacao):
  Homem:  (10 x peso) + (6.25 x altura) - (5 x idade) + 5
  Mulher: (10 x peso) + (6.25 x altura) - (5 x idade) - 161
```

Regra: calcular AMBAS. Se diferenca <= 10%, usar Harris-Benedict. Se > 10%, usar media ponderada (60% Mifflin + 40% HB). Para obesos (IMC > 35), preferir Mifflin.

**3.2 — TDEE (Gasto Energetico Total)**

```
TDEE = TMB x Fator de Atividade
```

| Frequencia | Nivel | Fator |
|------------|-------|-------|
| 1-2x/sem | Qualquer | 1.375 |
| 3x/sem | Iniciante | 1.375 |
| 3x/sem | Inter/Avancado | 1.55 |
| 4-5x/sem | Iniciante/Inter | 1.55 |
| 5x/sem | Avancado | 1.725 |
| 6x/sem | Qualquer | 1.725 |
| 7x/sem | Qualquer | 1.9 |

**3.3 — Ajuste Calorico por Objetivo**

| Objetivo | Ajuste sobre TDEE |
|----------|-------------------|
| Bulking | +15% a +20% |
| Cutting | -15% a -20% |
| Emagrecimento | -20% a -25% |
| Recomposicao | -5% a -10% |
| Manutencao | 0% |

**3.4 — Peso Ajustado (se IMC > 30)**

Quando IMC > 30, usar peso total para macros gera valores excessivos. A IA calcula:

```
Peso_ideal = 22 x (altura_m)^2
Peso_ajustado = Peso_ideal + 0.25 x (Peso_real - Peso_ideal)
```

Exemplo: paciente 130kg, 1.75m → Peso ajustado = 83kg → Proteina 166g (vs 260g com peso total).

- TMB: usa peso REAL
- Macros (g/kg): usa peso AJUSTADO
- Hidratacao: usa peso REAL

**3.5 — Calcular Macros (g/kg)**

| Objetivo (Homem) | Proteina | Gordura | Carboidrato |
|-------------------|----------|---------|-------------|
| Bulking | 2.0-2.2 | 0.8-1.0 | 5.0-6.0 |
| Cutting | 1.8-2.0 | 0.5-0.7 | 2.5-3.0 |
| Emagrecimento | 1.6-1.8 | 0.5-0.6 | 2.0-2.8 |
| Recomposicao | 2.0 | 0.6-0.8 | 3.0-4.0 |
| Manutencao | 1.8-2.0 | 0.7-0.9 | 3.5-4.5 |

Mulheres: proteina e carb ligeiramente menores, gordura minima mais alta (saude hormonal).

Conversao: Proteina 1g = 4kcal | Carboidrato 1g = 4kcal | Gordura 1g = 9kcal

**3.6 — Distribuir por Refeicoes**

Exemplo com 5 refeicoes:

| Refeicao | % Kcal |
|----------|--------|
| Cafe da Manha | 20% |
| Lanche Manha | 10% |
| Almoco | 30% |
| Lanche Tarde | 15% |
| Jantar | 25% |

Ajustado conforme horario do treino (pre/pos treino recebem mais carbs).

> Arquivo: `engine/formulas.md`, `engine/distribuicao.md`

---

### ETAPA 4 — Montagem da Dieta

Com os macros por refeicao definidos, a IA monta o plano alimentar:

**Passo 1:** Selecionar alimentos do banco (135 alimentos com macros TACO)
- Respeitar restricoes (lactose, gluten, vegan)
- Excluir tudo que o paciente disse que nao gosta
- Incluir alimentos obrigatorios

**Passo 2:** Calcular gramagens exatas para atingir os macros de cada refeicao

**Passo 3:** Atribuir substitutos — minimo 3 por alimento, com gramagens equivalentes
- Exemplo: "Arroz branco 270g → Arroz integral 270g / Macarrao integral 250g / Batata doce 350g"

**Passo 4:** Incluir suplementacao se aplicavel
- Creatina: 3-10g/dia conforme peso (93% das prescricoes = 5g)
- Whey: 25-30g quando nao atinge proteina via comida
- Veganos: B12 obrigatoria, DHA de alga, ferro, zinco

**Regras obrigatorias:**
1. Carboidrato do almoco = mesmo carboidrato do jantar (variacoes nos substitutos)
2. Minimo 3 substitutos por alimento com gramagens
3. 1 vegetal principal por refeicao, outros como substituicoes
4. Hidratacao: 35ml x peso/dia minimo

> Arquivos: `database/alimentos.json`, `database/substitutos.json`, `engine/suplementacao.md`

---

### ETAPA 5 — Montagem do Treino

A IA monta o plano de treino seguindo esta ordem:

**Passo 1:** Selecionar divisao baseada em frequencia + nivel + objetivo

| Frequencia | Divisao |
|------------|---------|
| 1-2x/sem | Full Body |
| 3x/sem | ABC |
| 4x/sem | ABCD ou Upper/Lower |
| 5x/sem | ABCDE |
| 6x/sem | ABCDEF ou ABC 2x |

**Passo 2:** Montar exercicios por grupo muscular (152 exercicios no banco)
- Compostos primeiro (agachamento, supino, remada)
- Isoladores depois (rosca, triceps, lateral)
- Grupo prioritario recebe +2-3 series extras

**Passo 3:** Verificar contra lesoes (16 lesoes mapeadas)
- Joelho → evitar impacto, preferir maquinas
- Ombro → evitar overhead livre, usar smith/maquinas
- Lombar → remover terra, agachamento livre → leg press, hack
- Se duvida: REMOVER exercicio (nao arriscar)

**Passo 4:** Aplicar metodos de intensificacao conforme nivel
- Iniciante: Convencional, Piramide Crescente
- Intermediario: + Drop-set, Rest-Pause, Pico de Contracao
- Avancado: + Bi-set, FST-7, Cluster, Isometrico

**Passo 5:** Definir series x reps x descanso
- Hipertrofia: 3-4 x 8-12 reps | 60-90s descanso
- Forca: 4-5 x 4-6 reps | 120-180s descanso
- Resistencia: 2-3 x 15-20 reps | 30-45s descanso

**Passo 6:** OBRIGATORIO — Incluir glossario dos metodos usados no final do treino

> Arquivos: `treinos/exercicios.json`, `treinos/divisoes.json`, `treinos/metodos.json`, `treinos/adaptacoes-lesao.json`

---

### ETAPA 6 — Revisao Automatica

A IA valida TUDO antes de entregar. Se algum item falhar, corrige e revalida.

**Convergencia nutricional (tolerancias):**
- Soma kcal das refeicoes = kcal alvo → tolerancia +-30 kcal
- Soma proteina = total calculado → tolerancia +-3g
- Soma carboidrato = total → tolerancia +-5g
- Soma gordura = total → tolerancia +-2g

**Restricoes:**
- Zero alimento que conflita com restricoes (lactose, gluten, vegan)
- Zero alimento que o paciente disse que nao gosta
- Creatina com dose correta para o peso

**Treino:**
- Zero exercicio que conflita com lesoes
- Exercicios compativeis com nivel do aluno
- Frequencia de treino bate com o informado

**Se nao convergir:**
- Iteracoes 1-5: ajustar gramagens
- Se nao convergir em 5 iteracoes: rebalancear refeicoes do zero
- NUNCA entregar com nota "necessita ajuste" — o ajuste e obrigatorio

> Regra de convergencia definida em `config.md`

---

### ETAPA 7 — Output (Entrega)

Apos aprovacao na revisao, a IA entrega em 2 canais:

**1. Resposta no canal** (Discord) — para visualizacao do consultor

Usa os templates formatados com:
- Tabela nutricional completa (refeicao, alimento, gramagem, macros)
- Substituicoes para cada alimento
- Tabela de treino (exercicio, series, reps, descanso, metodo)
- Glossario de metodos
- Suplementacao
- Hidratacao

**2. POST no webhook n8n** — para integracao automatica

```
POST https://wb.caos.app.br/webhook/discord-message-received
Content-Type: application/json
```

JSON completo com todos os dados para registro em Google Sheets e automacoes.

Regras:
- SOMENTE apos revisao APROVADA
- NUNCA enviar dados parciais ou nao validados

> Arquivos: `output/template-dieta.md`, `output/template-treino.md`, `output/template-output.json`

---

### ETAPA 8 — Renovacao (a cada 30 dias)

A cada 30 dias o paciente envia feedback + fotos + peso atualizado. A IA analisa e decide:

**Arvore de decisao:**

| Cenario | Acao |
|---------|------|
| Peso subiu + fotos mostram mais gordura | Reduzir superavit -5% ou aumentar deficit |
| Peso subiu + fotos mostram mais musculo | PERFEITO — ajustar macros p/ novo peso |
| Peso caiu + fotos mostram mais definicao | PERFEITO — ajustar macros p/ novo peso |
| Peso estavel + sem mudanca visual | Plato — ajustar kcal +-10%, trocar isoladores |
| Feedback negativo (dores/fome/sono/stress) | Adaptar conforme tipo de queixa |

**Regras de feedback (F1-F7):**

| Regra | Situacao | Acao |
|-------|----------|------|
| F1 | Aderencia dieta < 100% | Perguntar QUAIS refeicoes e POR QUE antes de ajustar |
| F2 | Ganho > 2kg/mes (bulking) | Reduzir superavit em -5% |
| F3 | Perda < 0.5kg/mes (emag) | Verificar aderencia, depois aumentar deficit +10% |
| F4 | Dor muscular (DOMS) | Normal — NAO trocar exercicio |
| F5 | Dor articular | Substituir exercicio + recomendar fisioterapeuta |
| F6 | Fome apos ultima refeicao | Redistribuir carbs pro jantar, +fibras, +100kcal |
| F7 | Paciente acha que e IA | RED FLAG — resposta personalizada obrigatoria |

**Na renovacao, a IA:**
1. Recalcula macros com o PESO ATUALIZADO
2. Mantem alimentos que funcionaram
3. Troca alimentos que o paciente nao consumiu
4. Troca 30-50% dos exercicios isoladores
5. Varia metodos de intensificacao
6. Adapta a novas dores/lesoes reportadas

Apos gerar o novo plano, volta para a ETAPA 6 (revisao) antes de entregar.

> Arquivos: `decisao/renovacao-30dias.md`, `decisao/ajuste-feedback.md`, `correlacoes-feedback.md`

---

## Estrutura de Arquivos

```
ConsultorIA/
│
├── config.md ·················· Configuracao principal, regras, fluxo de coleta
├── ARQUITETURA.md ············· Diagrama visual do pipeline completo
│
├── engine/ ···················· Motor de calculos
│   ├── formulas.md ············ TMB, TDEE, macros, peso ajustado
│   ├── distribuicao.md ········ Distribuicao % por refeicao e horario
│   ├── suplementacao.md ······· Creatina, whey, vegan, hidratacao
│   ├── nutricao-avancada.md ··· Nutricao avancada
│   ├── nutricao-dia-off.md ···· Dias sem treino
│   ├── series-reps.md ········· Series e reps por objetivo
│   └── volume-semanal.md ······ Volume por grupo muscular
│
├── decisao/ ··················· Sistema de decisao
│   ├── triagem.md ············· 11 categorias + red flags + validacao IMC
│   ├── seguranca.md ··········· Medicacoes, condicoes cronicas, restricoes
│   ├── ajuste-feedback.md ····· Regras de ajuste por feedback
│   └── renovacao-30dias.md ···· Renovacao mensal + analise de fotos
│
├── database/ ·················· Bases de dados
│   ├── alimentos.json ········· 135 alimentos com macros (base TACO)
│   ├── substitutos.json ······· 135 substitutos (100% cobertura)
│   └── refeicoes-template.json  Templates por objetivo
│
├── treinos/ ··················· Sistema de treino
│   ├── exercicios.json ········ 152 exercicios
│   ├── divisoes.json ·········· 12 divisoes (3x-7x/sem + femininas)
│   ├── metodos.json ··········· 12 metodos de intensificacao
│   ├── adaptacoes-lesao.json ·· 16 lesoes mapeadas com adaptacoes
│   └── periodizacao.md ········ Ciclos de 4 semanas
│
├── output/ ···················· Templates de saida
│   ├── template-dieta.md ······ Template do plano alimentar
│   ├── template-treino.md ····· Template do plano de treino
│   ├── template-output.json ··· JSON para Google Sheets / n8n
│   └── exemplo-completo.md ···· Exemplos reais completos
│
├── arvore-decisao.md ·········· Macros por objetivo + divisoes + lesoes
├── analise-completa.md ········ Sumario executivo dos 3.106 registros
├── correlacoes-feedback.md ···· Analise de 1.137 feedbacks + regras F1-F7
├── raw-stats.md ··············· Estatisticas brutas
├── regras-calculo-nutricional.md  Regras de calculo com validacao
├── patterns-dieta.md ·········· Padroes de dieta identificados
├── patterns-treino.md ········· Padroes de treino identificados
└── tabela-alimentos.md ········ Tabela nutricional completa
```

---

## Dados de Origem

| Metrica | Valor |
|---------|-------|
| Consultorias analisadas | 3.106 |
| Planos novos | 1.969 |
| Feedbacks processados | 1.137 |
| Consultorias conectadas | 14 |
| Alimentos no banco | 135 |
| Exercicios no banco | 152 |
| Lesoes mapeadas | 16 |
| Metodos de treino | 12 |
| Divisoes de treino | 12 |

## Base Cientifica

- Harris-Benedict revisada (Roza & Shizgal, 1984)
- Mifflin-St Jeor (1990)
- Helms et al. (2014) — proteina em restricao calorica
- Kreider et al. (2017) — ISSN position stand creatina
- National Kidney Foundation — peso ajustado (ABW)

## Integracao

Projetado para funcionar com:
- **Discord** — canal dedicado com system prompt
- **n8n** — webhooks de entrada e saida
- **OpenClaw** — agente AI com acesso as bases

---

Desenvolvido por [Pedro Cruz](https://github.com/CruzAprc)
