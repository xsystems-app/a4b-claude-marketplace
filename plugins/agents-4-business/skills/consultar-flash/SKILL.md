---
name: Consultar Flash Benefícios
description: >
  This skill should be used when the user asks about anything related to
  Flash Benefícios, HR management, or employee administration. Triggers include:
  "funcionários", "colaboradores", "quem trabalha", "listar funcionários",
  "ponto", "controle de ponto", "quem bateu ponto", "faltas", "ausências", "frequência",
  "férias", "programação de férias", "quem está de férias",
  "verbas", "holerite", "contracheque", "horas extras", "banco de horas",
  "benefícios", "vale refeição", "VR", "VA", "vale transporte",
  "departamentos", "setores", "organograma",
  "cargos", "funções", "posições",
  "recarga", "calcular recarga", "valor do benefício", "desconto de faltas",
  "RH", "recursos humanos", "folha de pagamento", "Flash".
---

# Consultar Flash Benefícios

## CRITICAL INSTRUCTIONS

This skill covers ALL Flash Benefícios admin tools. Choose the right tool(s) based on what the user asks. You can and SHOULD call multiple tools when the question requires cross-referencing data.

## AVAILABLE TOOLS

### 1. `listar_funcionarios_flash` — Funcionários
Lists employees from Flash. Use for employee queries.
- `search` — filter by name or email (optional)
- `status` — `ACTIVE` or `INACTIVE` (optional)
- `page`, `limit` — pagination

### 2. `consultar_ponto_flash` — Ponto / Frequência
Queries attendance records for ALL employees. Shows clock-in/out times, presence and absence.
- `data_inicio` — start date YYYY-MM-DD (defaults to start of current week)
- `data_fim` — end date YYYY-MM-DD (defaults to today)
- `funcionario` — filter by employee name, partial match (optional)

**Business rule:** At least 1 clock-in = PRESENT for the day.

### 3. `consultar_ferias_flash` — Férias
Queries vacation schedules for all employees.
- `year` — year to query (defaults to current)
- `month` — month 1-12 (defaults to current)
- `employeeIds` — comma-separated IDs to filter (optional)

### 4. `consultar_verbas_flash` — Verbas / Holerite
Queries payroll budgets — overtime, night differential, time bank, etc.
- `year` — year to query (defaults to current)
- `month` — month 1-12 (defaults to current)
- `employeeIds` — comma-separated IDs to filter (optional)

### 5. `listar_beneficios_flash` — Benefícios da Empresa
Lists all company benefits (VR, VA, VT, etc.).
- `status` — `active` or `inactive` (optional)
- `nome` — filter by name (optional)

### 6. `listar_departamentos_flash` — Departamentos
Lists company departments.
- `page`, `limit` — pagination

### 7. `listar_cargos_flash` — Cargos
Lists job positions.
- `page`, `limit` — pagination

### 8. `calcular_recarga_flash` — Cálculo de Recarga de Benefícios
Calculates benefit recharge amounts based on attendance. **Requires the daily value.**
- `valor_diario_padrao` — default daily value in BRL (REQUIRED — ask if not provided)
- `valores_individuais` — JSON with individual values per employee name (optional)
- `ajustes` — JSON with absence adjustments per employee name (optional)
- `mes_referencia` — month 1-12 (defaults to current)
- `ano_referencia` — year (defaults to current)

## CROSS-REFERENCING DATA

When the user asks compound questions, combine tools:

- **"quem faltou e quanto custa?"** → call `consultar_ponto_flash` first, then `calcular_recarga_flash`
- **"funcionários do departamento X e suas férias"** → call `listar_funcionarios_flash` + `consultar_ferias_flash`
- **"ponto e horas extras de março"** → call `consultar_ponto_flash` + `consultar_verbas_flash`
- **"quem faltou e desconta da recarga"** → call `consultar_ponto_flash` to find absences, then `calcular_recarga_flash` with adjustments

## PRESENTING RESULTS

Always use tables. Key formats:

**Ponto:**
```
| # | Funcionário       | Dias Úteis | Presenças | Ausências |
|---|-------------------|------------|-----------|-----------|
| 1 | Ana Paula Marques | 22         | 20        | 2         |
```

**Recarga:**
```
| # | Funcionário       | Dias Úteis | Presenças | Faltas | Dias Pagos | Valor/Dia | Total     |
|---|-------------------|------------|-----------|--------|------------|-----------|-----------|
| 1 | Ana Paula Marques | 22         | 20        | 2      | 20         | R$ 35,00  | R$ 700,00 |
```

## HANDLING RECARGA ADJUSTMENTS

When the user says:
- "Ana apresentou atestado de 3 dias" → add to `ajustes` and recalculate
- "Desconsidere as faltas da Maria" → add ALL her absences as abono
- After any adjustment, call the tool AGAIN and show the updated table

## PRESENTING RESULTS TO THE USER

The user is a business person, NOT a developer. When presenting results:
- NEVER mention tool names, MCP servers, parameters, JSON, or any technical details in your response
- NEVER say "chamei a ferramenta X", "usei o consultar_ponto_flash", or reference any technical process
- NEVER show raw API responses, technical field names, or parameter names
- Go straight to the data — the user wants tables with results, not explanations about how you obtained them
- Only mention technical details if the user explicitly asks how it works

## Do NOT:
- Make up data from any tool
- Say you don't have access to Flash data
- Forget the daily recharge value once provided in the conversation
- Use individual tools (meus pontos, minhas férias, etc.) when the user asks as admin
- Expose any technical details about tools, APIs, or MCP in your response
