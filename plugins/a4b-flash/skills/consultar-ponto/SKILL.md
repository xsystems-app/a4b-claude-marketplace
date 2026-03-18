---
name: Consultar Ponto Flash
description: >
  This skill should be used when the user asks "ponto", "controle de ponto",
  "quem bateu ponto", "registro de ponto", "faltas", "ausências",
  "quem faltou hoje", "frequência", "presença dos funcionários",
  "horários de entrada e saída", or any question about attendance,
  clock-in/out, absences, or presence tracking from Flash Benefícios.
---

# Consultar Ponto Flash

## CRITICAL INSTRUCTIONS

You MUST call the `consultar_ponto_flash` tool from the `a4b-flash` MCP server. This is mandatory.

## IMPORTANT BUSINESS RULE

**If the employee clocked in at least ONCE in the day, they are considered PRESENT for that day.**
This rule directly impacts benefit calculations — a present employee has the right to receive the daily benefit.

## CALLING THE TOOL

Call `consultar_ponto_flash` with:
- `data_inicio` — start date in YYYY-MM-DD format (defaults to start of current week)
- `data_fim` — end date in YYYY-MM-DD format (defaults to today)
- `funcionario` — filter by employee name, partial match (optional)

## PRESENTING RESULTS

### Summary Table (ALWAYS show first)

```
| #  | Funcionário          | Dias Úteis | Presenças | Ausências |
|----|----------------------|------------|-----------|-----------|
| 1  | Ana Paula Marques    | 5          | 4         | 1         |
| 2  | João Silva           | 5          | 5         | 0         |
```

### Detail Table (show when user asks about a specific employee or wants details)

```
| Data  | Status   | Batidas                        |
|-------|----------|--------------------------------|
| 10/03 | PRESENTE | 07:38, 13:22, 14:23, 16:35     |
| 11/03 | AUSENTE  | -                              |
| 12/03 | PRESENTE | 08:00, 12:00, 13:00, 17:00     |
```

After the tables, ALWAYS show:
- **Período**: start and end dates
- **Total funcionários**: count
- Highlight employees with absences

## EXAMPLES

- "quem faltou essa semana?" → call tool with current week dates, show summary highlighting absences
- "ponto da Ana Paula em março" → call tool with `data_inicio: "2026-03-01"`, `data_fim: "2026-03-31"`, `funcionario: "Ana Paula"`, show both summary and detail
- "controle de ponto do mês" → call tool with first and last day of current month
- "quem bateu ponto hoje?" → call tool with today's date as both start and end

## Do NOT:
- Make up attendance data
- Say you don't have access to attendance records
- Skip the summary table
- Show detail table for ALL employees unless explicitly asked (it gets too long)
