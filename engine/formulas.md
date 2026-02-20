# Motor de Cálculo — Fórmulas

## 1. Taxa Metabólica Basal (TMB)

### 1.1 Harris-Benedict Revisada (Roza & Shizgal, 1984) — PRIMÁRIA

**Homem:**
```
TMB = 88.362 + (13.397 × peso_kg) + (4.799 × altura_cm) - (5.677 × idade)
```

**Mulher:**
```
TMB = 447.593 + (9.247 × peso_kg) + (3.098 × altura_cm) - (4.330 × idade)
```

> Margem de erro: ±213 kcal/dia (homens), ±201 kcal/dia (mulheres)
> Referência: Roza AM, Shizgal HM. "The Harris Benedict equation reevaluated." Am J Clin Nutr. 1984;40(1):168-82.

### 1.2 Mifflin-St Jeor (1990) — VALIDAÇÃO

**Homem:**
```
TMB = (10 × peso_kg) + (6.25 × altura_cm) - (5 × idade) + 5
```

**Mulher:**
```
TMB = (10 × peso_kg) + (6.25 × altura_cm) - (5 × idade) - 161
```

> Considerada mais precisa para populações modernas (Frankenfield et al., 2005)
> Referência: Mifflin MD et al. "A new predictive equation for resting energy expenditure." Am J Clin Nutr. 1990;51(2):241-7.

### 1.3 Regra de Uso
- Calcular AMBAS as fórmulas
- Se diferença ≤ 10%: usar Harris-Benedict
- Se diferença > 10%: usar média ponderada (60% Mifflin + 40% HB)
- Para obesos (IMC > 35): preferir Mifflin-St Jeor

## 2. Fator de Atividade Física

| Nível | Descrição | Fator |
|-------|-----------|-------|
| Sedentário | Escritório, sem exercício | 1.2 |
| Levemente ativo | Exercício leve 1-3x/sem | 1.375 |
| Moderadamente ativo | Exercício moderado 3-5x/sem | 1.55 |
| Muito ativo | Exercício intenso 6-7x/sem | 1.725 |
| Extremamente ativo | Atleta, treino 2x/dia | 1.9 |

### Mapeamento da base de dados:
| Frequência semanal | Nível de experiência | Fator recomendado |
|-------------------|---------------------|-------------------|
| 1-2x | Qualquer | 1.375 |
| 3x | Iniciante | 1.375 |
| 3x | Intermediário/Avançado | 1.55 |
| 4x | Iniciante | 1.55 |
| 4x | Intermediário/Avançado | 1.55 |
| 5x | Iniciante/Intermediário | 1.55 |
| 5x | Avançado | 1.725 |
| 6x | Iniciante/Intermediário | 1.725 |
| 6x | Avançado | 1.725 |
| 7x | Qualquer | 1.9 |

## 3. TDEE (Total Daily Energy Expenditure)

```
TDEE = TMB × Fator_de_Atividade
```

## 4. Ajuste Calórico por Objetivo

| Objetivo | Ajuste sobre TDEE |
|----------|-------------------|
| Bulking | +15% a +20% (superávit) |
| Cutting | -15% a -20% (déficit) |
| Emagrecimento | -20% a -25% (déficit) |
| Manutenção | 0% |
| Recomposição | -5% a -10% (déficit leve) |

### Validação com a Base Real (kcal/kg):
| Objetivo | Base real (kcal/kg) | Faixa esperada |
|----------|--------------------|----|
| Bulking | 41.0 | 38-45 ✓ |
| Cutting | 24.7 | 22-28 ✓ |
| Emagrecimento | 25.0 | 20-28 ✓ |
| Hipertrofia | 37.0 | 35-42 ✓ |
| Perda de peso | 22.3 | 18-25 ✓ |

## 5. Macros por Objetivo e Sexo (g/kg de peso corporal)

### HOMEM

