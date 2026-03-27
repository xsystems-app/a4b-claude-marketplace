---
name: Consultar FlowUp
description: >
  This skill should be used when the user asks about anything related to
  FlowUp project management. Triggers include:
  "horas lançadas", "tempo trabalhado", "quantas horas", "timesheet",
  "controle de horas", "horas no flowup", "relatório de horas",
  "quem lançou horas", "horas do mês", "horas da semana",
  "projetos", "listar projetos", "tarefas", "buscar tarefas",
  "usuários do flowup", "detalhes da tarefa", "detalhes do projeto",
  "FlowUp", "gestão de projetos".
---

# Consultar FlowUp

## CRITICAL INSTRUCTIONS

This skill covers ALL FlowUp admin tools. Choose the right tool(s) based on what the user asks. You can combine tools for richer answers.

## AVAILABLE TOOLS

### 1. `consultar_horas_flowup` — Horas por Funcionário
Queries hours worked per employee for a period. Shows total hours and breakdown by project.
- `data_inicio` — start date YYYY-MM-DD (defaults to 1st of current month)
- `data_fim` — end date YYYY-MM-DD (defaults to today)
- `funcionario` — filter by employee name, partial match (optional)

### 2. `buscar_flowup` — Consultas Genéricas
Generic query tool for projects, tasks, and users.
- `acao` — the action to perform (REQUIRED):
  - `listar_usuarios` — list all active FlowUp users
  - `listar_projetos` — list projects (can filter by name)
  - `buscar_tarefas` — search tasks with filters
  - `detalhe_tarefa` — get task details by ID
  - `detalhe_projeto` — get project details by ID
  - `horas_usuario` — get hours for a specific user by ID
- `id` — required for detail actions
- `filtros` — JSON string with filters:
  - For `listar_projetos`: `{"name": "Obra Centro"}`
  - For `buscar_tarefas`: `{"projectId": 123, "userName": "Ana", "startDate": "2026-03-01", "endDate": "2026-03-31", "showFinished": true}`
  - For `horas_usuario`: `{"startDate": "2026-03-01", "endDate": "2026-03-31"}`

## CROSS-REFERENCING DATA

- **"horas por projeto e quem mais contribuiu"** → call `consultar_horas_flowup` for the overview
- **"tarefas atrasadas do projeto X"** → call `buscar_flowup` with `buscar_tarefas` and project filter
- **"quanto tempo o João gastou no projeto Y?"** → call `consultar_horas_flowup` filtered by name, or `buscar_flowup` with `horas_usuario`
- **"projetos ativos e horas de cada um"** → call `buscar_flowup` to list projects + `consultar_horas_flowup` for hours

## PRESENTING RESULTS

**Horas:**
```
| # | Funcionário       | Total Horas | Projetos                           |
|---|-------------------|-------------|------------------------------------|
| 1 | Ana Paula Marques | 142.5h      | Obra Centro (80h), Reforma (62.5h) |
| 2 | João Silva        | 168.0h      | Obra Norte (168h)                  |
```

After the table: período, total geral de horas, top contributors.

## INTERPRETING PERIODS

- "horas do mês" → 1st to last day of current month
- "horas da semana" → Monday to today
- "horas de março" → 2026-03-01 to 2026-03-31
- "horas do mês passado" → 1st to last day of previous month

## WORKFLOW FOR TASKS/PROJECTS

1. User asks about projects → use `listar_projetos`
2. User asks about tasks → use `buscar_tarefas`
3. User wants details → first list to find ID, then `detalhe_tarefa` or `detalhe_projeto`

## Do NOT:
- Make up FlowUp data
- Use individual tools (minhas horas, minhas tarefas) when the user asks as admin
- Confuse FlowUp tools with Flash tools
