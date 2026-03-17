---
name: Listar Funcionários Flash
description: >
  This skill should be used when the user asks "listar funcionários",
  "funcionários flash", "quem trabalha na empresa", "lista de colaboradores",
  "mostrar empregados", "consultar funcionários", or any question about
  employees, staff, or team members from Flash Benefícios.
---

# Listar Funcionários Flash

## CRITICAL INSTRUCTIONS

You MUST call the `listar_funcionarios_flash` tool from the `a4b` MCP server. This is mandatory.

Do NOT:
- Try to list employees from memory or make up employee data
- Say you don't have access to employee information
- Ask the user to check another system
- Ask for unnecessary clarification when the user clearly wants to see employees

Do THIS instead:
1. Call the `listar_funcionarios_flash` tool
2. Present the results in a clean, readable table format

## Parameters (all optional)

- `search` — filter by name or email (e.g. "João", "maria@empresa.com")
- `status` — filter by status: `ACTIVE` or `INACTIVE`
- `page` — page number (default: 1)
- `limit` — items per page (default: 20, max: 100)

## Examples

- "lista os funcionários" → call tool with no parameters
- "funcionários ativos" → call tool with `status: "ACTIVE"`
- "buscar funcionário João" → call tool with `search: "João"`
- "próxima página de funcionários" → call tool with `page: 2`