---
layout: default
title: "Plan de Lanzamiento (Roadmap Operativo)"
nav_order: 11
---

# 09 — Plan de Lanzamiento (Roadmap Operativo)

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, Tech Lead, equipo de producto.

---

## Principio rector

**Validar antes de construir.** Cada fase tiene un KPI de cierre claro. Si una fase no alcanza su KPI, no se avanza a la siguiente — se itera el producto o se pivota la hipótesis.

---

## Resumen de Fases

| Fase | Mes | Producto principal | KPIs de cierre |
|---|---|---|---|
| **1 — MVP Cotizador + Tienda** | 0-3 | Cotizador Híbrido universal + tienda dropshipping curada + dual mode | ≥20 importaciones particulares + 3 mayoristas/mes |
| **1.5 — Marketplace Beta** | 3-6 | Marketplace con 20-50 vendedores invitados + comisión 8-12% | GMV ≥$5.000 USD/mes |
| **2 — Marketplace Abierto + ERP** | 6-12 | Vidriera abierta a inscripciones + ERP de stock + bot WhatsApp | GMV ≥$15.000 USD/mes; ≥30% de vendedores convierten a B2B |
| **3 — Lotes de Demanda (condicional)** | 12-24 | Worker Python + cola + cache + event sourcing | Fill Rate ≥75%; reducción costo unitario por kilo ≥20% |

---

## Fase 1 — MVP Cotizador + Tienda (mes 0-3)

### Objetivo
Validar que el cotizador automatizado realmente convierte y libera el tiempo del founder.

### Producto a construir
- **Landing pública** con cotizador embebido (Next.js + Vercel).
- **Cotizador Híbrido** con whitelist conservadora (5-7 categorías iniciales de NCM).
- **Toggle particular/mayorista** desde el primer touchpoint.
- **Tienda dropshipping** con catálogo curado de 50-150 SKUs (Airtable + Next.js).
- **Pagos integrados** (Mercado Pago + Stripe).
- **Tracking básico** (AfterShip).

### Modo asistido
**Durante los primeros 60 días el cotizador opera en modo asistido:** el cliente ve la cotización inmediata, pero el founder confirma manualmente antes de cobrar. Esto captura errores de calibración antes de quemar margen real. Detalle en [02 — Producto Cotizador](02-producto-cotizador.md).

### Backend operativo
- Supabase para datos críticos (cotizaciones, pedidos, clientes).
- Airtable para catálogo curado (mantenimiento ágil).
- WhatsApp Business API para soporte y notificaciones.

### KPIs de cierre Fase 1
- ≥20 importaciones particulares + 3 mayoristas/mes.
- Conversión visita → cotización ≥ 8%.
- Conversión cotización → compra ≥ 30% (particular), ≥ 50% (mayorista).
- Margen post-envío promedio ≥ 14%.
- Tasa de fallback a manual < 35%.
- Tasa de incidentes aduaneros < 5%.

**Si los KPIs no se alcanzan en 90 días: iterar el producto o pivotar la hipótesis. No avanzar a Fase 1.5.**

---

## Fase 1.5 — Marketplace Beta (mes 3-6)

### Objetivo
Validar que el marketplace genera tráfico orgánico y convierte vendedores a clientes B2B.

### Producto a construir
- **Marketplace beta** invitación-only, 20-50 vendedores curados (no abierto al público).
- **Comisión 8-12%** según categoría.
- **Split payment** vía Mercado Pago.
- **Verificación AFIP** padrón al alta del vendedor.
- **Panel de vendedor** con productos, pedidos, ventas.
- **Cotizador integrado** en el panel del vendedor (cross-sell automático).

### Captación inicial de vendedores
- Outreach a 100-200 vendedores expulsados o disconformes con ML.
- Aprovechamiento de la audiencia del founder (autos primero).
- Garantía de comisión congelada por 6 meses para early adopters.

### KPIs de cierre Fase 1.5
- GMV marketplace ≥ $5.000 USD/mes.
- ≥ 20 vendedores activos.
- Conversión vendedor marketplace → cliente cotizador ≥ 20% (a 60 días).
- Refund rate < 5%.
- Tasa de fraude < 1%.

---

