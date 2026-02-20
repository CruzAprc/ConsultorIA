# Regras de Ajuste por Feedback

## 1. Ajuste de Calorias

### Quando AUMENTAR kcal
| Situação | Ajuste |
|----------|--------|
| Bulking: ganhou < 0.5kg/mês | +200 kcal (via carb) |
| Bulking: perdeu peso | +300-400 kcal (via carb + gordura) |
| Cutting: perdendo > 1kg/semana | +200 kcal (via carb) — perda muito rápida = catabolismo |
| Fome constante fora de hora | +100-200 kcal (via gordura + fibra) |
| Rendimento no treino caindo | +200 kcal (via carb pré/pós treino) |
| Fadiga/cansaço constante | +150-200 kcal |

### Quando DIMINUIR kcal
| Situação | Ajuste |
|----------|--------|
| Emagrecimento: perdeu < 0.5kg/mês | -200 kcal (via carb) |
| Emagrecimento: não perdeu nada em 2 semanas | -200-300 kcal (via carb) |
| Bulking: ganhando > 1.5kg/mês (excesso gordura) | -200 kcal (via carb) |
| Sem fome, sobrando comida | -100-200 kcal |

### Regras de segurança
- NUNCA reduzir abaixo de 1200 kcal (mulheres) ou 1500 kcal (homens)
- Máximo de ajuste por vez: ±300 kcal
- Aguardar mínimo 2 semanas entre ajustes
- Proteína NUNCA diminui — só aumenta ou mantém

## 2. Ajuste de Treino

### Quando mudar divisão
| Situação | Ação |
|----------|------|
| Mudou frequência semanal | Adaptar divisão à nova frequência |
| Estagnação > 4 semanas | Mudar exercícios (não necessariamente divisão) |
| Nova lesão | Adaptar exercícios do grupo afetado |
| Iniciante → 6 meses consistente | Progredir de Full Body para ABC |
| Intermediário → avançado | Progredir para ABCDE ou PPL |

### Progressão de carga
| Nível | Regra |
|-------|-------|
| Iniciante | Aumentar carga toda semana se completou todas as reps |
| Intermediário | Aumentar carga a cada 2-3 semanas |
| Avançado | Periodização (acumular → intensificar → deload) |

### Quando adicionar/trocar métodos
| Situação | Ação |
|----------|------|
| Estagnação muscular | Introduzir drop-set ou rest-pause |
| Falta de conexão mente-músculo | Introduzir pico de contração ou super slow |
| Quer mais volume sem mais tempo | Introduzir bi-set |
| Treino monótono (aderência caindo) | Trocar exercícios mantendo padrão de movimento |

## 3. Ajuste por Feedback Específico

### "Estou com muita fome"
1. Verificar se gordura está adequada (mín 0.5 g/kg)
2. Aumentar fibras (vegetais, aveia, feijão)
3. Redistribuir kcal (mais no horário de fome)
4. Se persistir: +100-200 kcal

### "Não consigo comer tudo"
1. Reduzir volume por refeição (mais refeições menores)
2. Incluir alimentos mais calóricos (pasta de amendoim, azeite, granola)
3. Permitir shakes (whey + banana + aveia + pasta amendoim)
4. Se persistir: -100-200 kcal e redistribuir

### "Dor durante exercício X"
1. Substituir exercício imediatamente
2. Consultar `treinos/adaptacoes-lesao.json`
3. Recomendar avaliação fisioterapêutica
4. Reduzir carga em 40% ao retornar

### "Não gosto de alimento X"
1. Substituir por equivalente da mesma faixa de macros
2. Consultar `database/substitutos.json`
3. Recalcular gramagem para manter macros

### "Mudei horário de treino"
1. Reajustar timing pré/pós treino
2. Consultar `engine/distribuicao.md` seção 4
3. Redistribuir carbs em torno do novo horário

### "Comecei/parei medicamento"
1. Verificar interações em `decisao/seguranca.md`
2. Ajustar conforme indicado
3. Se medicamento é novo e relevante: recalcular TDEE

## 4. Frequência de Revisão Recomendada
| Tipo | Frequência |
|------|-----------|
| Primeira revisão | 2-4 semanas após início |
| Revisões subsequentes | A cada 4 semanas |
| Ajustes urgentes (dor, lesão) | Imediato |
| Mudança de objetivo | Nova anamnese completa |
