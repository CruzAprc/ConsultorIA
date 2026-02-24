# ConsultorIA — Arquitetura Completa do Sistema

> **Base de dados:** 3.106 consultorias reais
> **11 categorias de objetivo** | **23 campos de anamnese** | **8 etapas de execucao**
> **Versao:** 1.0 | **Ultima atualizacao:** 2026-02-23

---

## Visao Geral do Pipeline

```
                           CONSULTORIA
                               |
       ========================|========================
       |                       |                       |
   ENTRADA              PROCESSAMENTO              SAIDA
   (Etapas 1-2)         (Etapas 3-6)            (Etapas 7-8)
       |                       |                       |
   Anamnese              Calculos                  Output
   Triagem               Dieta                   Renovacao
                         Treino
                         Revisao
```

---

## Pipeline Completo — 8 Etapas

```
  ETAPA 1          ETAPA 2          ETAPA 3          ETAPA 4
 ___________      ___________      ___________      ___________
|           |    |           |    |           |    |           |
| ANAMNESE  |--->| TRIAGEM   |--->| CALCULOS  |--->|   DIETA   |
|___________|    |___________|    |___________|    |___________|
                                                        |
                                                        v
  ETAPA 8          ETAPA 7          ETAPA 6          ETAPA 5
 ___________      ___________      ___________      ___________
|           |    |           |    |           |    |           |
| RENOVACAO |<---| OUTPUT    |<---|  REVISAO  |<---|  TREINO   |
|___________|    |___________|    |___________|    |___________|
```

---

## ETAPA 1 — Anamnese

> **Arquivo:** `config.md` (Fluxo de Coleta)
> **Modos:** A (dados completos) | B (passo a passo)

### Campos Obrigatorios (23 campos)

```
DADOS PESSOAIS              ANTROPOMETRIA           OBJETIVO
 |-- Nome completo           |-- Peso (kg)           |-- Texto livre
 |-- Data nascimento          |-- Altura (cm)         |-- Normalizado p/ 11 cat.
 |-- Sexo (M/F)              |-- IMC (calculado)

EXPERIENCIA                 SAUDE                   ALIMENTACAO
 |-- Tempo de treino          |-- Doencas cronicas    |-- Restricoes
 |-- Frequencia semanal       |-- Medicacoes          |-- Nao gosta
 |-- Tempo por sessao         |-- Lesoes/limitacoes   |-- Obrigatorios
 |-- Horario do treino                                |-- N. refeicoes
                                                      |-- Horarios
SUPLEMENTACAO               DETALHES TREINO          |-- Pref. 1a refeicao
 |-- Suplementos atuais       |-- Academia/casa
 |-- Ergogenicos (S/N)        |-- Grupo prioritario
                              |-- Hora que acorda
```

### Validacoes por Campo

| Campo | Regra | Flag |
|-------|-------|------|
| Idade | 14-80 | Amarela se <16 ou >60 |
| Peso | 30-250kg | Confirmar se <40 ou >150 |
| Altura | 100-220cm | — |
| IMC | Calculado auto | Vermelha se >35 ou <16 |
| Frequencia | 1-7x/sem | Sugerir Full Body se 1-2x |
| Sessao | 30-180 min | — |
| Refeicoes | 3-6/dia | — |

### Resumo Final Obrigatorio

Antes de avancar para Etapa 2, exibir resumo completo e aguardar confirmacao.

---

## ETAPA 2 — Triagem e Classificacao

> **Arquivo:** `decisao/triagem.md`

### 2.1 — Normalizacao de Objetivo (60+ raw -> 11 categorias)

```
SUPERAVIT                      DEFICIT                       NEUTRO
 |                              |                              |
 |-- Bulking (n=1127)           |-- Emagrecimento (n=848)      |-- Recomposicao (n=4)
 |   41.0 kcal/kg               |   25.0 kcal/kg               |   29.4 kcal/kg
 |                              |                              |
 |-- Ganho de massa (n=81)      |-- Cutting (n=251)            |-- Manutencao
 |   39.4 kcal/kg               |   24.7 kcal/kg               |   ~33 kcal/kg
 |                              |
 |-- Hipertrofia (n=20)         |-- Definicao (n=229)
 |   37.0 kcal/kg               |   28.1 kcal/kg
 |                              |
 |-- Ganhar peso (n=12)         |-- Perda de peso (n=194)
 |   45.9 kcal/kg               |   22.3 kcal/kg
 |                              |
 |                              |-- Perder peso (n=79)
 |                              |   21.5 kcal/kg
 |                              |
 |                              |-- Secar (n=6)
 |                                  29.6 kcal/kg
```

