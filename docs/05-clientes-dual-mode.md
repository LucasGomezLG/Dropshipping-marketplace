---
layout: default
title: "Clientes Dual Mode: Particular vs Tienda Mayorista"
nav_order: 7
---

# 05 — Clientes Dual Mode: Particular vs Tienda Mayorista

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, equipo de producto, soporte, asesor aduanero.

---

## Por qué este documento es transversal

La diferenciación Particular vs Mayorista atraviesa todos los componentes del sistema (cotizador, tienda, marketplace, soporte, compliance, finanzas). En lugar de duplicar la información en cada archivo, **se centraliza acá** y los demás documentos referencian este.

---

## Tabla comparativa completa

| Variable | Particular (B2C internacional) | Tienda Mayorista (B2B nacional) |
|---|---|---|
| Ticket promedio | $50-$300 USD | $1.000-$5.000 USD |
| Frecuencia anual | 1-2 importaciones | 5-8 importaciones |
| Documento requerido | DNI | CUIT + factura A/B |
| Régimen aduanero | Courier puerta a puerta | Courier (hasta USD 3.000) o despacho a plaza con despachante |
| Cupo Courier anual | 12 envíos / año | 12 envíos / año (insuficiente) → necesita régimen comercial |
| Estructura de precio | Precio cerrado, fee fijo | Descuento escalonado por volumen |
| Comisión fee logístico | 14-18% | 10-14% (descuento por volumen) |
| Soporte que necesita | Tracking automático + tranquilizador | Asesor de cuenta + cotizaciones a medida |
| Lock-in primario | Plazo predecible + simpleza | Reposición programada + financiamiento |
| Onboarding | Auto-servicio con tutorial | Llamada con asesor + alta como cliente recurrente |
| Cuenta corriente | No | Sí (pago a 30/60 días post Fase 2) |
| LTV proyectado | $79 USD (escenario realista) | $3.500 USD (escenario realista) |
| Churn anual | 40% | 30% |

**LTV de un mayorista es 44× mayor que un particular.** Esto justifica toda la diferenciación operativa.

---

## Implementación técnica

### Toggle inicial del cotizador
La primera pantalla del cotizador presenta un toggle obligatorio:
> **"¿Estás importando para vos o para tu negocio?"**
> [ ] Soy particular (compro para mí)
> [ ] Soy tienda / negocio (compro para revender)

El toggle dispara todo el flujo posterior:
- Pricing diferente.
- Validaciones distintas.
- Documentación requerida distinta.
- Canal de soporte distinto.

### Modelo de datos
La tabla `clients` tiene un campo `client_type` (`individual` | `wholesale`) que dispara reglas distintas en:
- **Pricing:** descuentos escalonados por volumen, IVA mayorista computable.
- **Validaciones aduaneras:** cupo Courier (particular) vs régimen comercial (mayorista).
- **Soporte:** tracking automático (particular) vs asesor de cuenta (mayorista).
- **Comunicación:** email transaccional (particular) vs WhatsApp prioritario (mayorista).

### Por qué desde el MVP (no agregarlo después)
Meter este split después es **retrabajo grande**:
- El modelo de datos de cliente, pedido y cotización ya nace dual.
- La fórmula de pricing tiene branching condicional.
- El soporte y onboarding son productos distintos.
- Si arrancás solo con particulares y después intentás sumar mayoristas, hay que reescribir el ~40% del backend.

---

## Diferencias aduaneras

| Aspecto | Particular | Mayorista |
|---|---|---|
| Régimen aduanero | Courier puerta a puerta | Courier (si entra en límites) o despacho a plaza |
| Cupo anual | 12 envíos / USD 3.000 c/u | Sin cupo si va por régimen comercial |
| Despachante | No requerido | Requerido en despacho a plaza |
| IVA importación | 21% + 20% adicional (no recuperable) | Computable como crédito fiscal si el mayorista es Responsable Inscripto |
| Documentación | DNI + factura de origen | CUIT + factura de origen + DDJJ Aduanera + DJAI/SIRA si aplica |
| Tiempo de despacho | 5-15 días post arribo | 7-30 días post arribo (más burocracia) |
| Canal de entrega | Correo Argentino / Andreani directo | Andreani con factura empresa / coordinación logística específica |

Detalle adicional en [07 — Compliance Legal](07-compliance-legal.md).

---

## Diferencias de soporte

### Particular
- **Auto-servicio primero:** preguntas frecuentes, tracking visual, ETA automática.
- **Notificaciones proactivas** en cada estado (origen → courier → aduana → distribución → entrega).
- **Soporte humano vía chat o WhatsApp Business** solo cuando el sistema detecta excepción (demora aduanera, faltante).
- **SLA de respuesta:** 24-48 hs.

### Mayorista
- **Asesor de cuenta dedicado** desde el alta (founder en Fase 1, B2B Manager desde Fase 2).
- **Canal directo de WhatsApp prioritario.**
- **Cotizaciones a medida** fuera de la whitelist automática.
- **Coordinación logística personalizada** (despachante, fechas, fulfillment).
- **SLA de respuesta:** 2-4 horas en horario comercial.

---

## Diferencias de retención

### Particular
- Notificaciones proactivas para reducir ansiedad.
- Sello de confianza tras primera importación exitosa.
- Email post-entrega con cross-sell ("podrías importar también X").

### Mayorista
- **Cuenta corriente** con condiciones a medida (post Fase 2).
- **Descuentos por volumen anual** acumulado.
- **Reposición programada** (cadencia mensual o bimestral con pre-cotización).
- **Acceso a financiamiento** condicionado a 90 días de actividad consistente.
- **Asesor dedicado** que conoce al cliente.

---

## Conversión de tipo

Un cliente puede cambiar de tipo en la plataforma:
- Un **particular** que hace 3 cotizaciones de alto ticket en un año → el sistema le ofrece pasarse a modo mayorista.
- Un **mayorista** que dejó de comprar volumen → el sistema lo reclasifica a particular y reduce costo de soporte.

El toggle no es estático; se reevalúa cada 6 meses según el comportamiento real.

---

## Métricas separadas en el dashboard

El dashboard interno tiene secciones independientes para cada perfil:

### Particulares
- Visitantes únicos del cotizador / mes.
- Conversión visita → cotización.
- Conversión cotización → compra.
- Ticket promedio.
- Frecuencia anual.
- Refund rate.
- NPS particular.

### Mayoristas
- Leads B2B / mes.
- Conversion lead → discovery call.
- Conversion call → primera importación.
- Ticket promedio mayorista.
- Frecuencia mensual.
- Account expansion rate.
- Account churn rate.
- LTV mayorista promedio.
- NPS mayorista.

Lo que funciona para uno puede no funcionar para el otro. Mezclar métricas hace tomar decisiones equivocadas.

---

## Implicancias en el equipo

- **Founder en Fase 1:** atiende personalmente a los mayoristas (es lo que ya hace bien).
- **CX en Fase 1:** atiende particulares con foco en automatización del soporte.
- **B2B Manager en Fase 2 (mes 6+):** rol nuevo dedicado a captación + cuenta corriente de mayoristas. Costo $600-1.200 USD/mes en Argentina. Detalle en [11 — Equipo y Operaciones](11-equipo-y-operaciones.md).

---

## Próximas lecturas

- [02 — Producto Cotizador](02-producto-cotizador.md): cómo cambia el flujo del cotizador según el toggle.
- [07 — Compliance Legal](07-compliance-legal.md): implicancias legales del régimen Courier vs comercial.
- [13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md): cómo se captan los mayoristas.
