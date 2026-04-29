---
layout: default
title: "Arquitectura Técnica"
nav_order: 8
---

# 06 — Arquitectura Técnica

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** Tech Lead, desarrolladores, asesores técnicos.

---

## Principio rector

**Stack escalonado:** la infraestructura se complejiza solo cuando la demanda lo justifica. Empezamos minimalista y agregamos componentes cuando hay datos que respaldan el costo.

No construimos para una escala que no existe todavía. Pero diseñamos los modelos de datos pensando en la escala futura para no reescribir cuando llegue.

---

## Stack Fase 1 (MVP — meses 0-6)

### Frontend
- **Next.js** sobre **Vercel**.
- ISR + CDN para catálogo público.
- SSR para páginas autenticadas (panel del cliente, panel del vendedor).
- Páginas: landing pública, cotizador, tienda dropshipping, marketplace beta, panel cliente, panel vendedor, panel admin.

### Backend & Base de Datos
- **Supabase** (PostgreSQL gestionado + Auth + Storage + Row Level Security).
- Tablas principales: `clients`, `quotations`, `orders`, `products`, `vendors`, `marketplace_listings`, `events_audit`.
- Auth con verificación de email + opcional verificación CUIT vía padrón AFIP.

### Microservicio cotizador
- **Node.js o Python** sobre **Railway / Fly.io**.
- Responsable del cálculo del Cotizador Híbrido (NCM + peso volumétrico + cobertura cambiaria + spread arancelario).
- Integración con APIs oficiales de proveedores (Amazon Product Advertising API, eBay Browse API, AliExpress Open Platform).
- Endpoint stateless, fácil de escalar horizontalmente.

### Pagos
- **Mercado Pago** (Argentina, particulares y vendedores del marketplace).
- **Stripe** (B2B internacional, cobro de fees logísticos a mayoristas).
- **Wise / Lemon Cash** (cripto-pasarelas para liquidación T+0 de cobertura cambiaria).

### Tracking y comunicación
- **AfterShip** plan inicial para tracking internacional (~$11 USD/mes inicial).
- **WhatsApp Business API** vía Twilio o Meta Cloud API (~$20-50 USD/mes inicial).

### Costo Fase 1: ~$200-350 USD/mes

| Concepto | Costo mensual |
|---|---|
| Vercel Pro | $20 USD |
| Supabase Pro | $25 USD |
| Railway / Fly.io (microservicio) | $20-50 USD |
| AfterShip | $11 USD |
| WhatsApp Business API | $20-50 USD |
| Mercado Pago / Stripe | % variable, no fijo |
| Dominio + Workspace | $15 USD |
| Sentry / observabilidad básica | $20 USD |
| **Subtotal Fase 1** | **~$130-190 USD fijos** |

(El resto es variable y depende de volumen.)

---

## Stack Fase 2 (Marketplace consolidado — meses 6-12)

Se suman:
- **Cache layer:** **Upstash Redis** para cotizaciones recurrentes y catálogo curado.
- **Observabilidad:** **Sentry** + **BetterStack** para métricas, logs y alertas.
- **ERP de stock para vendedores:** módulo construido sobre Supabase + Next.js. Vendedores cargan productos, reciben alertas de stock bajo, conectan al cotizador integrado para reposición.
- **Bot de WhatsApp** para gestión rápida de inventario por parte de vendedores.

### Costo Fase 2: ~$350-600 USD/mes

| Concepto adicional | Costo mensual |
|---|---|
| Upstash Redis | $10-30 USD |
| Sentry + BetterStack | $50 USD |
| Mailgun / Resend (transaccionales) | $20-50 USD |
| Plan Vercel/Supabase escalado | +$50-100 USD |
| **Subtotal Fase 2 (sumado a Fase 1)** | **~$350-500 USD** |

---

## Stack Fase 3 (Lotes de Demanda — meses 12-24, condicional)

El stack base **no resuelve** consolidación algorítmica de pedidos en lotes virtuales. Se agrega:

### Worker de optimización
- **Microservicio Python** (FastAPI + OR-Tools / Pyomo) sobre Railway o AWS ECS.
- Resuelve bin-packing multi-dimensional con restricciones aduaneras.
- Decide cuándo cerrar un lote (trade-off entre tiempo de espera del cliente y eficiencia logística).

### Cola de eventos / orquestador
- **Inngest, Trigger.dev o Temporal** para state machines y workflows de larga duración (días-semanas).
- Edge Functions de Supabase tienen timeout de 25s — insuficientes para workflows largos.

### Event sourcing inmutable
- Tabla `events_audit` append-only en Postgres con hash encadenado.
- Defensa legal ante eventual fiscalización AFIP / Aduana.
- Migración a **Materialize / ClickHouse** si volumen sube > 1M eventos/mes.

### Costo adicional Fase 3: +$200-500 USD/mes

