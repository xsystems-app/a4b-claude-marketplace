---
name: Consultar Férias Flash
description: >
  This skill should be used when the user asks "férias", "quem está de férias",
  "programação de férias", "vacation", "férias do mês", "férias de março",
  "quem vai tirar férias", or any question about vacations, time off,
  or leave schedules from Flash Benefícios.
---

# Consultar Férias Flash

## CRITICAL INSTRUCTIONS

You MUST call the `consultar_ferias_flash` tool from the `a4b` MCP server. This is mandatory.

Do NOT:
- Make up vacation data or guess who is on vacation
- Say you don't have access to vacation information
- Ask the user to check another system
- Ask for unnecessary clarification when the user clearly wants vacation info

Do THIS instead:
1. Determine the year and month from the user's question (default to current month/year if not specified)
2. Call the `consultar_ferias_flash` tool with the appropriate parameters
3. Present the results in a clean, readable table format with employee name, start date, end date, and status

## Parameters

- `year` (integer) — Year to query. Defaults to current year if not specified.
- `month` (integer, 1-12) — Month to query. Defaults to current month if not specified.
- `employeeIds` (string) — Comma-separated employee IDs to filter. Only use if the user asks about a specific person and you already have their ID from a previous query.

## Examples

- "quem está de férias?" → call tool with current year/month
- "férias de abril" → call tool with `month: 4` and current year
- "férias em janeiro de 2026" → call tool with `year: 2026, month: 1`
- "programação de férias do próximo mês" → call tool with next month's year/month
