---
layout: default
title: "Equipo y Operaciones"
nav_order: 13
---

# 11 — Equipo y Operaciones

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, RRHH, inversores con foco en ejecución.

---

## Principio rector

Equipo magro en Fase 1, escalamiento condicionado a tracción real. Sweat equity de los founders cubre los primeros 6-9 meses; los roles pagados arrancan solo donde la operación lo requiere desde el día 1 (asesor aduanero).

---

## Estructura del Equipo Inicial (Fase 1 — MVP)

### Founder / Líder de Ventas y Comunidad
- Outreach a vendedores y mayoristas.
- Producción de contenido (Reels, casos de estudio, tutoriales).
- Atención personalizada a mayoristas (es lo que ya hace bien).
- En modo asistido del cotizador: confirma manualmente cada cotización los primeros 60 días.
- Compensación: equity / sweat equity.

### Tech Lead / Desarrollador
- Frontend Next.js + Cotizador Híbrido + integraciones APIs.
- Configuración de Supabase, microservicio cotizador, integraciones de pago.
- Setup de monitoreo y observabilidad básica.
- Puede ser part-time inicial (1 persona, 20-30 hs/semana) o por proyecto.
- Compensación: equity / sweat equity / pago por proyecto.

### Broker Logístico / Operador B2B
- Negociación con forwarders (China, Miami, SP).
- Consolidación manual de pedidos en Fase 1.
- Última milla local (alianzas con Andreani, Correo Argentino, OCA).
- Manejo de incidencias aduaneras.
- Compensación: equity / sweat equity.

### Soporte al Usuario (CX)
- Gestión proactiva de expectativas de Dropshipping (plazos, demoras aduaneras).
- Atención post-venta y manejo de devoluciones.
- Atención del cliente particular vía chat / WhatsApp.
- Puede ser part-time inicial (1 persona, 20 hs/semana).
- Compensación: equity / sweat equity / pago variable.

### Asesor Aduanero / Contable (medio tiempo, **no opcional**)
- Validación de NCMs en whitelist.
- Monitoreo de cambios regulatorios.
- Defensa ante fiscalizaciones AFIP / Aduana.
- Redacción y revisión de TyC.
- Validación de operatoria cambiaria (cobertura USDT/USDC).
- **Costo: $500-1.500 USD/mes según jurisdicción.**
- **Crítico no opcional** — particularmente importante con el marketplace cobrando comisión (responsabilidad solidaria AFIP).

---

## Equipo Fase 2 (mes 6-12)

### B2B Manager / Account Manager Mayoristas (rol nuevo)
- Captación activa de mayoristas (outreach, eventos, alianzas).
- Gestión de cuenta corriente de clientes recurrentes.
- Cotizaciones a medida fuera de la whitelist automática.
- Coordinación logística personalizada (despachante, fechas, fulfillment).
- Reposición programada y discovery calls.
- **Costo: $600-1.200 USD/mes** en Argentina.
- **Por qué este rol es necesario:** los mayoristas concentran el LTV (44× mayor que un particular). Sin un dueño dedicado del canal B2B, la rentabilidad se diluye.

### Operador de cotizaciones manuales (opcional, condicional)
- Si la tasa de fallback a manual supera el 35%, contratar un junior dedicado.
- **Costo: $400-800 USD/mes** en Argentina.
- **Solo se contrata** si los datos lo justifican.

### Soporte adicional CX
- Si el GMV del marketplace supera $15.000 USD/mes y los particulares activos pasan de 50, sumar 1 persona adicional a CX (full-time o part-time).

---

## Equipo Fase 3 (mes 12-24, condicional)

### Especialista en optimización / Ops Engineer
- Mantenimiento del worker Python de Lotes de Demanda.
- Tunning de algoritmo de bin-packing.
- Smart Routing dinámico.
- **Costo: $1.500-3.000 USD/mes** según seniority.
- Solo se contrata si Fase 3 se activa.

---

## Burn-rate Detallado

### Fase 1 (MVP, sweat equity)
| Concepto | Costo mensual |
|---|---|
| Infraestructura tech | $200-350 USD |
| APIs (AfterShip, WhatsApp, Pasarelas) | $50-150 USD |
| Asesor aduanero / contable medio tiempo | $500-1.500 USD |
| Mantenimiento sociedad (LLC + SRL) | $150-400 USD |
| Pauta publicitaria experimental | $100-300 USD |
| **Subtotal Fase 1** | **~$1.000-2.700 USD** |
| **Burn-rate efectivo (con sueldos en equity)** | **~$1.000-1.500 USD/mes** |

