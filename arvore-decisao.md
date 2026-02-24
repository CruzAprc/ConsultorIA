# Árvore de Decisão: Anamnese → Prescrição

> **IMPORTANTE:** As 11 categorias abaixo são as mesmas de `decisao/triagem.md`. A normalização dos 60+ objetivos raw deve seguir o mapa de triagem.
> **IMPORTANTE:** Para pacientes com IMC > 30, usar **Peso Ajustado** para cálculo de macros (g/kg). Ver `engine/formulas.md` seção 8.

## 1. Objetivo → Macros (Regras Extraídas — 11 Categorias)

### Bulking (n=1127)
- **Proteína:** 2.12 g/kg (range: 0.80-3.02)
- **Gordura:** 0.96 g/kg (range: 0.44-1.50)
- **Carboidrato:** 5.52 g/kg (range: 2.77-9.37)
- **Kcal total:** 3149 (range: 2530-4000)
- **Kcal/kg:** 41.0

### Cutting (n=251)
- **Proteína:** 1.87 g/kg (range: 1.00-2.02)
- **Gordura:** 0.57 g/kg (range: 0.30-1.00)
- **Carboidrato:** 2.73 g/kg (range: 1.70-4.68)
- **Kcal total:** 2141 (range: 1350-2900)
- **Kcal/kg:** 24.7

### Definição muscular (n=229)
- **Proteína:** 1.86 g/kg (range: 1.00-2.02)
- **Gordura:** 0.63 g/kg (range: 0.40-1.00)
- **Carboidrato:** 3.47 g/kg (range: 2.00-6.01)
- **Kcal total:** 2505 (range: 1396-2929)
- **Kcal/kg:** 28.1

### Emagrecimento (n=847)
- **Proteína:** 1.61 g/kg (range: 0.60-2.02)
- **Gordura:** 0.53 g/kg (range: 0.22-1.00)
- **Carboidrato:** 2.55 g/kg (range: 0.84-4.36)
- **Kcal total:** 2013 (range: 1360-2795)
- **Kcal/kg:** 25.0

### Ganhar peso (n=7)
- **Proteína:** 2.00 g/kg (range: 2.00-2.01)
- **Gordura:** 1.02 g/kg (range: 1.00-1.12)
- **Carboidrato:** 6.31 g/kg (range: 5.27-9.09)
- **Kcal total:** 3124 (range: 3056-3200)
- **Kcal/kg:** 46.4

### Ganho de massa muscular (n=81)
- **Proteína:** 2.03 g/kg (range: 1.98-2.50)
- **Gordura:** 0.92 g/kg (range: 0.50-1.06)
- **Carboidrato:** 5.18 g/kg (range: 3.20-6.85)
- **Kcal total:** 3022 (range: 2186-3530)
- **Kcal/kg:** 39.4

### Ganho de peso (n=5)
- **Proteína:** 2.00 g/kg (range: 2.00-2.00)
- **Gordura:** 1.00 g/kg (range: 1.00-1.00)
- **Carboidrato:** 6.27 g/kg (range: 5.67-7.00)
- **Kcal total:** 3146 (range: 2900-3358)
- **Kcal/kg:** 45.4

### Hipertrofia (n=20)
- **Proteína:** 2.05 g/kg (range: 2.00-2.52)
- **Gordura:** 0.87 g/kg (range: 0.73-1.00)
- **Carboidrato:** 4.95 g/kg (range: 4.00-5.86)
- **Kcal total:** 3090 (range: 2231-3500)
- **Kcal/kg:** 37.0

### Perda de peso (n=194)
- **Proteína:** 1.60 g/kg (range: 0.60-2.01)
- **Gordura:** 0.51 g/kg (range: 0.27-0.90)
- **Carboidrato:** 2.57 g/kg (range: 1.32-3.73)
- **Kcal total:** 2051 (range: 1450-2670)
- **Kcal/kg:** 22.3

### Perder peso (n=79)
- **Proteína:** 1.53 g/kg (range: 0.20-2.01)
- **Gordura:** 0.53 g/kg (range: 0.05-1.00)
- **Carboidrato:** 2.49 g/kg (range: 0.31-3.64)
- **Kcal total:** 1952 (range: 1400-2911)
- **Kcal/kg:** 21.5

### Secar (n=6)
- **Proteína:** 1.92 g/kg (range: 1.50-2.00)
- **Gordura:** 0.63 g/kg (range: 0.45-0.80)
- **Carboidrato:** 3.70 g/kg (range: 2.90-4.76)
- **Kcal total:** 2560 (range: 2270-2900)
- **Kcal/kg:** 29.6

