---
name: Buscar no FlowUp
description: >
  This skill should be used when the user asks about FlowUp projects, tasks,
  users, or needs to search/query anything specific in FlowUp that is NOT
  about time entries. Examples: "listar projetos", "tarefas do projeto X",
  "quem são os usuários do flowup", "detalhes da tarefa 123",
  "buscar projeto Y", "tarefas em andamento".
---

# Buscar no FlowUp

## CRITICAL INSTRUCTIONS

You MUST call the `buscar_flowup` tool from the `a4b` MCP server. This is mandatory.

## CALLING THE TOOL

Call `buscar_flowup` with:
- `acao` — the action to perform (REQUIRED):
  - `listar_usuarios` — list all active FlowUp users
  - `listar_projetos` — list projects (can filter by name)
  - `buscar_tarefas` — search tasks with filters
  - `detalhe_tarefa` — get task details by ID
  - `detalhe_projeto` — get project details by ID
  - `horas_usuario` — get hours for a specific user by ID
- `id` — required for detail actions (detalhe_tarefa, detalhe_projeto, horas_usuario)
- `filtros` — JSON string with filters, varies by action:
  - For `listar_projetos`: `{"name": "Obra Centro"}`
  - For `buscar_tarefas`: `{"projectId": 123, "userName": "Ana", "startDate": "2026-03-01", "endDate": "2026-03-31", "showFinished": true}`
  - For `horas_usuario`: `{"startDate": "2026-03-01", "endDate": "2026-03-31"}`

## WORKFLOW

1. If the user asks about projects → use `listar_projetos`
2. If the user asks about tasks → use `buscar_tarefas` (or `detalhe_tarefa` if they have an ID)
3. If the user asks "quem usa o flowup" → use `listar_usuarios`
4. If the user asks about a specific project → first `listar_projetos` to find the ID, then `detalhe_projeto`

## Do NOT:
- Make up FlowUp data
- Confuse this with the Flash Benefícios tools
- Use this for time/hours queries (use `/consultar-horas` instead)
