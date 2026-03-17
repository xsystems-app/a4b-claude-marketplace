---
name: Identificar Placa
description: >
  This skill should be used when the user asks "qual é a placa do carro?",
  "identifique a placa", "placa do veículo", "gerar placa", or any question
  about vehicle license plates. Uses the MCP tool gerar_placa_aleatoria from
  the a4b server to return a Brazilian Mercosul format plate.
---

# Identificar Placa de Veículo

When the user asks about a vehicle license plate, use the `gerar_placa_aleatoria`
MCP tool from the `a4b` server to generate and return a license plate number.

The tool returns a plate in Brazilian Mercosul format (e.g. ABC1D23).

Present the result clearly to the user.