### Fase 2 (sumando B2B Manager)
| Concepto adicional | Costo mensual |
|---|---|
| B2B Manager | $600-1.200 USD |
| CX adicional (part-time) | $300-600 USD |
| Infraestructura escalada (Fase 2) | +$150-250 USD |
| **Subtotal adicional Fase 2** | **+$1.050-2.050 USD** |
| **Burn-rate efectivo Fase 2** | **~$2.000-3.500 USD/mes** |

### Cuándo empezar a pagar sueldos a founders
Cuando el margen mensual sostenido supere los $5.000 USD durante 3 meses consecutivos. Antes de eso, el equity / sweat equity es la compensación.

---

## Operación dual: por qué duplica el costo de soporte

Atender 30 particulares con tracking automático es barato (proceso casi 100% automatizado).
Atender 3 mayoristas con asesor dedicado, factura A, despachante y reposición programada es **otro tipo de trabajo que requiere otra persona o más horas del founder**.

| Actividad | Particular | Mayorista |
|---|---|---|
| Onboarding | 0-15 min auto-servicio | 30-60 min discovery call |
| Atención por cotización | 0-5 min (auto) | 15-30 min (asesor) |
| Atención por incidencia aduanera | 15 min (chat) | 1-2 horas (gestión activa) |
| Reposición programada | n/a | 2-3 horas/mes/cliente |
| Account expansion | 0 (orgánico) | 4-6 horas/trimestre/cliente |

**Ejemplo de carga real Fase 2:** 50 particulares + 10 mayoristas = ~12 horas/semana de soporte particulares (automatizable con CX) + ~25 horas/semana de gestión mayoristas (requiere B2B Manager dedicado).

Esto justifica el rol de B2B Manager desde Fase 2.

---

## North Star Metrics

- **B2C particular:** importaciones / visitantes únicos del cotizador (mensual).
- **B2B mayorista:** ticket promedio × frecuencia × clientes mayoristas activos (LTV trimestral).
- **Marketplace:** GMV mensual + comisión neta mensual.

---

## KPIs operativos críticos

| KPI | Target Fase 1 | Target Fase 2 |
|---|---|---|
| Conversión visita → cotización | ≥ 8% | ≥ 10% |
| Conversión cotización → compra (particular) | ≥ 30% | ≥ 35% |
| Conversión cotización → compra (mayorista) | ≥ 50% | ≥ 60% |
| Frecuencia mayoristas / año | ≥ 4 | ≥ 6 |
| GMV marketplace / mes | n/a | ≥ $15.000 USD |
| Comisión neta marketplace / mes | n/a | ≥ $600 USD |
| Margen post-envío promedio | ≥ 14% | ≥ 16% |
| Tasa de incidentes aduaneros | < 5% | < 3% |
| Tasa de fallback a manual del cotizador | < 35% | < 25% |
| LTV/CAC (todos los segmentos) | ≥ 2× | ≥ 3× |

---

## Cultura de equipo

### Principios operativos
1. **Validar antes de construir.** Cada feature nuevo requiere KPI de éxito definido.
2. **Whitelist crece con datos, no con intuición.** Categorías nuevas solo si ≥10 envíos exitosos manuales con margen ≥14%.
3. **Modo asistido los primeros 60 días.** Founder confirma manualmente cada cotización.
4. **Cohortes weekly.** Cohort analysis semanal (no mensual) para detectar problemas temprano.
5. **Tolerancia pasiva al bypass.** No perseguir, no facilitar activamente.
6. **Mayoristas son prioridad estratégica.** El founder protege su tiempo de mayoristas hasta que entre el B2B Manager.

### Stack de comunicación
- **Slack** (interno).
- **WhatsApp Business** (clientes).
- **Notion** (documentación de procesos).
- **GitHub** (código + este repo de docs).

---

## Próximas lecturas

- [09 — Plan de Lanzamiento](09-plan-lanzamiento.md): cuándo se suma cada rol.
- [12 — Capital y Financiamiento](12-capital-y-financiamiento.md): cuándo el equipo crece con sueldos.
- [13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md): el rol del B2B Manager en detalle.
