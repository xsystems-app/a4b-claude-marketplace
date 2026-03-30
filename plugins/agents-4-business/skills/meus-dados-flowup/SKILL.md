---
name: Meus Dados FlowUp
description: >
  This skill should be used when the user asks about their OWN personal data
  from FlowUp. Triggers include:
  "minhas horas", "meu timesheet", "quantas horas eu lancei",
  "meu controle de horas", "quanto tempo eu trabalhei",
  "minhas tarefas", "meus tasks", "o que eu tenho pra fazer",
  "minhas atividades", "meus afazeres", "tarefas pendentes",
  "meu backlog", "o que está atribuído a mim",
  "minhas entregas", "meus prazos".
  Use this when the user refers to THEMSELVES (meu/minha/meus/minhas)
  in the context of FlowUp.
---

# Meus Dados FlowUp

## CRITICAL INSTRUCTIONS

This skill covers ALL FlowUp individual tools. These tools return ONLY the logged-in user's own data — no filters needed, the user is identified automatically by their login email.

You can call multiple tools to give a complete picture.

## AVAILABLE TOOLS

### 1. `consultar_minhas_horas_flowup` — Minhas Horas
My time entries — total hours and breakdown by project.
- `data_inicio` — start date YYYY-MM-DD (defaults to 1st of current month)
- `data_fim` — end date YYYY-MM-DD (defaults to today)

### 2. `consultar_minhas_tarefas_flowup` — Minhas Tarefas
My assigned tasks — project, status, priority, due date, progress.
- `data_inicio` — start date YYYY-MM-DD (optional)
- `data_fim` — end date YYYY-MM-DD (optional)
- `incluir_finalizadas` — `"true"` to include finished tasks (default: active only)

## CROSS-REFERENCING DATA

When the user asks compound questions, combine tools:

- **"minhas horas e tarefas pendentes"** → call both tools
- **"em quais projetos estou trabalhando e quantas horas?"** → call `consultar_minhas_horas_flowup` for hours by project + `consultar_minhas_tarefas_flowup` for active tasks
- **"resumo da minha semana"** → call both with current week dates

## PRESENTING RESULTS

**Minhas Horas:**
```
| Projeto         | Horas   |
|-----------------|---------|
| Obra Centro     | 80.00h  |
| Reforma Sala    | 62.50h  |
| **Total**       | **142.50h** |
```

**Minhas Tarefas:**
```
| Projeto         | Tarefa                  | Status       | Prioridade | Entrega    | Progresso |
|-----------------|-------------------------|--------------|------------|------------|-----------|
| Obra Centro     | Revisão estrutural      | Em Andamento | Alta       | 28/03/2026 | 60%       |
| Reforma Sala    | Orçamento materiais     | Pendente     | Média      | 02/04/2026 | 0%        |
```

Highlight overdue tasks (past due date). Group by project when there are many items.

## INTERPRETING PERIODS

- "minhas horas do mês" → 1st to last day of current month
- "minhas tarefas da semana" → Monday to today
- "meu timesheet de fevereiro" → 2026-02-01 to 2026-02-28
- "tarefas finalizadas" → set `incluir_finalizadas: "true"`

## PRESENTING RESULTS TO THE USER

The user is a regular employee checking their own data, NOT a developer. When presenting results:
- NEVER mention tool names, MCP servers, parameters, JSON, or any technical details in your response
- NEVER say "chamei a ferramenta X" or reference any technical process
- Present the data naturally — "Aqui estão suas horas do mês:", "Suas tarefas pendentes:"
- Go straight to the data — tables, totals, deadlines. No technical noise.
- Only mention technical details if the user explicitly asks how it works

## Do NOT:
- Use admin tools (consultar_horas_flowup, buscar_flowup) when the user asks about THEIR OWN data
- Ask for the employee name — tools find the user automatically
- Make up data
- Expose any technical details about tools, APIs, or MCP in your response