### 2.2 — Classificacao por Nivel

```
TEMPO DE TREINO         NIVEL              VOLUME/GRUPO         METODOS
                                           (series/sem)
< 6 meses        --->  INICIANTE     --->  10-12           ---> Convencional, Piramide
6m - 2 anos      --->  INTERMEDIARIO --->  14-18           ---> + Rest-pause, Drop-set
> 2 anos         --->  AVANCADO      --->  18-24           ---> + Bi-set, FST-7, Cluster
```

### 2.3 — Validacao Objetivo vs IMC

```
IMC < 18.5  --> Qualquer objetivo aceito + nota cautela se emag
IMC 18.5-25 --> ZERO ajuste, respeitar 100%
IMC 25-30   --> Bulking: superavit limitado +10-15%
IMC 30-35   --> Bulking: limitado +5-10% + nota sugerindo recomp
IMC > 35    --> Bulking: REDIRECIONAR p/ Recomposicao
IMC > 40    --> ENCAMINHAR MEDICO
```

### 2.4 — Red Flags

```
ENCAMINHAR MEDICO                    CAUTELA                    PADRAO
(nao prescrever)                     (prescrever com ajustes)   (seguir normal)

-- Doenca renal                      -- Diabetes T2 controlada  -- Sem condicoes
-- Diabetes T1 s/ controle           -- Hipotireoidismo         -- IMC 18.5-35
-- Cardiopatias graves               -- SOP                     -- Idade 16-55
-- IMC < 16                          -- Hipertensao             -- Sem lesoes
-- IMC > 40                          -- Lesoes ortopedicas
-- Idade < 14                        -- Idade > 55
-- Gestantes/lactantes               -- Medicamentos especificos
-- Transtornos alimentares
```

---

## ETAPA 3 — Calculos

> **Arquivo:** `engine/formulas.md`

### 3.1 — TMB (Taxa Metabolica Basal)

```
                    HARRIS-BENEDICT (primaria)
                    |
                    |  Homem: 88.362 + (13.397 x peso) + (4.799 x altura) - (5.677 x idade)
                    |  Mulher: 447.593 + (9.247 x peso) + (3.098 x altura) - (4.330 x idade)
                    |
                    v
COMPARAR  <------  MIFFLIN-ST JEOR (validacao)
                    |
                    |  Homem: (10 x peso) + (6.25 x altura) - (5 x idade) + 5
                    |  Mulher: (10 x peso) + (6.25 x altura) - (5 x idade) - 161
                    |
                    v
              DIFERENCA <= 10%?
              |            |
             SIM          NAO
              |            |
        Usar H-B     Media ponderada
                     60% Mifflin + 40% HB
```

### 3.2 — TDEE

```
TMB  x  FATOR DE ATIVIDADE  =  TDEE

Fator de Atividade:
  1.200 --> Sedentario (escritorio, sem exercicio)
  1.375 --> Levemente ativo (1-3x/sem)
  1.550 --> Moderadamente ativo (3-5x/sem)
  1.725 --> Muito ativo (6-7x/sem)
  1.900 --> Extremamente ativo (atleta, 2x/dia)
```

### 3.3 — Ajuste Calorico

```
TDEE  x  AJUSTE  =  KCAL ALVO

  Bulking:        +15% a +20%
  Cutting:        -15% a -20%
  Emagrecimento:  -20% a -25%
  Manutencao:      0%
  Recomposicao:   -5% a -10%
```

### 3.4 — Peso Ajustado (se IMC > 30)

```
IMC > 30?
  |
 SIM
  |
  v
Peso_ideal = 22 x (altura_m)^2
Peso_ajustado = Peso_ideal + 0.25 x (Peso_real - Peso_ideal)

  --> TMB: usa peso REAL
  --> Macros (g/kg): usa peso AJUSTADO
  --> Hidratacao: usa peso REAL
  --> Creatina: usa peso AJUSTADO
```

### 3.5 — Macros (g/kg de peso corporal)

