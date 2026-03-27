---
name: Meus Dados Flash
description: >
  This skill should be used when the user asks about their OWN personal data
  from Flash Benefícios. Triggers include:
  "meu ponto", "minhas batidas", "minha frequência", "eu bati ponto",
  "minhas férias", "quando são minhas férias", "meus dias de folga",
  "minhas verbas", "meu holerite", "meu contracheque", "minhas horas extras",
  "meu banco de horas", "meu demonstrativo", "minha folha",
  "meus dados", "meus registros".
  Use this when the user refers to THEMSELVES (meu/minha/meus/minhas).
---

# Meus Dados Flash

## CRITICAL INSTRUCTIONS

This skill covers ALL Flash individual tools. These tools return ONLY the logged-in user's own data — no employee filter is needed, the user is identified automatically by their login email.

Choose the right tool(s) based on what the user asks. You can call multiple tools to give a complete picture.

## AVAILABLE TOOLS

### 1. `consultar_meus_pontos_flash` — Meu Ponto
My attendance records — clock-in/out times, presence/absence.
- `data_inicio` — start date YYYY-MM-DD (defaults to start of current week)
- `data_fim` — end date YYYY-MM-DD (defaults to today)

### 2. `consultar_minhas_ferias_flash` — Minhas Férias
My vacation schedule.
- `year` — year to query (defaults to current)
- `month` — month 1-12 (defaults to current)

### 3. `consultar_minhas_verbas_flash` — Minhas Verbas / Meu Holerite
My payroll budgets — overtime, night differential, time bank, etc.
- `year` — year to query (defaults to current)
- `month` — month 1-12 (defaults to current)

## CROSS-REFERENCING DATA

When the user asks compound questions, combine tools:

- **"meu ponto e minhas férias desse mês"** → call both `consultar_meus_pontos_flash` + `consultar_minhas_ferias_flash`
- **"minhas faltas e horas extras"** → call `consultar_meus_pontos_flash` + `consultar_minhas_verbas_flash`
- **"um resumo dos meus dados"** → call all three tools

## PRESENTING RESULTS

**Meu Ponto:**
```
| Data  | Dia       | Status   | Batidas                    |
|-------|-----------|----------|----------------------------|
| 10/03 | segunda   | PRESENTE | 07:38, 13:22, 14:23, 16:35 |
| 11/03 | terça     | AUSENTE  | -                          |
```

Always show: nome, período, presenças/ausências.

## INTERPRETING PERIODS

- "meu ponto do mês" → 1st to last day of current month
- "meu ponto da semana" → Monday to today
- "meu holerite de fevereiro" → year current, month 2
- "minhas férias do ano" → query current year

## Do NOT:
- Use admin tools (consultar_ponto_flash, etc.) when the user asks about THEIR OWN data
- Ask for the employee name — tools find the user automatically
- Make up data
