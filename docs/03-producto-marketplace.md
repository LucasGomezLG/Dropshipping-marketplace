# 03 — Producto: Marketplace con Comisión 8-12%

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, asesor contable, equipo comercial.

---

## Por qué falló el 3-5% de versiones anteriores

Las pasarelas de pago en Argentina (Mercado Pago, Ualá Bis) cobran **2,9% a 6,29% + IVA**. Si la comisión de la plataforma es 3-5%, **el banco se queda con el margen completo**.

| Componente | Porcentaje |
|---|---|
| Comisión bruta plataforma | 4% (punto medio del 3-5%) |
| Comisión pasarela + IVA | 5% (punto medio) |
| **Margen neto plataforma** | **-1% (pérdida)** |

Sobre un GMV mensual de $10.000 USD eso son $100 USD de pérdida por mes solo del marketplace. Inviable.

---

## Por qué cierra el 8-12%

| Componente | Porcentaje |
|---|---|
| Comisión bruta plataforma | 10% (punto medio del 8-12%) |
| Comisión pasarela + IVA | 6% |
| **Margen neto plataforma** | **4%** |

Sobre un GMV de $10.000 USD/mes → **$400 USD/mes de margen neto solo del marketplace**. Suma. No es el motor del negocio (lo es el dropshipping mayorista), pero **deja de ser un sumidero** y se vuelve una unidad de negocio independiente.

---

## Posicionamiento competitivo

| Plataforma | Comisión bruta | Diferencial |
|---|---|---|
| Mercado Libre | 13-17% según categoría | Tráfico masivo, logística Full, posicionamiento orgánico |
| Tiendanube | 0% comisión + abono mensual ($30-100 USD) | Tienda propia, pero sin tráfico orgánico |
| **Nuestra plataforma** | **8-12%** | **Cotizador + Dropshipping integrados + comunidad maker** |

El gancho para captar vendedores expulsados de ML:
> *"Pagás menos comisión que en ML, **y además accedés a importar tus insumos a precio mayorista** desde el mismo panel."*

---

## Comisión por categoría (no plana)

La comisión 8-12% es un **rango configurable por categoría**, no un porcentaje plano:

| Categoría de producto | Comisión sugerida |
|---|---|
| Indumentaria, accesorios, marroquinería | 8% |
| Productos hechos a mano / artesanos | 9% |
| Hogar y decoración | 10% |
| Repuestos automotores y herramientas | 11% |
| Electrónica (alto fraude / devoluciones) | 12% |

La calibración por categoría se ajusta cada trimestre según tasa de devolución, fraude y costo de soporte real.

---

## Cómo se cobra (split payment)

**Cambio crítico vs versiones anteriores:** la plataforma NO retiene fondos de terceros prolongadamente para evitar calificar como agente de retención bajo Resolución General 4622 de AFIP.

### Flujo de pago
1. La pasarela (Mercado Pago / Ualá Bis) cobra al **comprador final**.
2. La pasarela transfiere el monto neto al **vendedor** (descontando su propia comisión bancaria).
3. La plataforma cobra su comisión 8-12% **al vendedor**, vía Mercado Pago split o débito directo, **después** del cobro al comprador.

### Por qué este modelo
- **No retiene fondos de terceros** → no califica como agente de retención.
- **No requiere certificación PCI-DSS** (no toca datos de tarjeta).
- **Reduce burocracia AFIP** asociada a procesar pagos.

**Nota crítica:** este modelo debe ser validado con el asesor aduanero/contable antes del lanzamiento. En algunos casos AFIP igual reclama responsabilidad solidaria por la actividad del vendedor. Documentar TyC con esa claridad explícita. Detalle en [07 — Compliance Legal](07-compliance-legal.md).

---

## Riesgos fiscales del marketplace con comisión

### Responsabilidad solidaria (Resolución General 4622 AFIP)
- Si AFIP detecta vendedores no inscriptos operando en la plataforma → la plataforma puede ser solidariamente responsable de la deuda fiscal.

### Mitigación
- **Verificación obligatoria de CUIT/CUIL + condición ante IVA** al alta del vendedor.
- **Webservice padrón AFIP A4/A5** para validar automáticamente.
- **TyC explícitos:** la plataforma es "intermediaria de pago" y los vendedores son responsables de sus propias obligaciones fiscales.
- **Asesor aduanero/contable medio tiempo** (no opcional) para monitorear cambios regulatorios.

---

## Dimensión Marketplace en el funnel general

El marketplace **no es el motor de rentabilidad** — lo son los mayoristas vía cotizador.
El marketplace **es el imán de adquisición** orgánica del cotizador:

```
Vendedor llega al marketplace por la comisión baja
      ↓
Sube su catálogo, vende producto local
      ↓
Necesita reponer insumos / herramientas / materia prima
      ↓
Descubre el cotizador integrado en su panel
      ↓
Hace su primera importación B2B → se vuelve cliente recurrente
```

Los vendedores del marketplace tienen una conversión esperada del **30-40%** a clientes del cotizador a 90 días, vs el 5-10% de un visitante anónimo.

---

## Producto del lado vendedor

Lo que recibe un vendedor que se da de alta:

1. **Tienda online lista** con su URL personalizada (`marketplace.com/tienda/nombre`).
2. **Panel de gestión** de productos, pedidos, ventas.
3. **Cobro automático** vía Mercado Pago split.
4. **Acceso al cotizador integrado** desde el mismo panel.
5. **ERP de stock genérico** (a partir de Fase 2) que sirve para ventas on y off platform.
6. **Bot de WhatsApp** para gestión rápida de inventario.
7. **Sello "Importador Verificado"** tras ≥3 importaciones B2B exitosas.

---

## Onboarding del vendedor

1. Alta con CUIT + verificación AFIP.
2. Carga de productos (UI tipo formulario + bulk upload CSV en Fase 2).
3. Configuración de medios de pago.
4. Tutorial breve del cotizador integrado.
5. Primera venta con soporte humano para asegurar buena experiencia inicial.

---

## KPIs específicos del marketplace

- **GMV mensual:** target ≥ $5.000 USD a partir del mes 6, ≥ $15.000 USD a partir del mes 12.
- **Comisión neta mensual:** target ≥ $400 USD a partir del mes 6.
- **Conversión vendedor → cliente cotizador (90 días):** target ≥ 30%.
- **Vendedores activos mensuales:** target ≥ 50 a partir del mes 6.
- **Refund rate:** target < 5%.
- **Tasa de fraude:** target < 1%.

---

## Próximas lecturas

- [05 — Clientes Dual Mode](05-clientes-dual-mode.md): el marketplace atiende vendedores que pueden ser particulares o tiendas mayoristas registradas.
- [07 — Compliance Legal](07-compliance-legal.md): detalle de responsabilidad solidaria y TyC del marketplace.
- [11 — Equipo y Operaciones](11-equipo-y-operaciones.md): qué roles soportan el marketplace.