## Fase 2 — Marketplace Abierto + ERP (mes 6-12)

### Objetivo
Escalar el marketplace a tráfico abierto y consolidar el lock-in B2B.

### Producto a construir
- **Marketplace abierto** a inscripciones públicas.
- **ERP de stock genérico** para vendedores (Supabase + Next.js).
- **Bot de WhatsApp** para gestión rápida de inventario.
- **Sello "Importador Verificado"** automatizado.
- **Sistema de microcréditos B2B** (escrutinio basado en historial de actividad).
- **Migración del catálogo curado** de Airtable a Postgres.
- **Cache Redis** para cotizaciones y catálogo.
- **Observabilidad completa** (Sentry + BetterStack).
- **Webservice padrón AFIP** integrado para validación automática.

### Equipo Fase 2
- **Sumar B2B Manager** ($600-1.200 USD/mes) para gestión de cuenta de mayoristas.
- Founder transiciona de "atender mayoristas" a "expansión de canal B2B" (eventos, alianzas).

### KPIs de cierre Fase 2
- GMV marketplace ≥ $15.000 USD/mes.
- ≥ 50 vendedores activos.
- ≥ 30% de vendedores convierten a B2B (clientes del cotizador).
- ≥ 10 mayoristas activos con frecuencia ≥ 3/año.
- Margen post-envío promedio ≥ 14% sostenido.
- Tasa de fallback a manual < 25%.
- LTV/CAC ≥ 3× sostenido.

---

## Fase 3 — Lotes de Demanda (mes 12-24, condicional)

### Objetivo
Optimizar el margen de flete consolidando pedidos en lotes virtuales.

### Producto a construir
- **Worker Python** (FastAPI + OR-Tools) para optimización bin-packing multi-dimensional.
- **Cola / orquestador** (Inngest, Trigger.dev o Temporal).
- **State machine** para ciclo de vida del lote (días-semanas).
- **Event sourcing inmutable** completo para auditoría aduanera.
- **Smart Routing dinámico** entre rutas (Miami / Shanghai / SP).

### Cuándo lanzar
**Esta fase es condicional.** Solo se construye si:
- El volumen de pedidos por categoría justifica consolidación (≥ 100 pedidos/mes en una ruta).
- Hay evidencia clara de que la consolidación reduce el costo unitario por kilo en ≥ 15%.
- El equipo tiene la capacidad operativa para soportar otro frente.

### KPIs de cierre Fase 3
- Fill Rate de lotes consolidados ≥ 75%.
- Reducción del costo unitario por kilo ≥ 20%.
- Tasa de incidentes aduaneros en lotes consolidados < 3%.

---

## Compromisos clave del plan (no negociables)

1. **Modo asistido los primeros 60 días** del cotizador automático.
2. **Whitelist conservadora** al inicio (5-7 categorías de NCM); expansión gradual mes a mes con datos de margen.
3. **Plan B2B explícito desde el día 1** ([13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md)).
4. **Aprovechar la audiencia de autos** como canal de entrada del cotizador (CAC orgánico real, no hipotético).
5. **Asesor aduanero medio tiempo desde el día 1** (no opcional).
6. **No saltarse fases.** Cada fase debe cumplir KPIs antes de avanzar.

---

## Riesgos del plan

- **Atraso de Fase 1.5 (Marketplace Beta):** probable atraso de 2-3 meses respecto al plan original (es un producto nuevo). Mitigación: empezar el outreach a vendedores en paralelo a Fase 1.
- **Sobre-construcción en Fase 2:** ERP + bot + microcréditos + AFIP + cache es mucho. Mitigación: priorizar ERP y bot; microcréditos pueden esperar a Fase 3.
- **Tentación de saltar al marketplace abierto antes de cumplir KPIs Fase 1.5:** mitigación clara — validar antes de abrir.

Detalle de riesgos en [10 — Riesgos y Mitigaciones](10-riesgos-y-mitigaciones.md).

---

## Próximas lecturas

- [02 — Producto Cotizador](02-producto-cotizador.md): detalle del componente principal de Fase 1.
- [11 — Equipo y Operaciones](11-equipo-y-operaciones.md): roles por fase.
- [13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md): plan paralelo al roadmap.
