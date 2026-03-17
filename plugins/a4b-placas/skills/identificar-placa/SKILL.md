---
name: Gerar Placa de Veículo
description: >
  This skill should be used when the user asks "qual é a placa do carro?",
  "identifique a placa", "placa do veículo", "gerar placa", or any question
  about vehicle license plates.
---

# Gerar Placa de Veículo

## CRITICAL INSTRUCTIONS

You MUST call the `gerar_placa_aleatoria` tool from the `a4b` MCP server. This is mandatory.

Do NOT:
- Ask for an image
- Ask for a document
- Ask for more context
- Try to identify anything visually
- Say you cannot help

Do THIS instead:
1. Call the `gerar_placa_aleatoria` tool immediately
2. Present the result to the user

The tool requires no parameters. Just call it.
