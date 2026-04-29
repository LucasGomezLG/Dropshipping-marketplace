---
layout: default
title: "Finanzas y Unit Economics"
nav_order: 10
---

# 08 — Finanzas y Unit Economics

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, inversores, CFO/contador.

---

## Principio rector

**Unit Economics calibrados con escenarios honestos**, no con proyecciones optimistas para pitch deck. Si el escenario realista no cierra, el plan está mal y hay que rehacerlo.

---

## Variables del modelo

Tres escenarios de calibración. Cada variable tiene rango optimista / realista / pesimista.

| Variable | Optimista | Realista | Pesimista |
|---|---|---|---|
| Conversión visita → cotización | 8% | 5% | 3% |
| Conversión cotización → compra (particular) | 35% | 25% | 15% |
| Conversión cotización → compra (mayorista) | 60% | 45% | 30% |
| Ticket promedio particular | $200 USD | $150 USD | $100 USD |
| Ticket promedio mayorista | $3.000 USD | $2.000 USD | $1.200 USD |
| Frecuencia particular / año | 3 | 1,5 | 1 |
| Frecuencia mayorista / año | 8 | 5 | 3 |
| Fee neto efectivo | 18% | 14% | 10% |
| Comisión neta marketplace | 6% | 4% | 2% |
| CAC efectivo | $5 USD | $15 USD | $30 USD |
| Churn anual | 25% | 40% | 55% |

---

## LTV por perfil (escenario realista)

```
LTV_particular = $150 × 1,5 × 14% × (1 / 0,40) = $79 USD
LTV_mayorista  = $2.000 × 5 × 14% × (1 / 0,40) = $3.500 USD
LTV_vendedor_marketplace (GMV $500/mes) = $500 × 12 × 4% × (1 / 0,40) = $600 USD
```

**Conclusión clave:** los **mayoristas son el motor de rentabilidad**. LTV de un mayorista es **44× mayor** que un particular. Pero los **particulares son el filtro masivo de adquisición** (alto volumen, bajo ticket, leads baratos para identificar futuros mayoristas).

---

## Estrategia de precios

| Tipo de cliente | Fee logístico | Comisión marketplace |
|---|---|---|
| Particular | 14-18% sobre valor en origen | n/a |
| Mayorista | 10-14% (descuento por volumen escalonado) | n/a |
| Vendedor del marketplace | n/a | 8-12% según categoría |

### Descuento por volumen mayorista
| Volumen anual acumulado | Fee aplicado |
|---|---|
| $0 - $5.000 USD | 14% |
| $5.001 - $20.000 USD | 12% |
| $20.001 - $50.000 USD | 11% |
| > $50.000 USD | 10% |

---

## Stress Test de viabilidad (3 escenarios)

### Escenario Optimista
```
30 particulares × $30 margen = $900
+ 5 mayoristas × $400 margen = $2.000
+ GMV $20.000 USD × 6% neto = $1.200
= Total: $4.100 USD/mes
- Burn-rate: $1.200 USD/mes
= Margen neto: $2.900 USD/mes
```
**Modelo altamente rentable.**

### Escenario Realista
```
20 particulares × $21 margen = $420
+ 3 mayoristas × $300 margen = $900
+ GMV $5.000 USD × 4% neto = $200
= Total: $1.520 USD/mes
- Burn-rate: $1.200 USD/mes
= Margen neto: $320 USD/mes
```
**Modelo en break-even apretado, sin margen para escalar marketing pago.**

### Escenario Pesimista
```
10 particulares × $14 margen = $140
+ 1 mayorista × $200 margen = $200
+ GMV $1.500 USD × 2% neto = $30
= Total: $370 USD/mes
- Burn-rate: $1.200 USD/mes
= Margen neto: -$830 USD/mes
```
**Modelo en pérdida, requiere capital de runway adicional.**

---

## Implicancias del stress test

1. **El escenario realista es break-even apretado** ($320/mes). No deja colchón para imprevistos (errores de calibración del cotizador, devoluciones mayoristas, cambios regulatorios). **Hay que apuntar al escenario optimista como objetivo real, no como upside.**

2. **La rentabilidad escala fuerte con número de mayoristas.** 1 mayorista = 14 particulares en margen mensual. **La estrategia comercial debe priorizar la captación de mayoristas desde el día 1**, usando los particulares como filtro masivo de leads. Detalle en [13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md).

3. **El marketplace no es el motor**, es el imán. Genera $200-1.200 USD/mes según escenario. Suficiente para cubrir parte del burn-rate de infra, no para pagar sueldos.

---

