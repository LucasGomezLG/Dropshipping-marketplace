---
layout: default
title: "Producto: Tienda Dropshipping Pre-cotizada"
nav_order: 6
---

# 04 — Producto: Tienda Dropshipping Pre-cotizada

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, equipo de producto.

---

## Qué es

Productos que el founder ya importa de forma repetida tienen **cotización pre-calculada** y **stock confirmado por proveedor** en origen. El cliente no necesita pasar por el cotizador: ve el producto con precio final, lo compra con un click.

**Convierte significativamente mejor que el cotizador puro** porque elimina la fricción de cargar datos.

---

## Estrategia: Catálogo Curado vs Catálogo tipo Tiendamia

### Por qué NO replicamos Tiendamia / Grabr
Replicar el modelo Tiendamia (catálogo masivo de Amazon/eBay scrapeado) tiene tres problemas que matan al proyecto en escala startup:

1. **Scrapping de Amazon/eBay** viola sus ToS y tiene rate limits agresivos. **Bloqueo en semanas.**
2. **Sincronización stock/precio en tiempo real** sobre millones de SKUs cuesta cinco cifras al mes en infra. Tiendamia tiene equipos de 30+ personas para esto.
3. **Markup dinámico** (cambio cambiario + arancel + IVA + IVA adicional + Tasa Estadística + fee + margen) recalculado por SKU es un problema de complejidad logarítmica.

### La estrategia correcta: Catálogo Curado

- **100-500 SKUs validados manualmente** con cotización pre-calculada y stock confirmado por proveedor.
- Mantenimiento manual en **Airtable o Google Sheets** (Fase 1) → migración a **Postgres / Supabase** en Fase 2.
- **100× más barato de mantener, 10× más rentable por SKU.**
- Permite mostrar "vidriera" sin asumir el costo de Tiendamia.

---

## Criterio de selección de SKUs

Los SKUs que entran al catálogo curado cumplen TODOS los siguientes criterios:

1. **Demanda probada:** ya se importó al menos 10 veces manualmente con margen ≥14%.
2. **Stock garantizado:** proveedor en origen con capacidad de reposición confiable.
3. **Cotización estable:** baja variabilidad de costo (sin productos sujetos a precios spot volátiles).
4. **NCM en whitelist:** debe estar dentro de las categorías cotizables automáticamente.
5. **Logística simple:** peso < 5 kg, volumen < 0,05 m³, sin restricciones especiales.
6. **Margen objetivo:** ≥ 16% (más alto que el promedio porque hay menos fricción y se vende más rápido).

---

## Operación del catálogo

### Fase 1 (mes 0-3): Catálogo manual

- 50-150 SKUs iniciales en Airtable.
- Productos que el founder ya importa repetidamente (autopartes, herramientas, accesorios).
- Frontend en Next.js consume la base via API de Airtable.
- Stock y precio se actualizan manualmente cada 7-14 días.

### Fase 2 (mes 6-12): Migración a Postgres

- Migración de Airtable a Supabase / Postgres.
- 200-500 SKUs.
- Actualización automatizada de precio (consulta a APIs oficiales de proveedores).
- Stock semi-automatizado (verificación al momento de la compra antes de confirmar al cliente).

### Fase 3+: Integraciones avanzadas (opcional)

- **Affiliate API oficial de Amazon** (Product Advertising API) — legal, gratuita hasta cierto volumen.
- **AliExpress Open Platform** — para SKUs validados que conviene tener vivos.
- **eBay Browse API** — productos de revendedores con buena reputación.

**Bajo ningún escenario:** scrapping ad-hoc.

---

## Diferenciación particular vs mayorista en la tienda

La tienda dropshipping también respeta el dual mode. Detalle en [05 — Clientes Dual Mode](05-clientes-dual-mode.md):

| Variable | Particular | Mayorista |
|---|---|---|
| Precio mostrado | Unitario con todo incluido | Escalonado por volumen |
| Cantidad mínima | 1 | 3-10 según producto |
| Documentación al checkout | DNI | CUIT obligatorio |
| Plazo de entrega | 30-45 días | 30-60 días según volumen |
| Soporte post-venta | Auto-servicio | Asesor de cuenta |

---

## Tienda como conversión del cotizador

El cotizador y la tienda se alimentan entre sí:

```
Cliente entra al cotizador → cotiza un producto X
      ↓
Sistema detecta: ese producto YA está en el catálogo curado, más barato y disponible
      ↓
Sugiere: "Este producto ya lo tenemos pre-cotizado y disponible: $XXX. ¿Compralo ahora?"
      ↓
Cliente compra con un click sin terminar de cargar el cotizador.
```

Esto **mejora la conversión del cotizador** y **reduce el trabajo operativo** (los SKUs del catálogo no requieren confirmación manual del founder en modo asistido).

---

## KPIs específicos de la tienda

- **Productos activos en catálogo:** target 100 (Fase 1), 300 (Fase 2), 500+ (Fase 3).
- **Tasa de conversión visita producto → compra:** target ≥ 5%.
- **Margen promedio por venta:** target ≥ 16% (mayor que el cotizador genérico).
- **Tiempo promedio entre compra y despacho desde origen:** target < 48 hs.
- **Stockouts:** target < 5% de los productos activos.

---

## Próximas lecturas

- [02 — Producto Cotizador](02-producto-cotizador.md): cómo el cotizador alimenta el catálogo curado.
- [05 — Clientes Dual Mode](05-clientes-dual-mode.md): cómo cambia la tienda según tipo de cliente.
- [09 — Plan de Lanzamiento](09-plan-lanzamiento.md): fasing del catálogo.