```
HOMEM
                    Proteina    Gordura     Carboidrato
Bulking             2.0-2.2     0.8-1.0     5.0-6.0
Cutting             1.8-2.0     0.5-0.7     2.5-3.0
Emagrecimento       1.6-1.8     0.5-0.6     2.0-2.8
Manutencao          1.8-2.0     0.7-0.9     3.5-4.5
Recomposicao        2.0         0.6-0.8     3.0-4.0

MULHER
                    Proteina    Gordura     Carboidrato
Bulking             1.8-2.0     0.8-1.0     4.5-5.5
Cutting             1.8-2.0     0.6-0.8     2.0-2.5
Emagrecimento       1.5-1.8     0.5-0.7     1.8-2.5
Manutencao          1.6-1.8     0.7-0.9     3.0-4.0
Recomposicao        1.8-2.0     0.6-0.8     2.5-3.5

Conversao: Prot 1g=4kcal | Carb 1g=4kcal | Gord 1g=9kcal
```

### 3.6 — Distribuicao por Refeicoes

> **Arquivo:** `engine/distribuicao.md`

```
KCAL ALVO  --->  Dividir entre refeicoes (3 a 6)

Exemplo 5 refeicoes:
  Cafe............ 20%
  Lanche manha.... 10%
  Almoco.......... 30%
  Pre/pos treino.. 15%
  Jantar.......... 25%
```

---

## ETAPA 4 — Montagem da Dieta

> **Arquivos:** `database/alimentos.json`, `database/substitutos.json`, `database/refeicoes-template.json`
> **Regras:** `config.md` (Regras de Montagem da Dieta)

### Fluxo

```
MACROS POR REFEICAO
        |
        v
SELECIONAR ALIMENTOS -----> Respeitar restricoes (lactose, gluten, vegan)
        |                   Excluir "nao gosta"
        |                   Incluir obrigatorios
        v
CALCULAR GRAMAGENS -------> Atingir macros da refeicao
        |
        v
ATRIBUIR SUBSTITUTOS -----> Minimo 3 substitutos por alimento
        |                   Com gramagens equivalentes
        v
SUPLEMENTACAO ------------> Creatina, Whey, etc. (engine/suplementacao.md)
```

### Regras Obrigatorias

```
REGRA 1: Carb do almoco = Carb do jantar (mesmo alimento)
         Variacoes vao nos SUBSTITUTOS

REGRA 2: Minimo 3 substitutos por alimento
         Formato: "Arroz 270g -> Integral 270g / Macarrao 250g / Batata doce 350g"

REGRA 3: 1 vegetal principal por refeicao
         Outros vao como substituicoes
         "Brocolis 100g -> Alface+tomate 130g / Abobrinha 120g / Couve-flor 100g"
```

---

## ETAPA 5 — Montagem do Treino

> **Arquivos:** `treinos/divisoes.json`, `treinos/exercicios.json`, `treinos/metodos.json`
> **Adaptacoes:** `treinos/adaptacoes-lesao.json`

### Fluxo

```
NIVEL + FREQUENCIA + OBJETIVO
        |
        v
SELECIONAR DIVISAO
  |
  |  1-2x/sem --> Full Body
  |  3x/sem   --> ABC
  |  4x/sem   --> ABCD / Upper-Lower
  |  5x/sem   --> ABCDE
  |  6x/sem   --> ABCDEF / ABC 2x
  |
  v
MONTAR EXERCICIOS POR GRUPO
  |
  |  Compostos primeiro (base)
  |  Isoladores depois (complemento)
  |  Grupo prioritario = +2-3 series
  |
  v
VERIFICAR CONTRA LESOES
  |
  |  Joelho --> Evitar impacto, preferir maquinas
  |  Ombro  --> Evitar overhead livre, usar maquinas
  |  Lombar --> Remover terra/agachamento livre
  |
  v
APLICAR METODOS
  |
  |  Iniciante:     Convencional, Piramide
  |  Intermediario: + Drop-set, Rest-Pause, Pico
  |  Avancado:      + Bi-set, FST-7, Cluster
  |
  v
DEFINIR SERIES x REPS x DESCANSO
  |
  |  Hipertrofia: 3-4 x 8-12 | 60-90s
  |  Forca:       4-5 x 4-6  | 120-180s
  |  Resistencia: 2-3 x 15-20| 30-45s
  |
  v
INCLUIR GLOSSARIO DE METODOS (obrigatorio)
```

---

## ETAPA 6 — Revisao Automatica

> **Regra:** Convergencia obrigatoria antes de entregar

### Checklist (TODOS devem passar)

