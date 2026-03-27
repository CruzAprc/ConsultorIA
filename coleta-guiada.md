# MODO B — Coleta Guiada Passo a Passo

> Usar APENAS quando os dados vierem INCOMPLETOS ou UM POR UM.
> Se recebeu anamnese completa via webhook, IGNORAR este arquivo e processar direto (MODO A).

## FLUXO DE COLETA

O agent pergunta UM dado por vez, valida, confirma e avança.

### PASSO 1/8: DADOS PESSOAIS
- Nome completo (mín 2 palavras)
- Data nascimento ou idade (14-80)
- Sexo (M/F)

### PASSO 2/8: ANTROPOMETRIA
- Peso atual (30-250kg)
- Altura (100-220cm)
- Calcular IMC imediatamente

### PASSO 3/8: OBJETIVO
- Texto livre → normalizar: Bulking / Cutting / Emagrecimento / Manutenção / Recomposição

### PASSO 4/8: TREINO
- Tempo de treino (<6m=Iniciante, 6m-2a=Intermediário, >2a=Avançado)
- Frequência semanal (1-7)
- Tempo por sessão (30-180 min)
- Horário (manhã/tarde/noite)
- Local (academia/casa)

### PASSO 5/8: SAÚDE
- Doenças crônicas
- Medicações
- Lesões

### PASSO 6/8: ALIMENTAÇÃO
- Restrições (lactose, glúten, vegetariano, vegano)
- Alimentos que não gosta
- Nº refeições por dia
- Horários

### PASSO 7/8: SUPLEMENTAÇÃO
- Suplementos atuais
- Uso de ergogênicos/anabolizantes

### PASSO 8/8: RESUMO + CONFIRMAÇÃO
Mostrar resumo completo e pedir confirmação antes de gerar.

## REGRAS
1. UM campo por vez
2. Validar ANTES de avançar
3. Confirmar cada dado
4. Resumo final OBRIGATÓRIO antes de gerar
5. Se faltar dado crítico, NÃO gerar
