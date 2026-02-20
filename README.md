# 🏋️ ConsultorIA

Sistema de IA para prescrição automática de **dieta + treino personalizado**, atendendo múltiplas consultorias fitness.

## O que faz

- Recebe dados de anamnese (23 campos) via webhook ou coleta guiada
- Calcula TMB (Harris-Benedict + Mifflin-St Jeor), TDEE e macros por sexo/objetivo
- Monta plano alimentar completo com substituições (135 alimentos, base TACO)
- Monta plano de treino personalizado (152 exercícios, 12 divisões, adaptações para lesões)
- Validação de convergência de macros (±30kcal, ±3g prot, ±5g carb, ±2g gord)
- Renovação automática a cada 30 dias com análise de feedback + fotos
- Triagem de segurança (IMC, condições médicas, medicações)

## Estrutura

```
├── config.md                    # Config principal, fluxo, regras, coleta passo-a-passo
├── engine/                      # Motor de cálculos
│   ├── formulas.md              # Harris-Benedict, Mifflin-St Jeor, TDEE, macros M/F
│   ├── distribuicao.md          # Distribuição por refeição e horário de treino
│   ├── suplementacao.md         # Creatina, whey, etc.
│   └── ...
├── database/                    # Bases de dados
│   ├── alimentos.json           # 135 alimentos com macros TACO
│   ├── substitutos.json         # 135 substitutos (100% cobertura)
│   └── refeicoes-template.json  # Templates por objetivo
├── treinos/                     # Sistema de treino
│   ├── exercicios.json          # 152 exercícios
│   ├── divisoes.json            # 12 divisões (3x-7x/semana + femininas)
│   ├── metodos.json             # 12 métodos de intensificação
│   ├── adaptacoes-lesao.json    # 16 lesões mapeadas
│   └── periodizacao.md          # Ciclos de 4 semanas
├── decisao/                     # Sistema de decisão
│   ├── triagem.md               # Normalização, classificação, validação IMC
│   ├── seguranca.md             # Medicações, condições, contraindicações
│   ├── ajuste-feedback.md       # Ajustes baseados em feedback
│   └── renovacao-30dias.md      # Renovação mensal + análise de fotos
├── output/                      # Templates de saída
│   ├── template-dieta.md        # Template do plano alimentar
│   ├── template-treino.md       # Template do plano de treino
│   ├── template-output.json     # Template JSON para integrações
│   └── exemplo-completo.md      # 2 exemplos: homem bulking + mulher emagrecimento
└── *.md                         # Análises, padrões e correlações extraídas de 3.106 registros reais
```

## Dados de origem

Construído a partir da análise de **3.106 registros reais** (1.969 planos novos + 1.137 feedbacks) de **14 consultorias fitness**.

## Regras críticas

1. **Convergência obrigatória**: macros devem bater (±30kcal, ±3g prot, ±5g carb, ±2g gord)
2. **Revisão 3 etapas**: gerar → validar → output (nunca entregar sem validação)
3. **Respeitar objetivo do usuário**: só intervir se IMC > 35 + bulking ou IMC > 40
4. **Fórmulas específicas por sexo** (M/F) para TMB e macros
5. **Hidratação**: 35ml/kg/dia mínimo

## Integração

Projetado para funcionar com:
- **Discord** (canal dedicado com system prompt)
- **n8n** (webhooks de entrada/saída)
- **OpenClaw** (agente AI com acesso às bases)

---

Desenvolvido por [Pedro Cruz](https://github.com/CruzAprc)