### Recomposição (n=4)
- **Proteína:** 2.00 g/kg (range: 2.00-2.00)
- **Gordura:** 0.69 g/kg (range: 0.56-0.80)
- **Carboidrato:** 3.62 g/kg (range: 3.30-3.90)
- **Kcal total:** 2678 (range: 2300-3050)
- **Kcal/kg:** 29.4

### Manutenção (sem dados suficientes — referência literatura)
- **Proteína:** 1.80 g/kg
- **Gordura:** 0.80 g/kg
- **Carboidrato:** 4.00 g/kg
- **Kcal total:** TDEE × 1.0
- **Kcal/kg:** ~33

## 2. Objetivo + Experiência → Divisão de Treino

### Bulking
- **Avançado:** UPPER/LOWER (27x), ABC (27x), ABCDE (10x)
- **Iniciante:** FULL BODY (37x), ABC (1x), AB (1x)
- **Intermediário:** UPPER/LOWER (79x), ABC (57x), AB (26x)

### Cutting
- **Avançado:** ABCDE (5x), ABC (1x), AB (1x)
- **Intermediário:** ABC (5x), AB (2x), UPPER/LOWER (1x)

### Definição muscular
- **Avançado:** UPPER/LOWER (12x), ABC (9x), AB (8x)
- **Iniciante:** FULL BODY (23x), ABC (6x), AB (5x)
- **Intermediário:** ABC (32x), AB (22x), ABCDE (12x)

### Emagrecimento
- **Avançado:** ABCDE (4x), AB (3x), ABC (3x)
- **Iniciante:** FULL BODY (98x), AB (2x), ABC (2x)
- **Intermediário:** FULL BODY (89x), AB (32x), ABC (24x)

### Emagrecimento e Definição
- **Intermediário:** AB (1x)

### Emagrecimento e definição
- **Intermediário:** UPPER/LOWER (1x)

### Emagrecimento e ganho de massa muscular
- **Intermediário:** AB (1x)

### Emagrecimento e perda de gordura
- **Intermediário:** AB (1x)

### Ganho de massa magra
- **Intermediário:** ABC (1x)

### Ganho de massa muscular
- **Avançado:** ABC (7x), UPPER/LOWER (1x), ABCDE (1x)
- **Iniciante:** FULL BODY (7x), ABC (3x), ABCDE (1x)
- **Intermediário:** ABC (27x), AB (5x), ABCDE (5x)

### Ganho de massa muscular (Bulking)
- **Avançado:** ABCD (1x)
- **Intermediário:** AB (1x)

### Ganho de massa muscular e perda de gordura abdominal
- **Iniciante:** ABC (1x)

### Ganho de massa, bulking limpo
- **Avançado:** ABC (1x)

### Ganho de peso
- **Iniciante:** FULL BODY (2x)
- **Intermediário:** ABC (2x)

### Ganho de peso muscular
- **Iniciante:** FULL BODY (1x)

### Hipertrofia
- **Avançado:** ABC (4x)
- **Iniciante:** FULL BODY (3x)
- **Intermediário:** UPPER/LOWER (5x), ABC (4x), AB (2x)

### Hipertrofia (Bulking)
- **Intermediário:** ABC (1x)

### Hipertrofia e Força
- **Intermediário:** ABC (1x)

### Hipertrofia e definição
- **Avançado:** ABCD (1x)

### Hipertrofia e definição muscular
- **Avançado:** ABCDE (1x)

### Hipertrofia e diminuição de gordura abdominal
- **Avançado:** ABCD (1x)

### Hipertrofia e redução do BF
- **Avançado:** ABC (1x)

### Perda de gordura
- **Intermediário:** ABC (1x)

### Perda de peso
- **Avançado:** AB (1x)
- **Iniciante:** FULL BODY (2x)
- **Intermediário:** FULL BODY (5x), AB (3x), ABC (2x)

## 3. Frequência → Divisão

- **1x/semana:** A (7x), ABCDEF (2x), AB (1x)
- **2x/semana:** AB (8x), ABCDEF (1x)
- **3x/semana:** ABC (67x), ABCDEF (53x), ABCD (4x)
- **4x/semana:** ABCDEF (253x), ABCD (94x), ABCDE (6x)
- **5x/semana:** ABCDEF (774x), ABCDE (646x), ABCD (11x), ABC (4x), CDEF (2x)
- **6x/semana:** ABCDEF (957x), ABCDE (95x), ABC (1x)
- **7x/semana:** ABC (1x), ABCDE (1x)

## 4. Lesões → Adaptações

### Lesão no joelho (17 casos)
- Treino de força com foco em definição muscular, adaptando exercícios para evitar sobrecarga no joelho e considerando a rotina de treino em jejum.
- Treino de corpo inteiro com ênfase em exercícios de baixo impacto para as pernas, 6x/semana, considerando a lesão no joelho
- Treino de força com ênfase em exercícios de baixo impacto e cardio, evitando sobrecarga no joelho, 5x/semana