| Concepto | Costo mensual |
|---|---|
| Worker Python (Railway/ECS) | $50-150 USD |
| Inngest / Trigger.dev | $20-100 USD |
| Cache adicional + escala BD | $50-150 USD |
| Almacenamiento de eventos | $20-100 USD |
| **Subtotal adicional Fase 3** | **~$140-500 USD** |

---

## Resumen de costos infra escalonados

| Fase | Burn-rate infra | Componentes principales |
|---|---|---|
| Fase 1 (MVP cotizador + tienda) | ~$200-350 USD/mes | Vercel + Supabase + microservicio + APIs |
| Fase 2 (marketplace + ERP) | ~$350-600 USD/mes | + cache + observabilidad + ERP vendedores |
| Fase 3 (Lotes de Demanda) | ~$550-1.100 USD/mes | + worker Python + cola + event sourcing |

**El costo de infra no es el problema.** Lo importante es que el stack **soporte la lógica de negocio sin obligar a reescribir** cuando crezca el volumen.

---

## Modelo de datos clave

### `clients`
```
id, email, name, phone, document_type, document_number,
client_type (individual | wholesale),
afip_status (verified | pending | not_required),
courier_quota_used, courier_quota_year,
created_at, updated_at
```

### `quotations`
```
id, client_id, status,
product_link, product_title, product_origin_country,
weight_declared, dimensions (jsonb), volumetric_weight, billable_weight,
ncm_category, value_origin_usd, quantity,
calculated_fee_usd, total_usd, total_ars, exchange_rate_used, exchange_spread,
expires_at, created_at, confirmed_at, paid_at,
manual_review_required (bool),
manual_review_status (pending | approved | rejected | adjusted)
```

### `orders` (post-cotización confirmada)
```
id, quotation_id, client_id,
status (pending_payment | paid | in_origin | in_transit | in_customs | delivered | cancelled),
tracking_provider, tracking_number, tracking_url,
estimated_delivery, actual_delivery,
margin_real_usd (auditoría post-envío)
```

### `marketplace_listings`
```
id, vendor_id, title, description, category, price_ars,
stock, status, commission_rate (8-12%),
sold_count, refund_count, fraud_flag
```

### `events_audit` (append-only, immutable)
```
id, event_type, entity_type, entity_id,
actor_id, payload (jsonb), hash_prev, hash_self,
timestamp
```

---

## Seguridad

- **PCI-DSS:** la plataforma **nunca** almacena datos de tarjeta. Tokenización delegada a Mercado Pago / Stripe.
- **PDPA / Ley 25.326:** datos de cliente cifrados en reposo (Postgres native + Supabase Vault) y en tránsito (TLS 1.3).
- **Row Level Security (RLS)** en todas las tablas de Supabase para aislar datos por usuario.
- **Logs forenses inmutables** con hash encadenado (event sourcing).
- **Retención mínima:** 5 años (cumplimiento aduanero).
- **Backups automáticos** diarios con retención 30 días.

---

## Escalabilidad ante picos virales

- Catálogos públicos en CDN (Vercel Edge / Cloudflare) — viralidad no toca la BD.
- Rate limiting + cache Redis en endpoints de cotización (cotizador es endpoint costoso).
- Webhooks de tracking → ingestión a cola, no escritura directa a BD; consolidación batch cada 5-15 min.
- Smart Routing dinámico (Miami / Shanghai / SP) — el sistema cambia rutas en tiempo real según restricciones aduaneras detectadas, cupo Courier remanente y costo logístico instantáneo.

---

## APIs y servicios externos

| Servicio | Propósito | Plan inicial |
|---|---|---|
| Amazon Product Advertising API | Extracción de datos de productos Amazon | Gratis hasta volumen, requiere aprobación |
| eBay Browse API | Extracción de datos de productos eBay | Gratis hasta volumen |
| AliExpress Open Platform | Extracción de datos de productos AliExpress | Gratis hasta volumen |
| AFIP Padrón A4/A5 | Validación CUIT + condición fiscal | Gratis (webservice oficial) |
| AfterShip | Tracking internacional | $11-50 USD/mes |
| Mercado Pago | Pagos locales (split payment) | % variable |
| Stripe | Pagos B2B internacionales | % variable |
| Wise / Lemon Cash / Binance | Liquidación T+0 USDT/USDC | % variable |
| WhatsApp Business API | Soporte y notificaciones | $20-100 USD/mes |
| Resend / Mailgun | Email transaccional | $20-50 USD/mes |

**Plan B obligatorio:** las APIs oficiales (Amazon en particular) requieren aprobación previa que puede tardar o ser rechazada. Diseñar el cotizador para soportar **captura manual de datos por el cliente** desde el día 1, sin asumir que las APIs estarán disponibles. Si están, mejor; si no, el flujo no se rompe.

---

## Próximas lecturas

- [02 — Producto Cotizador](02-producto-cotizador.md): la lógica de negocio que corre sobre este stack.
- [09 — Plan de Lanzamiento](09-plan-lanzamiento.md): qué se construye en cada fase.
- [11 — Equipo y Operaciones](11-equipo-y-operaciones.md): qué roles soportan este stack.