| Objetivo | Proteína (g/kg) | Gordura (g/kg) | Carboidrato (g/kg) |
|----------|----------------|----------------|-------------------|
| Bulking | 2.0 - 2.2 | 0.8 - 1.0 | 5.0 - 6.0 |
| Cutting | 1.8 - 2.0 | 0.5 - 0.7 | 2.5 - 3.0 |
| Emagrecimento | 1.6 - 1.8 | 0.5 - 0.6 | 2.0 - 2.8 |
| Manutenção | 1.8 - 2.0 | 0.7 - 0.9 | 3.5 - 4.5 |
| Recomposição | 2.0 | 0.6 - 0.8 | 3.0 - 4.0 |

### MULHER

| Objetivo | Proteína (g/kg) | Gordura (g/kg) | Carboidrato (g/kg) |
|----------|----------------|----------------|-------------------|
| Bulking | 1.8 - 2.0 | 0.8 - 1.0 | 4.5 - 5.5 |
| Cutting | 1.8 - 2.0 | 0.6 - 0.8 | 2.0 - 2.5 |
| Emagrecimento | 1.5 - 1.8 | 0.5 - 0.7 | 1.8 - 2.5 |
| Manutenção | 1.6 - 1.8 | 0.7 - 0.9 | 3.0 - 4.0 |
| Recomposição | 1.8 - 2.0 | 0.6 - 0.8 | 2.5 - 3.5 |

> **Diferenças M vs F:**
> - Mulheres: gordura mínima mais alta (mínimo 0.5 g/kg) — essencial para saúde hormonal
> - Mulheres: carboidrato ligeiramente menor (menor massa muscular relativa)
> - Proteína similar em cutting (preservação muscular é prioridade em ambos)
> Referência: Helms ER et al. "A systematic review of dietary protein during caloric restriction." Int J Sport Nutr Exerc Metab. 2014;24(2):127-38.

### Validação cruzada com a base real:
| Objetivo | Base (Prot g/kg) | Fórmula (M) | Status |
|----------|-----------------|-------------|--------|
| Bulking | 2.12 | 2.0-2.2 | ✓ Dentro da faixa |
| Cutting | 1.87 | 1.8-2.0 | ✓ Dentro da faixa |
| Emagrecimento | 1.61 | 1.6-1.8 | ✓ Dentro da faixa |
| Hipertrofia | 2.05 | 2.0-2.2 | ✓ Dentro da faixa |

## 6. Conversão Macros → Calorias

```
Proteína: 1g = 4 kcal
Carboidrato: 1g = 4 kcal
Gordura: 1g = 9 kcal
```

### Verificação:
```
Kcal_total = (Proteína_g × 4) + (Carboidrato_g × 4) + (Gordura_g × 9)
```

## 7. Ajustes Especiais

### Por Idade
| Faixa | Ajuste |
|-------|--------|
| < 18 anos | +5% kcal (crescimento) |
| 18-39 | Padrão |
| 40-49 | -3% kcal |
| 50-59 | -5% kcal, proteína mín 1.8 g/kg |
| 60+ | -8% kcal, proteína mín 1.8 g/kg, evitar restrição calórica severa |

### Por Condição Médica
| Condição | Ajuste |
|----------|--------|
| Diabetes tipo 2 | Carb ≤ 3.0 g/kg, priorizar baixo IG |
| Hipotireoidismo | -10% TDEE estimado |
| Hipertireoidismo | +10% TDEE estimado |
| Doença renal | Proteína ≤ 0.8 g/kg — ENCAMINHAR MÉDICO |
| SOP (mulheres) | Carb ≤ 2.5 g/kg, priorizar baixo IG |

### Por IMC
| IMC | Consideração |
|-----|-------------|
| < 18.5 | Bulking obrigatório, mín +20% TDEE |
| 18.5-24.9 | Normal |
| 25-29.9 | Sobrepeso — se objetivo é bulking, limitar superávit a +10% |
| 30-34.9 | Obesidade I — déficit máx -25%, proteína alta |
| ≥ 35 | Obesidade II+ — usar Mifflin-St Jeor, encaminhar médico |
