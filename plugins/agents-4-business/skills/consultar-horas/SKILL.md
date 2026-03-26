---
name: Consultar Horas FlowUp
description: >
  This skill should be used when the user asks "horas lançadas", "tempo trabalhado",
  "quantas horas", "timesheet", "controle de horas", "horas no flowup",
  "horas do mês", "horas da semana", "quem lançou horas", "relatório de horas",
  or any question about time entries, hours worked, or timesheets from FlowUp.
---

# Consultar Horas FlowUp

## CRITICAL INSTRUCTIONS

You MUST call the `consultar_horas_flowup` tool from the `Agents 4 Business` MCP server. This is mandatory.

## CALLING THE TOOL

Call `consultar_horas_flowup` with:
- `data_inicio` — start date YYYY-MM-DD (defaults to 1st of current month)
- `data_fim` — end date YYYY-MM-DD (defaults to today)
- `funcionario` — filter by employee name, partial match (optional)

## PRESENTING RESULTS

### Summary Table (ALWAYS show first)

```
| #  | Funcionário          | Total Horas | Projetos                          |
|----|----------------------|-------------|-----------------------------------|
| 1  | Ana Paula Marques    | 142.5h      | Obra Centro (80h), Reforma (62.5h)|
| 2  | João Silva           | 168.0h      | Obra Norte (168h)                 |
```

After the table, ALWAYS show:
- **Período**: start and end dates
- **Total geral de horas**: sum across all employees
- Highlight top contributors

## INTERPRETING PERIODS

- "horas do mês" → 1st to last day of current month
- "horas da semana" → Monday to today
- "horas de março" → 2026-03-01 to 2026-03-31
- "horas do mês passado" → 1st to last day of previous month
- "horas da semana passada" → previous Monday to previous Sunday

## Do NOT:
- Make up hours or project data
- Say you don't have access to FlowUp data
- Skip the summary table