## Punto de Equilibrio detallado

### Burn-rate Fase 1 (con asesor aduanero, sin sueldos founders)
| Concepto | Costo mensual |
|---|---|
| Infraestructura tech | $200-350 USD |
| APIs (AfterShip, WhatsApp, Pasarelas) | $50-150 USD |
| Asesor aduanero / contable medio tiempo | $500-1.500 USD |
| Mantenimiento sociedad (LLC + SRL) | $150-400 USD |
| Pauta publicitaria experimental | $100-300 USD |
| **Subtotal Fase 1** | **~$1.000-2.700 USD** |

Con sweat equity y founders sin sueldo, **burn-rate efectivo Fase 1: ~$1.000-1.500 USD/mes**.

### Mix mínimo para break-even
- 20 importaciones particulares × $21 USD = $420 USD
- 3 importaciones mayoristas × $300 USD = $900 USD
- $5.000 USD GMV marketplace × 4% neto = $200 USD
- **Total: $1.520 USD/mes** vs $1.200 USD de burn-rate.

**Es exigente pero realista** con la audiencia existente del founder.

---

## Proyección a 12 meses (escenario realista, captación gradual)

| Mes | Particulares activos | Mayoristas activos | GMV marketplace USD | Ingreso mensual USD | Margen neto USD |
|---|---|---|---|---|---|
| 1 | 5 | 0 | 0 | $105 | -$1.095 |
| 3 | 15 | 1 | $2.000 | $695 | -$505 |
| 6 | 25 | 3 | $5.000 | $1.625 | $425 |
| 9 | 35 | 5 | $10.000 | $2.535 | $1.335 |
| 12 | 50 | 8 | $15.000 | $3.650 | $2.450 |

**Break-even alcanzado en mes 6-7. Margen acumulado positivo en mes 9-10.**

---

## Por qué los mayoristas dominan el modelo financiero

### Cálculo comparativo (escenario realista)
- 1 mayorista promedio: $2.000 ticket × 5/año × 14% fee × 2,5 años = **$3.500 USD LTV**
- 1 particular promedio: $150 ticket × 1,5/año × 14% fee × 2,5 años = **$79 USD LTV**

**Para igualar el LTV de 1 mayorista necesito 44 particulares.** La estrategia comercial debe reflejar esto: priorizar canal B2B explícito desde el día 1.

### Implicancia operativa
- **No es eficiente** invertir en pauta publicitaria masiva para captar particulares.
- **Es muy eficiente** invertir tiempo del founder + asesor en outreach B2B y eventos de la industria.
- **Los particulares se captan orgánicamente** desde el contenido del founder y el marketplace; los mayoristas requieren funnel comercial específico.

---

## Sensibilidad a variables clave

### Si la conversión cotización → compra cae al 15% (particular)
Margen mensual escenario realista: $1.520 → $1.220. **Apenas cubre burn-rate.** Alarma.

### Si el churn anual sube al 55%
LTV mayorista cae de $3.500 a $2.545. **El motor se debilita.** Foco urgente en retención.

### Si el fee neto cae al 10%
Margen mensual escenario realista: $1.520 → $1.085. **Bajo break-even.** Recalibrar cotizador.

### Si el CAC sube a $30 USD por mala adquisición
LTV/CAC particular cae a 2,6× (margen). Sigue arriba del 2× mínimo, pero hay que volver a canal orgánico.

---

## Métricas a monitorear semanalmente

- **LTV/CAC por perfil de cliente.**
- **Margen post-envío por categoría** (alerta si < 12%).
- **Conversión visita → cotización → compra.**
- **GMV del marketplace.**
- **Tasa de fallback a manual del cotizador.**
- **Account expansion rate** (mayoristas que aumentan volumen mes sobre mes).

---

## Cuándo levantar capital

**No antes** de tener:
- 6 meses consecutivos de break-even o superávit.
- LTV/CAC sostenido por encima de 3× en mayoristas.
- 10+ mayoristas activos con frecuencia probada.
- Margen post-envío promedio ≥ 14% en al menos 5 categorías de NCM.

Si se levanta antes, se levanta sobre proyecciones, no sobre datos. Y se diluye participación a peor valuación. Detalle en [12 — Capital y Financiamiento](12-capital-y-financiamiento.md).

---

## Próximas lecturas

- [11 — Equipo y Operaciones](11-equipo-y-operaciones.md): detalle de costos por rol.
- [12 — Capital y Financiamiento](12-capital-y-financiamiento.md): cuándo y cuánto levantar.
- [13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md): cómo se captan los mayoristas que sostienen el modelo.
