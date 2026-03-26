---
name: Calcular Recarga de Benefício Flash
description: >
  This skill should be used when the user asks "calcular recarga", "recarga de benefício",
  "quanto cada funcionário vai receber", "programar recarga", "valor do benefício",
  "preparar recarga do próximo mês", "desconto de faltas no benefício",
  "calcular benefício", or any question about calculating, programming, or
  adjusting benefit recharge amounts for employees.
---

# Calcular Recarga de Benefício Flash

## CRITICAL INSTRUCTIONS

You MUST call the `calcular_recarga_flash` tool from the `Agents 4 Business` MCP server. This is mandatory.

## BEFORE CALLING THE TOOL

You need the **valor diário padrão** (default daily value in BRL). If the user has already told you this value in the current conversation, use it. If not, ask:

> "Qual o valor diário padrão por funcionário para a recarga? (ex: R$ 35,00)"

Once the user tells you, remember this value for the rest of the conversation. The user can change it at any time by saying something like "o valor diário é R$ 40".

## CALLING THE TOOL

Call `calcular_recarga_flash` with:
- `valor_diario_padrao` — the default daily value (REQUIRED)
- `valores_individuais` — JSON string if any employee has a different daily value. E.g. `{"Ana Paula": 40.00}`
- `ajustes` — JSON string if the user requested absence adjustments. E.g. `{"Ana Paula": {"dias": 3, "motivo": "atestado médico"}}`
- `mes_referencia` — reference month (defaults to current if not specified)
- `ano_referencia` — reference year (defaults to current if not specified)

## PRESENTING RESULTS

ALWAYS present results in this EXACT table format. Never change the columns or order:

```
| #  | Funcionário          | Dias Úteis | Presenças | Faltas | Abonos | Dias Pagos | Valor/Dia  | Total Recarga |
|----|----------------------|------------|-----------|--------|--------|------------|------------|---------------|
| 1  | Ana Paula Marques    | 22         | 20        | 2      | 0      | 20         | R$ 35,00   | R$ 700,00     |
| 2  | João Silva           | 22         | 22        | 0      | 0      | 22         | R$ 35,00   | R$ 770,00     |
```

After the table, ALWAYS show:
- **Período**: start and end dates
- **Total Geral**: sum of all recharges
- If any employee has adjustments (abonos), list them with the reason

## HANDLING ADJUSTMENTS

When the user says something like:
- "Ana Paula apresentou atestado de 3 dias" → add to `ajustes`: `{"Ana Paula Marques Canazart": {"dias": 3, "motivo": "atestado médico"}}`
- "João faltou por motivo justificado 2 dias" → add to `ajustes`: `{"João ...": {"dias": 2, "motivo": "falta justificada"}}`
- "Desconsidere as faltas da Maria" → add ALL her absences as abono

After any adjustment, call the tool AGAIN with the updated `ajustes` and re-display the full table.

## HANDLING INDIVIDUAL VALUES

When the user says:
- "Ana Paula recebe R$ 40 por dia" → add to `valores_individuais`: `{"Ana Paula Marques Canazart": 40.00}`
- "O valor do João é R$ 30" → add to `valores_individuais`

After any value change, call the tool AGAIN and re-display the full table.

## Do NOT:
- Make up attendance data or guess who was absent
- Skip showing the full table after adjustments
- Change the table format or column order
- Forget the daily value the user already provided
- Ask for the daily value again if already known in this conversation