### Lesão no ombro (10 casos)
- Divisão ABC com inclusão de mais exercícios para grandes grupos musculares, respeitando a lesão no ombro
- Divisão ABC com ênfase em grandes grupos musculares, incluindo mais exercícios para costas e peito, respeitando a lesão no ombro.
- Divisão ABC com foco em definição muscular, respeitando a lesão no ombro e priorizando a execução correta dos exercícios.

### Lesão no ombro direito (9 casos)
- Divisão de treino focada em hipertrofia com atenção especial ao ombro, considerando 6x/semana
- Treino de força focado em hipertrofia, priorizando exercícios que não sobrecarreguem o ombro direito, com ênfase em peito e posterior, 3x por semana.
- Treino de força com foco em emagrecimento, evitando exercícios que sobrecarreguem o ombro direito, com ênfase em circuitos e cardio

### Lesão na lombar há mais de 2 anos (6 casos)
- Divisão A/B/C/D (5x/semana) com foco em hipertrofia, considerando a lesão na lombar e a necessidade de variações para evitar desconfortos
- Divisão A/B/C/D com foco em hipertrofia, monitorando a carga e volume para evitar sobrecarga na lombar
- Divisão A/B/C/D com foco em volume e intensidade, considerando a lesão na lombar e a necessidade de ganho muscular

### Lesão no calcanhar (6 casos)
- Treino de força com foco em emagrecimento, evitando exercícios que sobrecarreguem o calcanhar, com ênfase em musculação 6x/semana
- Divisão de treino focada em hipertrofia e perda de peso, com ênfase em exercícios para posterior, costa e ombro, respeitando a lesão no calcanhar.
- Divisão de treino atual parece adequada, mas considerar ajustes para evitar sobrecarga no calcanhar e incluir mais exercícios de recuperação.

### Lesão no ombro esquerdo (6 casos)
- Treino de força focado em hipertrofia, com atenção especial ao ombro esquerdo e variação de exercícios para evitar sobrecarga
- Divisão de treino focada em hipertrofia, priorizando exercícios para costas e evitando sobrecarga no ombro esquerdo, com ênfase em variações que não comprometam a articulação.
- Divisão A/B/C com foco em aumentar volume para peito e costas, ajustando a intensidade conforme necessário

### Tendinose no tendão rotuliano (5 casos)
- Treino de corpo inteiro 4x/semana com ênfase em exercícios que não sobrecarreguem o tendão rotuliano, focando na definição muscular
- Push/Pull/Legs (4x/semana) com adaptações para evitar sobrecarga no tendão rotuliano
- Push/Pull/Legs (4x/semana) com atenção especial à tendinose no tendão rotuliano

### Epicondilite no cotovelo (5 casos)
- Divisão de treino atual (Pernas, Peitoral, Ombros, Costas, Braços) está adequada para o objetivo de definição muscular, com atenção à epicondilite.
- Treino de força com foco em emagrecimento, evitando exercícios que possam agravar a epicondilite, com ênfase em treinos de corpo inteiro ou circuitos.
- Divisão de treino atual está adequada, focando em hipertrofia e definição muscular, com atenção à epicondilite.

### Rinite (5 casos)
- Treino de força com foco em emagrecimento (5x/semana), priorizando exercícios compostos e alta intensidade
- Divisão de treino focada em hipertrofia, priorizando exercícios compostos e isolados para ombros, costas e braços, com ênfase em aumento de calorias na dieta.
- Upper/Lower (4x/semana) com ênfase em costas e ombros para maximizar o ganho de massa muscular

### Dor no quadril (4 casos)
- Divisão A/B/C/D/E com foco em hipertrofia e definição, considerando a dor no quadril e a experiência inicial
- Treino A/B/C/D/E (5x/semana) com foco em definição muscular e atenção à dor no quadril
- Divisão A/B/C/D/E com foco em definição muscular, respeitando a dor no quadril e a frequência de 5x/semana

### Lesão recente no ombro esquerdo (lesão SLAP) (4 casos)
- ABC 2x por semana, com adaptações para evitar sobrecarga no ombro esquerdo
- ABC 2x/semana com adaptações para evitar sobrecarga no ombro esquerdo
- ABC2x (6x/semana) com adaptações para evitar sobrecarga no ombro esquerdo

### Dor no ombro direito (4 casos)
- Divisão de treino por grupos musculares (Peito, Costas, Pernas, Braços, Ombros) com foco em evitar sobrecarga no ombro e incluir treinos específicos para panturrilha.
- Divisão atual (Peito e Tríceps, Pernas, Ombros, Costas e Bíceps) está adequada, mas monitorar a dor no ombro direito e considerar ajustes se necessário.
- Treino de força com foco em emagrecimento, com atenção especial ao ombro direito, podendo incluir treinos de resistência e circuitos para otimizar a queima de gordura.