```
CONVERGENCIA NUTRICIONAL (tolerancias)
  [  ] Soma kcal das refeicoes = kcal alvo ............. (+-30 kcal)
  [  ] Soma proteina das refeicoes = total calculado .... (+-3g)
  [  ] Soma carboidrato das refeicoes = total ........... (+-5g)
  [  ] Soma gordura das refeicoes = total ............... (+-2g)

RESTRICOES ALIMENTARES
  [  ] Nenhum alimento conflita com restricoes
  [  ] Nenhum alimento na lista "nao gosta"
  [  ] Se lactose: zero lacticinios com lactose
  [  ] Se vegetariano/vegano: zero proteina animal
  [  ] Se sem gluten: zero fontes de gluten

TREINO
  [  ] Nenhum exercicio conflita com lesoes
  [  ] Exercicios compativeis com nivel
  [  ] Frequencia bate com o informado
  [  ] Creatina: dose adequada ao peso

LOOP DE CORRECAO
  Se nao convergir:
    Iteracao 1-5 --> Ajustar gramagens
    Se nao convergir em 5 --> Rebalancear refeicoes do zero
  NUNCA entregar com "necessita ajuste"
```

---

## ETAPA 7 — Output

> **Templates:** `output/template-dieta.md`, `output/template-treino.md`
> **JSON:** `output/template-output.json`

### Canais de Entrega

```
                    APROVADO NA REVISAO
                           |
              +------------+------------+
              |                         |
              v                         v
       RESPOSTA NO CANAL           POST WEBHOOK (n8n)
       (visualizacao)              (integracao automatica)
                                        |
                                        v
                                   Google Sheets
                                   Automacoes
```

### Webhook

```
POST https://wb.caos.app.br/webhook/discord-message-received
Content-Type: application/json

Regras:
  -- SOMENTE apos revisao APROVADA
  -- NUNCA dados parciais
  -- JSON completo do template-output.json
```

---

## ETAPA 8 — Renovacao (30 dias)

> **Arquivo:** `decisao/renovacao-30dias.md`
> **Ajustes:** `decisao/ajuste-feedback.md`
> **Correlacoes:** `correlacoes-feedback.md`

### Entrada

```
PACIENTE ENVIA (dia 30):
  |-- Formulario de feedback
  |     |-- Aderencia ao treino (%)
  |     |-- Aderencia a dieta (%)
  |     |-- Dores / incomodos
  |     |-- Fome fora de hora
  |     |-- Rendimento percebido
  |     |-- Peso atual
  |
  |-- Fotos de evolucao
  |     |-- Frente
  |     |-- Costas
  |     |-- Lateral
  |
  |-- Peso atualizado
```

### Arvore de Decisao da Renovacao

```
PESO + FOTOS = ?
  |
  |-- Peso SUBIU + Fotos = mais GORDURA
  |     |-- Se bulking: reduzir superavit -5%, aumentar cardio
  |     |-- Se cutting/emag: aumentar deficit -5%, revisar aderencia
  |
  |-- Peso SUBIU + Fotos = mais MUSCULO
  |     |-- Se bulking: PERFEITO -- manter, ajustar macros p/ novo peso
  |     |-- Se cutting: converter p/ recomposicao
  |
  |-- Peso CAIU + Fotos = mais DEFINICAO
  |     |-- Se cutting/emag: PERFEITO -- manter, ajustar macros
  |     |-- Se bulking: PROBLEMA -- revisar aderencia, aumentar kcal
  |
  |-- Peso ESTAVEL + SEM MUDANCA visual
  |     |-- Plato provavel
  |     |-- Ajustar kcal +-10%
  |     |-- Trocar exercicios isoladores
  |     |-- Adicionar 1 dia de cardio
  |
  |-- Feedback NEGATIVO (dores, fome, sono, estresse)
        |-- Dores --> adaptar treino
        |-- Fome --> redistribuir carbs, +fibras, +100kcal
        |-- Sono ruim --> reduzir cafeina, +carbs no jantar
        |-- Estresse --> reduzir volume -20%
```

### Regras de Feedback (F1-F7)

```
F1  Aderencia dieta < 100%    -->  Perguntar QUAIS refeicoes e POR QUE
F2  Ganho > 2kg/mes (bulking) -->  Reduzir superavit -5%
F3  Perda < 0.5kg/mes (emag)  -->  Verificar aderencia, depois +deficit 10%
F4  Dor muscular (DOMS)       -->  Normal, NAO trocar exercicio
F5  Dor articular              -->  Substituir exercicio + fisioterapeuta
F6  Fome apos ultima refeicao -->  Redistribuir carbs, +fibras, +100kcal
F7  "Parece IA"               -->  RED FLAG -- resposta personalizada obrigatoria
```

