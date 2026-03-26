# Regras de Montagem de Dieta — ConsultorIA

## Regras Obrigatórias (aplicar em TODAS as consultas)

### 1. ARROZ = SEMPRE BRANCO (normal)
- Nunca prescrever arroz integral
- Sempre usar arroz branco comum
- Mesmo em dietas de emagrecimento/recomposição/deficit
- Se o cliente já come arroz branco, manter
- Não listar "arroz integral" como substituto

### 2. LEGUMES e SALADA = SEMPRE SEPARADOS
- Legumes (abobrinha, cenoura, brócolis, chuchu, beterraba etc.) = item separado com gramagem definida
- Salada (alface, rúcula, repolho, agrião etc.) = item separado
- NUNCA juntar na mesma linha (ex: ❌ "legumes + salada 200g")
- Sempre listar como itens distintos na refeição

### 3. SALADA = À VONTADE
- Salada verde (folhosas) é praticamente sempre **à vontade**
- Não colocar gramagem restritiva em salada (ex: ❌ "salada 100g" ou "salada 200g")
- Formato correto: "Salada à vontade (alface, rúcula, repolho etc.)"
- Calorias de salada verde são negligíveis e não entram no cálculo de macros

### 4. FLUXO OBRIGATÓRIO — Regra de 09/03
Para **TODA** consulta processada, o fluxo é SEMPRE:
1. **Calcular** — TMB, TDEE, macros, convergência
2. **Enviar JSON pro webhook** — `POST https://wb.caos.app.br/webhook/get-messages` via `curl`
3. **Responder no Discord** — Confirmação com resumo

**NUNCA** pular a etapa 2! Toda consulta DEVE gerar um `curl -X POST` com o JSON completo para o webhook.
Se o webhook falhar (HTTP != 200), avisar no Discord e tentar novamente.

### 5. FORMATO JSON DO WEBHOOK
O JSON enviado DEVE seguir este formato padrão (modelo de referência: Matheus Freire):

```json
{
  "id": "<UUID do cliente>",
  "submission_id": "<UUID da submission>",
  "conta": "<Nome da conta>",
  "cliente": "<Nome completo>",
  "email": "<email>",
  "telefone": "<telefone>",
  "data_geracao": "<YYYY-MM-DD>",
  "resumo_calculo": {
    "idade": <number>,
    "sexo": "M|F",
    "peso": <number>,
    "peso_desejado": <number>,
    "altura_cm": <number>,
    "imc": <number>,
    "classificacao_imc": "<string>",
    "biotipo": "<string>",
    "cintura_cm": <number>,
    "quadril_cm": <number>,
    "rcq": <number>,
    "rcq_classif": "<string>",
    "tmb_mifflin": <number>,
    "formula_usada": "<fórmula com cálculo>",
    "fator_atividade": "<valor + justificativa>",
    "tdee": <number>,
    "ajuste": "<% + justificativa>",
    "kcal_alvo": <number>,
    "kcal_kg": <number>,
    "nota": "<resumo caso>"
  },
  "condicoes_medicas": {
    "exames": ["<lista>"],
    "exames_nota": "<análise>",
    "anabolizantes": "<status + alertas>",
    "restricoes_alimentares": ["<lista>"],
    "doencas": "<string>",
    "medicamentos": "<string>",
    "lesoes": "<string>"
  },
  "macros": {
    "proteina_g": <number>,
    "proteina_gkg": <number>,
    "carboidrato_g": <number>,
    "carboidrato_gkg": <number>,
    "gordura_g": <number>,
    "gordura_gkg": <number>,
    "kcal_total": <number>,
    "nota": "<verificação convergência>"
  },
  "hidratacao_ml": <number>,
  "hidratacao_nota": "<cálculo>",
  "suplementacao": [
    {"nome": "<>", "dose": "<>", "timing": "<>", "nota": "<>"}
  ],
  "alertas_criticos": ["<lista de alertas com emojis>"],
  "dieta": {
    "num_refeicoes": <number>,
    "nota_geral": "<resumo>",
    "refeicoes": [
      {
        "nome": "<nome da refeição>",
        "horario": "<~HH:MM>",
        "percentual": "<% do total>",
        "kcal": <number>,
        "macros": {"P": <>, "C": <>, "G": <>},
        "opcao_principal": ["<item — P:Xg C:Xg G:Xg>"],
        "substitutos": ["<opções>"],
        "nota": "<observações>"
      }
    ]
  },
  "treino": {
    "frequencia": "<>",
    "nivel": "<>",
    "duracao": "<> min",
    "horario": "<>",
    "divisao": "<tipo split>",
    "nota": "<observações>",
    "dias": [
      {
        "dia": "<Letra — Nome (Dia da semana)>",
        "exercicios": [
          {
            "exercicio": "<nome>",
            "series": "<SxR>",
            "descanso": "<Xs>",
            "metodo": "<método>",
            "execucao": "<dica execução>"
          }
        ]
      }
    ],
    "cardio": {
      "tipo": "<>",
      "frequencia": "<>",
      "duracao": "<>",
      "intensidade": "<>",
      "nota": "<>"
    }
  },
  "observacoes": ["<lista final de observações com emojis de prioridade>"]
}
```

**Campos obrigatórios:** id, submission_id, conta, cliente, email, data_geracao, resumo_calculo, macros, hidratacao_ml, dieta, treino, observacoes.
**Campos opcionais:** condicoes_medicas (incluir se houver), exames, cardio.

---

*Atualizado: 2026-03-26 — por Pedro Cruz (adição regra webhook + formato JSON)*