### Desconforto ocasional na lombar (4 casos)
- Divisão de treinos por grupos musculares, mantendo a frequência de 6x/semana para otimizar a definição muscular
- Divisão de treinos por grupos musculares, mantendo a estrutura atual, com inclusão do desenvolvimento de ombro no smith conforme solicitado.
- Divisão de treinos por grupos musculares, mantendo a estrutura atual, com foco em hipertrofia e definição muscular

### Dor na lombar (4 casos)
- Treino de força com foco em emagrecimento (4x/semana), com atenção especial à dor lombar
- Divisão de treino focada em hipertrofia, priorizando exercícios que não sobrecarreguem a lombar, com ênfase em peito, costas e trapézio.
- Treino em casa com foco em exercícios de peso corporal e resistência, adaptando para evitar sobrecarga na lombar.

### Dor no ombro esquerdo, lombar e glúteo esquerdo (4 casos)
- Divisão A/B/C (Peito/Costas/Pernas) com ênfase em costas, ajustando exercícios para minimizar dor no ombro
- Divisão de treino com foco em hipertrofia, evitando exercícios que sobrecarreguem os ombros, com ênfase em costas e exercícios que não agravem as dores.
- Push/Pull/Legs (5x/semana) com atenção especial ao ombro esquerdo e foco em ganho de massa muscular

### Histórico de lesão no joelho (4 casos)
- Treino de corpo inteiro (Full Body) 3x a 5x/semana, com foco em exercícios de baixo impacto para respeitar a lesão no joelho e promover emagrecimento.
- Treino de força com foco em emagrecimento, priorizando exercícios de baixo impacto para evitar sobrecarga no joelho, com ênfase em circuitos e treinamento funcional.
- Treino de corpo inteiro (Full Body) 3x a 6x/semana, com foco em emagrecimento e definição muscular, respeitando a recuperação do joelho.

### Lesão no joelho esquerdo (4 casos)
- Treino de força com ênfase em emagrecimento e cuidado com o joelho (exercícios de baixo impacto e fortalecimento muscular) 5x/semana
- Treino de força com foco em emagrecimento, priorizando exercícios de baixo impacto para evitar sobrecarga no joelho, com ênfase em circuitos e treinamento funcional.
- Treino de corpo inteiro 3x/semana com foco em exercícios de baixo impacto para evitar sobrecarga no joelho

### Nunca teve lesão (4 casos)
- Treino dividido em A/B/C/D/E, focando em hipertrofia com ênfase em peitoral e quadríceps, adequado para 5x/semana
- Divisão de treino atual está adequada para bulking, com ênfase em hipertrofia e aumento de carga progressiva.
- Divisão atual (Peito e tríceps, costas e bíceps, perna completa) com foco em hipertrofia para os grupos musculares de interesse, ajustando a intensidade e volume conforme necessário para emagrecimento

### Lesão no pé (4 casos)
- Treino de força focado em hipertrofia, com divisão de grupos musculares (ex.: Push/Pull/Legs) 6x/semana, considerando a experiência e o objetivo de bulking.
- 
- Treino de força com foco em emagrecimento, priorizando exercícios de baixo impacto para evitar sobrecarga na lesão do pé, 6x/semana

### Lesão entre o fêmur e o acetábulo (4 casos)
- Treino de força focado em definição muscular, com ênfase em exercícios compostos e isolados, adaptando para a lesão preexistente.
- Treino de força com foco em hipertrofia e definição, com variações de exercícios para evitar a monotonia e respeitando a lesão preexistente.
- Treino de força com foco em definição muscular, adaptado para evitar sobrecarga na lesão do fêmur, com ênfase em exercícios compostos e variações para abdômen, braços, costas e coxas.

## 5. Regras de Creatina

Baseado na análise dos dados:
- **0g/dia:** 1 prescrições, peso médio 79kg (range: 79-79kg)
- **3g/dia:** 62 prescrições, peso médio 71kg (range: 46-118kg)
- **4g/dia:** 2 prescrições, peso médio 70kg (range: 63-77kg)
- **5g/dia:** 2415 prescrições, peso médio 87kg (range: 47-949kg)
- **6g/dia:** 58 prescrições, peso médio 84kg (range: 58-126kg)
- **7g/dia:** 23 prescrições, peso médio 89kg (range: 61-116kg)
- **8g/dia:** 8 prescrições, peso médio 83kg (range: 65-95kg)
- **9g/dia:** 4 prescrições, peso médio 91kg (range: 70-104kg)
- **10g/dia:** 45 prescrições, peso médio 93kg (range: 60-168kg)
- **12g/dia:** 1 prescrições, peso médio 94kg (range: 94-94kg)