---

## Mapa de Arquivos

```
consultoria-agent/
|
|-- config.md                          # Configuracao principal + fluxo
|-- ARQUITETURA.md                     # Este arquivo
|-- arvore-decisao.md                  # Macros por objetivo + divisoes + lesoes
|-- analise-completa.md                # Sumario executivo (3106 registros)
|-- correlacoes-feedback.md            # Analise de feedbacks + regras F1-F7
|-- raw-stats.md                       # Estatisticas brutas
|-- regras-calculo-nutricional.md      # Regras de calculo com validacao
|-- patterns-dieta.md                  # Padroes de dieta identificados
|-- patterns-treino.md                 # Padroes de treino identificados
|-- tabela-alimentos.md                # Tabela nutricional de alimentos
|
|-- engine/
|   |-- formulas.md                    # TMB, TDEE, macros, peso ajustado
|   |-- distribuicao.md                # Distribuicao % por refeicao
|   |-- suplementacao.md               # Creatina, whey, vegan
|   |-- nutricao-avancada.md           # Nutricao avancada
|   |-- nutricao-dia-off.md            # Nutricao em dias sem treino
|   |-- series-reps.md                 # Series e repeticoes por objetivo
|   |-- volume-semanal.md              # Volume semanal por grupo
|
|-- decisao/
|   |-- triagem.md                     # 11 categorias + red flags + IMC
|   |-- seguranca.md                   # Medicamentos, condicoes cronicas
|   |-- ajuste-feedback.md             # Regras de ajuste por feedback
|   |-- renovacao-30dias.md            # Sistema de renovacao mensal
|
|-- database/
|   |-- alimentos.json                 # Banco de alimentos com macros
|   |-- substitutos.json               # Mapa de substituicoes
|   |-- refeicoes-template.json        # Templates de refeicao
|
|-- treinos/
|   |-- exercicios.json                # Database de exercicios
|   |-- divisoes.json                  # Templates de divisao
|   |-- metodos.json                   # Metodos de intensificacao
|   |-- adaptacoes-lesao.json          # Adaptacoes por tipo de lesao
|   |-- periodizacao.md                # Periodizacao e progressao
|
|-- output/
|   |-- template-dieta.md              # Template de saida da dieta
|   |-- template-treino.md             # Template de saida do treino
|   |-- template-output.json           # JSON para Google Sheets/n8n
|   |-- exemplo-completo.md            # Exemplo completo de output
|   |-- teste-matheus-luchetti.md      # Teste real
```

---

## Fluxo de Dados Completo

```
    FORMULARIO                                              GOOGLE SHEETS
    (Typeform/JotForm)                                      (Registro)
         |                                                       ^
         v                                                       |
    +---------+     +---------+     +---------+     +---------+  |
    |         |     |         |     |         |     |         |  |
    | WEBHOOK |---->| DISCORD |---->| CONSUL- |---->| WEBHOOK |--+
    |  (n8n)  |     | CHANNEL |     | TORIA   |     |  (n8n)  |
    |         |     |         |     |         |     |         |
    +---------+     +---------+     +---------+     +---------+
                                         |
                          +--------------+--------------+
                          |              |              |
                      TRIAGEM       CALCULOS       REVISAO
                      (decisao/)    (engine/)    (convergencia)
                          |              |              |
                      11 categ.     TMB/TDEE      +-30kcal
                      Red flags     Macros        +-3g prot
                      Nivel         Distrib.      +-5g carb
                                    Peso adj.     +-2g gord
```

---

## Ciclo de Vida do Paciente

```
DIA 0                    DIA 1-29                  DIA 30

ANAMNESE                 EXECUCAO                  RENOVACAO
  |                        |                         |
  v                        v                         v
Triagem                  Paciente segue            Feedback
Calculos                 dieta + treino            Fotos
Dieta                                              Peso
Treino                                               |
Revisao                                              v
Output                                            Analise
  |                                               Novos calculos
  v                                               Nova dieta
Entrega via                                       Novo treino
Discord + Sheets                                    |
                                                    v
                                                  Entrega
                                                    |
                                               VOLTA P/ DIA 1
```

---

**Base cientifica:** Harris-Benedict (1984), Mifflin-St Jeor (1990), Helms et al. (2014)
**Base empirica:** 3.106 consultorias reais, 1.137 feedbacks processados
