# Template de Output — Dieta

## Formato de Saída

```
# 🥗 PLANO ALIMENTAR — {NOME}

**Data:** {DATA}
**Objetivo:** {OBJETIVO}
**Peso:** {PESO} kg | **Altura:** {ALTURA} cm | **Idade:** {IDADE} anos | **Sexo:** {SEXO}

---

## 📊 Resumo Nutricional

| Macro | Total Diário | g/kg |
|-------|-------------|------|
| Calorias | {KCAL} kcal | {KCAL_KG} kcal/kg |
| Proteína | {PROT}g | {PROT_KG} g/kg |
| Carboidrato | {CARB}g | {CARB_KG} g/kg |
| Gordura | {GORD}g | {GORD_KG} g/kg |

**TMB:** {TMB} kcal (Harris-Benedict) | **TDEE:** {TDEE} kcal | **Ajuste:** {AJUSTE}%

---

## ☀️ Refeição 1 — Café da Manhã ({HORARIO_1})
**Meta:** {KCAL_1} kcal | P: {PROT_1}g | C: {CARB_1}g | G: {GORD_1}g

| Alimento | Quantidade | Substituto |
|----------|-----------|------------|
| {ALIMENTO_1} | {GRAMAS_1}g ({MEDIDA_1}) | {SUBSTITUTO_1} |
| {ALIMENTO_2} | {GRAMAS_2}g ({MEDIDA_2}) | {SUBSTITUTO_2} |
| ... | ... | ... |

**Macros da refeição:** P: {SOMA_P_1}g | C: {SOMA_C_1}g | G: {SOMA_G_1}g | {SOMA_KCAL_1} kcal

---

## 🍌 Refeição 2 — Lanche da Manhã ({HORARIO_2})
[mesmo formato]

---

## 🍚 Refeição 3 — Almoço ({HORARIO_3})
[mesmo formato]

---

## 🥜 Refeição 4 — Lanche da Tarde ({HORARIO_4})
[mesmo formato]

---

## 🍽️ Refeição 5 — Jantar ({HORARIO_5})
[mesmo formato]

---

## 🌙 Refeição 6 — Ceia ({HORARIO_6}) [se aplicável]
[mesmo formato]

---

## 💊 Suplementação
| Suplemento | Dose | Horário |
|-----------|------|---------|
| Creatina monohidratada | {DOSE_CREATINA}g | Café da manhã |
| Whey Protein | {DOSE_WHEY}g | {HORARIO_WHEY} |

---

## ✅ Verificação de Macros

| | Proteína | Carboidrato | Gordura | Kcal |
|---|---------|------------|---------|------|
| **Meta** | {PROT}g | {CARB}g | {GORD}g | {KCAL} |
| **Soma refeições** | {SOMA_P}g | {SOMA_C}g | {SOMA_G}g | {SOMA_KCAL} |
| **Diferença** | {DIFF_P}g | {DIFF_C}g | {DIFF_G}g | {DIFF_KCAL} |

---

## 📝 Observações
- Beber no mínimo {AGUA}L de água por dia
- Horários são sugestões — adaptar à rotina
- Substituições: trocar por equivalentes da mesma linha
- Temperos naturais liberados: sal (moderado), pimenta, limão, ervas
- {OBSERVACOES_ESPECIFICAS}
```

## Regras do Template
1. SEMPRE incluir tabela de verificação de macros
2. Diferença máxima permitida: ±30 kcal, ±3g prot, ±5g carb, ±2g gordura
3. SEMPRE incluir substitutos para cada alimento
4. Horários devem considerar timing de treino
5. Água: peso × 35ml (mínimo 2L, máximo 4L)

## Regras de Montagem (OBRIGATÓRIAS)

### Carboidrato padronizado almoço = jantar
- O carboidrato BASE do almoço e jantar DEVE ser o mesmo
- Ex: se almoço é arroz branco → jantar também é arroz branco
- Variações ficam nos substitutos

### Mínimo 3 substitutos por alimento
- Cada alimento principal deve ter 3 opções de substituição
- Formato: "Substituto 1 (Xg) / Substituto 2 (Xg) / Substituto 3 (Xg)"
- Gramagens equivalentes para manter macros

### 1 vegetal por refeição
- Colocar APENAS 1 item de vegetal/salada na dieta base
- Os demais vão como substituições
- ✅ "Brócolis 100g → Alface+tomate 130g / Abobrinha 120g / Couve-flor 100g"
- ❌ "Brócolis 100g + Alface 30g + Tomate 80g" (múltiplos itens = confuso)
