---
title: Summit - Fluxo de Ligações (Simplificado)
---

# Summit — Fluxo de Ligações (Simplificado)

Fluxo resumido de roteamento e políticas (Jupiter / Vivo).

## Fluxo de saída (Jupiter > Inbound SSW ID 1)

- Bloqueio temporário: `11940087541`
- Se Fixo 11 → `policy 10`
- Se Fixo LDN → `policy 10`
- Se VC1 → `policy 10`
- Se VC2 → rota `11110001154` (Vivo_TR)
- Se VC3 → rota `11110001154` (Vivo_TR)

## Core Out (ID 10)

- Se RN de A ≠ `55648` (VIP) → `deny` (saem pelas operadoras SCM do Jupiter)
- Se RN de A = `55648` (VIP):
  - B fixo (LCL) → `policy 51`
  - B LDN → `policy 55`
  - B VC1 → `policy 52`
  - B VC2 → `policy 56`
  - B VC3 → `policy 57`

## Forward (selecionado)

- Forward LC (ID 51): RN B `55648` → `policy 11` (entrada); Vivo/Claro → rota `1151`
- Forward LD (ID 55): RN B `55648` → `policy 11`; outros → rota `1154`
- Forward VC1 (ID 52): Vivo VC1 → rota `1155`; outros → `1151`
- Forward VC2 (ID 56): Vivo LD sem CSP → rota `1153`
- Forward VC3 (ID 57): Vivo LD sem CSP → rota `1153`

## Fluxo de entrada (Vivo > Inbound ITX ID 2)

- Qualquer entrada → `policy 11`

## Rotas e IPs

- `1151`: Vivo SPO LC — 10.11.160.207
- `1152`: Vivo LD15 — 10.11.160.208
- `1153`: Vivo LD sem CSP — 10.11.160.209
- `1154`: Vivo TR — 10.11.160.210
- `1155`: Vivo VC1 — 10.11.160.211
