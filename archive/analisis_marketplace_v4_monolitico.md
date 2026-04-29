---
layout: default
title: "Reporte de Viabilidad Técnica y Estratégica: Plataforma Integrada Cotizador + Marketplace + Dropshipping"
nav_order: 100
---

# Reporte de Viabilidad Técnica y Estratégica: Plataforma Integrada Cotizador + Marketplace + Dropshipping

**Versión 4.0 — Visión del Founder.**
La v3.0 propuso un wedge vertical en autos. El founder rechazó esa estrategia: el dolor concreto que quiere resolver es **automatizar el flujo manual de cotización por WhatsApp** que ya hace todos los días, y **expandirlo a todos los rubros desde el día 1**, complementado con un marketplace de comisiones competitivas (8-12%, no 3-5%) y diferenciación de cliente particular vs tienda mayorista desde el MVP.

Esta versión 4.0 alinea el documento con esa visión, manteniendo todo el análisis técnico de versiones anteriores (Cotizador Híbrido, compliance multi-jurisdicción, stack escalonado, riesgos) que sigue siendo correcto bajo cualquier escenario.

**Cambios estructurales v3.0 → v4.0:**
- Descartado el wedge solo-autos. Cotizador universal con whitelist de NCMs desde día 1.
- Marketplace recupera centralidad pero con comisión real (8-12%, no 3-5%) que sí cierra Unit Economics.
- Sumada diferenciación **Particular vs Tienda Mayorista** como dimensión de producto desde el MVP.
- Cotizador automático posicionado como **componente #1 prioritario** (resuelve el cuello de botella actual del founder: su tiempo).

---

## 1. Resumen Ejecutivo

El proyecto construye una plataforma integrada de tres componentes que se alimentan entre sí:

1. **Cotizador automático universal** que reemplaza el flujo manual actual del founder por WhatsApp.
2. **Tienda de dropshipping pre-cotizada** con catálogo curado (~100-500 SKUs ya importados repetidamente).
3. **Marketplace local "vidriera"** con comisión 8-12% para vendedores que quieran vender producto físico al público local.

El sistema atiende dos perfiles de cliente claramente diferenciados desde el MVP:
- **Cliente particular:** trae UNA cosa para sí mismo (ticket bajo, frecuencia baja, régimen Courier).
- **Cliente tienda mayorista:** compra volumen para revender (ticket alto, recurrencia, posiblemente despachante).

El founder ya opera la unidad de Dropshipping manualmente. La plataforma es la herramienta que **escala su tiempo** automatizando la parte cotizable y derivando lo complejo a su atención manual.

---

## 2. El Problema Real que Resuelve la Plataforma

### 2.1. El cuello de botella actual: tiempo manual del founder
Hoy, cada cotización pasa por el cerebro y WhatsApp del founder:
1. Cliente manda mensaje pidiendo precio.
2. Founder pide datos (link del producto, peso, dimensiones, país de origen).
3. Cliente responde (a veces incompleto, a veces tarda días).
4. Founder calcula manualmente: cambio + arancel + flete + fee + IVA.
5. Founder responde con cotización.
6. Cliente acepta o no.

**Cada cotización consume 10-30 minutos de ida y vuelta.** El negocio escala linealmente con el tiempo del founder, lo cual es el techo natural del proyecto manual.

### 2.2. Lo que la plataforma automatiza
*   El **cliente carga sus propios datos** en un formulario estructurado (no hay ida y vuelta de WhatsApp).
*   El **cotizador calcula y muestra el precio en segundos** según fórmula calibrada.
*   El **cliente paga online** sin que el founder intervenga.
*   El founder solo interviene en casos que caen en blacklist (productos restringidos, voluminosos, valor > $1.000 USD).

**Impacto esperado:** liberar 70-85% del tiempo actual del founder, escalando capacidad sin sumar personal.

---

## 3. Arquitectura del Producto: Tres Componentes Integrados

### 3.1. Componente 1 — Cotizador Automático Universal (núcleo del sistema)
**Es el corazón del producto.** Cualquier visitante puede cotizar sin login. El login se exige solo al confirmar la compra.

**Flujo del cotizador:**
1. **Toggle inicial:** "soy particular" / "soy tienda" (define todo el flujo posterior).
2. **Categoría obligatoria** desde dropdown limitado a NCMs validados (whitelist).
3. **Captura de variables clave:**
   *   Link del producto (Amazon, eBay, AliExpress, otros).
   *   Peso declarado.
   *   Dimensiones declaradas (alto × ancho × largo) — el sistema calcula peso volumétrico automáticamente.
   *   Valor en origen (USD).
   *   País de origen.
   *   Cantidad de unidades.
4. **Cálculo automático:** `precio_origen + arancel(NCM) + IVA + IVA_adicional + tasa_estadística + IIBB + flete + fee_logístico + cobertura_cambiaria`.
5. **Resultado:** precio total en USD y ARS, plazo estimado, tracking previsto.
6. **CTA:** "Confirmá y pagá" o "Guardar cotización para después".

### 3.2. Componente 2 — Tienda Dropshipping Pre-cotizada
Productos que el founder ya importa de forma repetida tienen cotización pre-calculada y stock confirmado por proveedor.

*   **Catálogo curado de 100-500 SKUs.**
*   **Mantenimiento manual** en Airtable / Sheets (Fase 1) → migración a Postgres (Fase 2).
*   **Compra directa con un click**, sin pasar por el cotizador.
*   **Convierte mejor que el cotizador puro** porque elimina la fricción de cargar datos.

### 3.3. Componente 3 — Marketplace "Vidriera"
Vendedores locales suben sus productos para vender al público local.

*   **Comisión 8-12%** (configurable por categoría).
*   **Pasarela integrada** (Mercado Pago / Ualá Bis): la comisión se descuenta automáticamente.
*   **Margen neto post-pasarela:** 2-6% para la plataforma.
*   **Posicionamiento competitivo:** Mercado Libre cobra 13-17%; nosotros 8-12% pero con dropshipping + cotizador como diferencial.
*   **Funciona como CAC del B2B:** los vendedores que usan el marketplace son leads naturales para el cotizador (necesitan reponer insumos).

---

## 4. Doble Flujo: Particular vs Tienda Mayorista

**Esta es la dimensión nueva de producto que diferencia v4.0 de versiones anteriores.** Comparten infraestructura técnica, NO comparten flujo, precio, ni soporte.

| Variable | Particular | Tienda Mayorista |
|---|---|---|
| Ticket promedio | $50-$300 USD | $1.000-$5.000 USD |
| Frecuencia | 1-2 al año | 1 cada 1-3 meses |
| Documento requerido | DNI | CUIT + factura A/B |
| Régimen aduanero | Courier puerta a puerta | Courier (hasta USD 3.000) o despacho a plaza con despachante |
| Cupo Courier | 12 envíos/año | 12 envíos/año (insuficiente) → necesita régimen comercial |
| Estructura de precio | Precio cerrado, fee fijo | Descuento escalonado por volumen |
| Soporte que necesita | Tracking + tranquilizador automático | Asesor de cuenta + cotizaciones a medida |
| Lock-in primario | Plazo de entrega + simpleza | Reposición programada + financiamiento |
| Onboarding | Auto-servicio con tutorial | Llamada con asesor + alta como cliente recurrente |

### 4.1. Implementación técnica del doble flujo
*   **Toggle al inicio del cotizador** define todo el flujo posterior.
*   **Modelos de datos diferenciados** desde el MVP: tabla `clients` con campo `client_type` (`individual` / `wholesale`) que dispara reglas distintas en pricing, validaciones aduaneras y soporte.
*   **Dashboard interno con métricas separadas:** conversión, ticket promedio, frecuencia, churn por cada perfil. Lo que funciona para uno puede no funcionar para el otro.

### 4.2. Por qué hacerlo desde el MVP (no agregarlo después)
Meter este split después es retrabajo grande:
*   El modelo de datos de cliente, pedido y cotización ya nace dual.
*   La fórmula de pricing tiene branching condicional (descuento por volumen, IVA mayorista, etc.).
*   El soporte y onboarding son productos distintos (auto-servicio vs asesor).
*   Si arrancás solo con particulares y después intentás sumar mayoristas, hay que reescribir el 40% del backend.

---

## 5. El Cotizador Híbrido: detalle técnico

El "Cotizador por Tramos" puro (tarifa plana solo en función de peso real y precio) se rompe en operación real por al menos cinco variables. La solución es un cotizador **híbrido** que automatiza lo cotizable y deriva lo complejo a manual.

### 5.1. Variables que ningún cotizador puede ignorar
*   **Peso volumétrico:** courier internacionales cobran por el mayor entre peso real y `L×W×H/5000`. 3 kg de tornillos vs 3 kg de almohadas tienen costos de flete que difieren 6-15×.
*   **Posición Arancelaria (NCM):** derecho de importación 0-35% + IVA 21% + IVA adicional 20% + Tasa Estadística + IIBB + Impuesto PAIS según régimen vigente.
*   **Restricciones específicas:** SENASA (alimentos), ANMAT (cosméticos / medicamentos), ENACOM / Resolución 92 SeCom (electrónica / dispositivos eléctricos), certificación COPANT / Resolución 404 (textiles), Resolución INTI (juguetes).
*   **Régimen Courier Argentina:** límite USD 3.000/envío, **12 envíos/año por CUIT**, máximo 50 kg, presunción comercial por cantidad de unidades idénticas.
*   **Tipo de cambio:** una devaluación del 15% durante 30-45 días de tránsito liquida el margen.

### 5.2. Diseño del Cotizador Híbrido

1. **Pre-cotizador con captura mínima de variables clave** (ver Sección 3.1).

2. **Tabla matriz multi-dimensional:** `categoría_NCM × peso_facturable × valor × ruta × tipo_cliente`.
   Fórmula: `fee_base + spread_arancelario(NCM) + cobertura_cambiaria(15-25%) + descuento_volumen(si tienda)`.

3. **Whitelist + Blacklist automática:**
   *   **Whitelist (cotizan automático):** repuestos automotores (NCM 87.08), bijouterie, herrajes, tornillería, accesorios de marroquinería, textiles sin etiquetado especial, herramientas manuales, decoración no voluminosa, accesorios de tecnología sin litio (cables, soportes, fundas).
   *   **Blacklist (derivan a cotización manual asistida):** electrónica con baterías de litio (drones, celulares, notebooks, herramientas eléctricas a batería), alimentos, cosméticos, medicamentos / suplementos, replicas / falsificaciones, juguetes para niños < 36 meses, productos voluminosos (>0,1 m³ declarado), envíos > $1.000 USD por cliente particular sin verificación previa.

4. **Cobertura cambiaria explícita:** precios mostrados en USD; cobro en pesos al TC del momento + spread del 15-25%; conversión inmediata T+0 a USDT/USDC vía exchange con buena liquidez (Binance, Bitso, Lemon Cash).

5. **Circuit breaker de margen:** auditoría automática post-envío comparando fee cobrado vs costo real. Si los últimos 10 envíos tuvieron margen < 10%, el cotizador automático se desactiva por categoría y deriva a manual hasta recalibrar.

### 5.3. Validación CUIT y cupo Courier (crítico)
Antes de cotizar, el sistema solicita CUIT del comprador y consulta cupo Courier remanente del año (auto-declaración asistida). Si el cliente pasó los 12 envíos/año:
*   **Particular:** sistema bloquea cotización Courier y propone esperar al año siguiente o usar régimen comercial (con despachante, costo mayor pero sin límite).
*   **Tienda mayorista:** sistema deriva automáticamente a régimen comercial (despacho a plaza), que es la modalidad correcta para volumen reventa.

### 5.4. Ingesta del link del producto (UX clave)
El cliente pega un link de Amazon / eBay / AliExpress / Mercado Livre Brasil. El sistema:
1. Extrae automáticamente título, precio, peso y dimensiones declaradas (si están disponibles vía API oficial — Amazon Product Advertising API, eBay Browse API).
2. Si el link es de un sitio sin API, deriva a captura manual de datos por el cliente.
3. **No scrapea de forma agresiva:** rate limit + uso de APIs oficiales para evitar bloqueos legales.

---

## 6. El Marketplace con Comisión 8-12%: por qué SÍ cierra ahora

### 6.1. Por qué fallaba el 3-5% del v1.0/v2.0
La pasarela cobra 2,9-6,29% + IVA. Una comisión 3-5% queda absorbida casi completamente por el banco, dejando margen neto cercano a cero o negativo. Esto se documentó en versiones anteriores.

### 6.2. Por qué cierra el 8-12%
*   **Pasarela:** ~6% promedio (Mercado Pago / Ualá Bis con IVA).
*   **Comisión bruta:** 10% (punto medio).
*   **Comisión neta para la plataforma:** **4%**.

Sobre un GMV de marketplace de $10.000 USD/mes, eso son $400 USD/mes de margen neto solo del marketplace. Suma. No es el motor del negocio (lo es el dropshipping), pero **deja de ser un sumidero**.

### 6.3. Posicionamiento competitivo
| Plataforma | Comisión | Diferencial |
|---|---|---|
| Mercado Libre | 13-17% según categoría | Tráfico masivo, logística Full |
| Tiendanube | 0% comisión + abono mensual | Tienda propia, sin tráfico orgánico |
| **Nuestra plataforma** | **8-12%** | **Cotizador + Dropshipping integrados + comunidad maker** |

El gancho para captar vendedores de ML es: "pagás menos comisión que en ML, **y además accedés a importar tus insumos a precio mayorista** desde el mismo panel".

### 6.4. Cómo se cobra (sin convertirse en agente de retención)
*   La pasarela cobra al comprador final.
*   La pasarela transfiere el monto neto al vendedor (descontando su propia comisión).
*   La plataforma cobra su comisión 8-12% **al vendedor**, vía Mercado Pago o débito directo, **después** del cobro al comprador (split payment).
*   La plataforma **no retiene fondos de terceros prolongadamente** → no califica como agente de retención bajo Resolución General 4622.
*   **Nota crítica:** validar este modelo con el asesor aduanero/contable; en algunos casos AFIP igual reclama responsabilidad. Documentar TyC con esa claridad.

---

## 7. Catálogo Curado vs Catálogo Tipo Tiendamia

### 7.1. Por qué NO se replica Tiendamia / Grabr
*   **Scrapping de Amazon/eBay** viola sus ToS y tiene rate limits agresivos. Bloqueo en semanas.
*   **Sincronización stock/precio en tiempo real** sobre millones de SKUs cuesta cinco cifras al mes.
*   **Markup dinámico** recalculado por SKU es un problema de escala que requiere equipo de 30+ personas (lo que tiene Tiendamia).

### 7.2. Estrategia correcta: Catálogo Curado + APIs oficiales
*   **Tienda Dropshipping (Componente 2):** 100-500 SKUs validados manualmente, mantenidos en Airtable/Sheets.
*   **Cotizador (Componente 1):** acepta cualquier link, pero usa **APIs oficiales** de Amazon (Product Advertising API), eBay (Browse API), AliExpress (Open Platform) para extraer datos. Las APIs oficiales son legales, gratuitas hasta cierto volumen y estables.
*   **Sin scrapping ad-hoc** bajo ningún escenario.

---

## 8. Arquitectura Técnica (Stack Escalonado)

### 8.1. Stack Fase 1 (MVP — meses 0-6)
*   **Frontend:** Next.js sobre Vercel (landing + cotizador + tienda + marketplace).
*   **BD & Auth:** Supabase (PostgreSQL + Auth + RLS + Storage).
*   **Microservicio cotizador:** Node.js o Python sobre Railway / Fly.io (cálculo + integración con APIs oficiales).
*   **Pagos:** Mercado Pago + Stripe (B2B internacional).
*   **Tracking:** AfterShip plan inicial.
*   **Comunicación:** WhatsApp Business API.
*   **Costo:** ~$200-350 USD/mes.

### 8.2. Stack Fase 2 (Marketplace consolidado — meses 6-12)
*   Observabilidad: Sentry + BetterStack.
*   Cache layer: Upstash Redis para cotizaciones y catálogo.
*   ERP de stock para vendedores (Supabase tablas + frontend Next.js).
*   **Costo:** ~$350-600 USD/mes.

### 8.3. Stack Fase 3 (Lotes de Demanda — meses 12-24, condicional)
El stack base no resuelve consolidación algorítmica. Se suma:
*   **Worker dedicado:** microservicio Python (FastAPI + OR-Tools) para optimización bin-packing multi-dimensional.
*   **Cola / orquestador:** Inngest, Trigger.dev o Temporal para workflows largos.
*   **Event sourcing inmutable** para auditoría aduanera.
*   **Costo adicional:** +$200-500 USD/mes.

### 8.4. Resumen de costos infra escalonados

| Fase | Burn-rate infra | Componentes principales |
|---|---|---|
| Fase 1 (MVP cotizador + tienda) | ~$200-350 USD/mes | Vercel + Supabase + microservicio + APIs |
| Fase 2 (marketplace + ERP) | ~$350-600 USD/mes | + cache + observabilidad + ERP vendedores |
| Fase 3 (Lotes de Demanda) | ~$550-1.100 USD/mes | + worker Python + cola + event sourcing |

---

## 9. Análisis Legal y Aduanero (Compliance)

### 9.1. Modelo Courier — Agente Facilitador
*   Mercadería ingresa bajo régimen Courier usando CUIT y cupo del **comprador final** (particulares) o régimen comercial con despachante (mayoristas).
*   TyC explícitos: servicio de gestión de compras y logística; sin garantía de fábrica; usuario asume responsabilidad por declaraciones.

### 9.2. Implicancias fiscales del marketplace con comisión
**Cambio frente a v2.0/v3.0:** el marketplace ahora SÍ procesa fee. Esto trae responsabilidades:
*   **Posible responsabilidad solidaria** bajo Resolución General 4622 si AFIP detecta vendedores no inscriptos operando en la plataforma.
*   **Mitigación:**
    *   Vendedor obligado a cargar CUIT/CUIL + condición ante IVA.
    *   Sistema verifica padrón AFIP al alta del vendedor (webservice padrón A4 / A5).
    *   TyC cláusula explícita: la plataforma es "intermediaria de pago" y los vendedores son responsables de sus propias obligaciones fiscales.
    *   **Asesor aduanero/contable es no opcional** — esto requiere armado prolijo desde el día 1.

### 9.3. Estructura legal multi-jurisdicción
*   **LLC en Delaware / Uruguay / Paraguay** para procesar fees B2B internacionales (cotizaciones de tiendas mayoristas, dropshipping).
*   **SRL/SAS en Argentina** para operación de marketplace local (la comisión se cobra desde acá).
*   **Logs forenses inmutables (event sourcing)** para defensa legal ante eventual fiscalización.

### 9.4. Diferenciación aduanera Particular vs Mayorista
| Aspecto | Particular | Mayorista |
|---|---|---|
| Régimen aduanero | Courier puerta a puerta | Courier (si entra en límites) o despacho a plaza |
| Cupo anual | 12 envíos / USD 3.000 c/u | Sin cupo si va por régimen comercial |
| Despachante | No requerido | Requerido en despacho a plaza |
| IVA importación | 21% + 20% adicional | Computable como crédito fiscal si el mayorista es Responsable Inscripto |
| Documentación | DNI + factura de origen | CUIT + factura de origen + DDJJ Aduanera + DJAI/SIRA |

---

## 10. Bypass Transaccional: tolerancia pasiva

Igual que en versiones anteriores: no perseguir, no facilitar activamente, ofrecer ERP genérico de inventario que sirve igual aunque la venta sea on o off platform. **Sin botón explícito "venta off-platform"** (riesgo legal de facilitación de evasión).

La diferencia v4.0 vs v2.0: ahora el marketplace cobra comisión real, así que el incentivo del vendedor a permanecer dentro de la plataforma es mayor (no solo es vidriera, es procesamiento de pago + protección al comprador + tracking).

---

## 11. Estrategia de Retención (Lock-in)

### 11.1. Lock-in informacional
La plataforma sabe qué fábrica origen produce el producto del vendedor; el vendedor no. Mientras la plataforma sea matchmaker entre vendedor local y origen, el bypass logístico es imposible.

### 11.2. Features condicionales
*   **Sello "Importador Verificado":** otorgado tras ≥3 importaciones B2B exitosas.
*   **Tarifas Tramo A (más baratas):** acceso con historial de actividad consistente.
*   **Microcréditos B2B:** condicionados a 90 días de actividad.
*   **Crédito acumulable** por reportar ventas externas (gamificación).

### 11.3. Diferenciación de retención por tipo de cliente
*   **Particulares:** retención por simpleza y plazo predecible. Notificaciones proactivas y tracking visual.
*   **Tiendas mayoristas:** retención por cuenta corriente, descuentos por volumen, reposición programada, asesor dedicado, financiamiento.

### 11.4. Alianzas estratégicas
*   Última milla local: alianzas con Andreani / Correo Argentino / OCA.
*   Integración con AFIP webservices: scraping autorizado de facturación electrónica (consentido) a cambio de descuento en fee logístico.

---

## 12. Métricas Clave (KPIs)

### 12.1. North Star Metrics (dual)
*   **B2C particular:** importaciones / visitantes únicos del cotizador (mensual).
*   **B2B mayorista:** ticket promedio × frecuencia × clientes mayoristas activos (LTV trimestral).

### 12.2. KPIs operativos críticos
*   Conversión visita → cotización: target ≥8% / mínimo viable ≥5%.
*   Conversión cotización → compra (particular): target ≥30%.
*   Conversión cotización → compra (mayorista): target ≥50% (más comprometidos por ticket).
*   Frecuencia de importación / cliente mayorista activo / año: target ≥6.
*   GMV del marketplace / mes: target ≥$10.000 USD a partir del mes 6.
*   Comisión neta del marketplace / mes: target ≥$400 USD a partir del mes 6.
*   Margen post-envío promedio: target ≥14%; activa circuit breaker si < 10%.
*   Tasa de incidentes aduaneros / envíos: target < 3%.

---

## 13. Estructura del Equipo Inicial

*   **Founder / Líder de Ventas y Comunidad:** outreach, contenido, gestión de mayoristas con asesoría dedicada (es lo que ya hace bien).
*   **Tech Lead / Desarrollador:** Frontend Next.js + Cotizador Híbrido + integraciones APIs. Puede ser part-time inicial.
*   **Broker Logístico / Operador B2B:** negociación con forwarders, consolidación, última milla.
*   **Soporte al Usuario (CX):** gestión proactiva de expectativas, atención post-venta, manejo de devoluciones.
*   **Asesor Aduanero / Contable (medio tiempo, $500-1.500 USD/mes):** **rol crítico no opcional** — particularmente importante con el marketplace cobrando comisión (responsabilidad solidaria AFIP).

---

## 14. Estructura de Costos (Burn Rate)

### 14.1. Burn-rate Fase 1 (MVP)

| Concepto | Costo mensual |
|---|---|
| Infraestructura tech | $200-350 USD |
| APIs (AfterShip, WhatsApp, Mercado Pago, Stripe) | $50-150 USD |
| Asesor aduanero / contable | $500-1.500 USD |
| Mantenimiento sociedad (LLC + SRL) | $150-400 USD |
| Pauta publicitaria experimental | $100-300 USD |
| **Subtotal** | **~$1.000-2.700 USD** |

Con sweat equity y founders sin sueldo, **burn-rate efectivo Fase 1: ~$1.000-1.500 USD/mes**.

### 14.2. Punto de Equilibrio

```
Burn-rate Fase 1: ~$1.200 USD/mes

Margen neto promedio:
  - Importación particular: ticket $150 × fee 14% = $21 USD
  - Importación mayorista: ticket $2.500 × fee 12% = $300 USD
  - Comisión marketplace: GMV × 4% neto

Mix realista mensual para break-even:
  - 20 importaciones particulares × $21 = $420 USD
  - 3 importaciones mayoristas × $300 = $900 USD
  - $5.000 USD GMV marketplace × 4% = $200 USD
  - Total: $1.520 USD → cubre burn-rate y deja margen
```

**Break-even alcanzable con:** 20 particulares + 3 mayoristas + $5.000 USD GMV marketplace al mes. Es exigente pero realista con la audiencia existente del founder.

---

## 15. Unit Economics

### 15.1. Variables del modelo (mix particular + mayorista + marketplace)

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

### 15.2. LTV por perfil (escenario realista)

```
LTV_particular = $150 × 1,5 × 14% × (1 / 0,40) = $79 USD
LTV_mayorista  = $2.000 × 5 × 14% × (1 / 0,40) = $3.500 USD
LTV_vendedor_marketplace (GMV $500/mes) = $500 × 12 × 4% × (1 / 0,40) = $600 USD
```

**Conclusión:** los **mayoristas son el motor de rentabilidad** (LTV 44× mayor que un particular). Pero los **particulares son el filtro masivo de adquisición** (alto volumen, bajo ticket → leads baratos para identificar futuros mayoristas).

### 15.3. Estrategia de precios
*   **Particulares:** fee de 14-18% sobre valor en origen.
*   **Mayoristas:** fee escalonado 10-14% (descuento por volumen incentivado).
*   **Marketplace:** comisión 8-12% sobre venta concretada.

---

## 16. Plan de Lanzamiento en Fases

| Fase | Mes | Producto | KPIs de cierre |
|---|---|---|---|
| **1 — MVP Cotizador + Tienda** | 0-3 | Cotizador Híbrido universal + tienda dropshipping curada + dual mode particular/mayorista | ≥20 importaciones particulares + 3 mayoristas/mes |
| **1.5 — Marketplace Beta** | 3-6 | Marketplace con 20-50 vendedores invitados + comisión 8-12% | GMV ≥$5.000 USD/mes |
| **2 — Marketplace Abierto + ERP** | 6-12 | Vidriera abierta a inscripciones + ERP de stock para vendedores + bot WhatsApp | GMV ≥$15.000 USD/mes; ≥30% de vendedores convierten a B2B |
| **3 — Lotes de Demanda (condicional)** | 12-24 | Worker Python + cola + cache + event sourcing | Fill Rate ≥75%; reducción costo unitario por kilo ≥20% |

---

## 17. Análisis de Riesgos Macroeconómicos (Contingencias)

### 17.1. Riesgo Aduanero (Cierre de Importaciones)
*   **Plan:** la empresa no quiebra (sin stock inmovilizado / sin deuda en dólares). Pivote: alianzas con importadores mayoristas locales.
*   **Mitigación adicional:** el marketplace local sigue funcionando incluso si las importaciones se traban, generando ingresos por comisión.

### 17.2. Riesgo Cambiario (Devaluación Brusca)
*   Cobertura cambiaria 15-25% en cotizador + conversión inmediata T+0 a USDT/USDC.

### 17.3. Riesgo Regulatorio del Marketplace (nuevo en v4.0)
*   **Escenario:** AFIP activa responsabilidad solidaria de la plataforma por evasión de vendedores.
*   **Mitigación:**
    *   Verificación obligatoria de CUIT + condición fiscal del vendedor al alta (webservice padrón AFIP).
    *   TyC explícitos: plataforma como intermediaria, vendedores responsables de sus obligaciones.
    *   Asesor contable monitorea cambios regulatorios.
    *   Estructura legal con sociedad local separada de la sociedad internacional (LLC procesa fees B2B; SRL solo procesa marketplace).

### 17.4. Riesgo de Concentración de Proveedores
*   Mínimo 2 forwarders por ruta + Smart Routing dinámico.

---

## 18. Veredicto de Riesgo: 3 Amenazas Críticas a 6 Meses

### Riesgo #1 — Quema de margen por mala calibración del Cotizador (Probabilidad: Alta)
**Por qué sigue siendo Alta en v4.0:** el cotizador universal cubre todos los rubros desde el día 1. Calibrar simultáneamente 10+ categorías de NCM es exponencialmente más complejo que calibrar 1 sola.

**Mitigación técnica:**
1. Whitelist conservadora al inicio (máximo 5-7 categorías de NCM); expansión gradual mes a mes.
2. Captura obligatoria de **dimensiones** además del peso (peso volumétrico).
3. Cobertura cambiaria 15-25% + liquidación T+0 a USDT/USDC.
4. **Circuit breaker de margen** por categoría: si una categoría tuvo margen < 10% en sus últimos 10 envíos, esa categoría cae a manual hasta recalibrar.
5. Auditoría automática post-envío con alerta a operaciones.
6. **Panel interno de "Health Check" del cotizador:** dashboard que muestra margen real vs predicho por categoría; señal temprana de calibración rota.

### Riesgo #2 — Asfixia regulatoria (AFIP / Aduana / pasarelas) (Probabilidad: Media-Alta)
**Mitigación técnica:**
1. Estructura legal multi-jurisdicción (LLC + SRL).
2. Compliance Layer: validación CUIT + cupo Courier antes de cotizar.
3. Verificación AFIP padrón al alta de vendedores del marketplace.
4. Logs forenses inmutables.
5. Smart Routing automático.
6. Asesor aduanero / contable medio tiempo no opcional.

### Riesgo #3 — Cuello de botella manual al escalar (Probabilidad: Media)
**Por qué es un riesgo nuevo en v4.0:** el cotizador automatiza la mayoría de los casos, pero la **blacklist deriva todo lo complejo a manual**. Si la blacklist es muy amplia, el founder sigue siendo el cuello de botella (la motivación original del proyecto desaparece).

**Mitigación técnica:**
1. Métrica de "tasa de fallback a manual" como KPI semanal: target < 25% de las cotizaciones.
2. Si la tasa supera 35%, sprint dedicado a expandir whitelist en categorías frecuentes.
3. Eventualmente, contratar un operador de cotizaciones manuales (junior, $400-800 USD/mes) que descomprima al founder mientras se calibran más NCMs.
4. Plantillas de respuesta rápida para casos manuales recurrentes (reduce tiempo de cotización manual de 20 min a 5 min).

---

## 19. Stress Test de Viabilidad

### Escenario Optimista
*   30 particulares × $30 margen = $900 + 5 mayoristas × $400 = $2.000 + GMV $20.000 × 6% neto = $1.200. **Total: $4.100 USD/mes.**
*   Burn-rate: $1.200 USD/mes. **Margen neto: $2.900 USD/mes.**

### Escenario Realista
*   20 particulares × $21 = $420 + 3 mayoristas × $300 = $900 + GMV $5.000 × 4% = $200. **Total: $1.520 USD/mes.**
*   Burn-rate: $1.200 USD/mes. **Margen neto: $320 USD/mes.** Modelo en break-even, sin margen para escalar marketing pago.

### Escenario Pesimista
*   10 particulares × $14 = $140 + 1 mayorista × $200 = $200 + GMV $1.500 × 2% = $30. **Total: $370 USD/mes.**
*   Burn-rate: $1.200 USD/mes. **Margen neto: -$830 USD/mes.** Modelo en pérdida, requeriría capital de runway.

**Conclusión:** el escenario realista es break-even apretado. La rentabilidad escala fuerte con número de mayoristas (1 mayorista = 14 particulares en margen). **La estrategia comercial debe priorizar la captación de mayoristas** desde el día 1, usando los particulares como filtro masivo de leads.

---

## 20. Requerimientos de Capital

### 20.1. Capital semilla (Friends & Family / Bootstrap)
*   **Monto sugerido:** $15.000-$30.000 USD.
*   **Runway esperado:** 6-9 meses de operación Fase 1 antes de break-even.
*   **Uso de fondos:**
    1. **Tecnología y Producto (40%):** Cotizador Híbrido + landing + marketplace beta + integración pasarelas y APIs.
    2. **Legal y Compliance (25%):** constitución multi-jurisdicción + asesor aduanero/contable (6 meses pre-pagados).
    3. **Marketing & Adquisición (20%):** outreach a vendedores ML + producción de contenido + pauta experimental.
    4. **Capital de Trabajo / Contingencia (15%):** fondo de reserva.

### 20.2. Etapa Pre-Seed (post Fase 2)
Solo después de validar marketplace + dropshipping en break-even sostenido.
*   **Monto sugerido:** $150.000-$300.000 USD.
*   **Hitos previos:**
    *   GMV marketplace ≥$15.000 USD/mes.
    *   ≥10 clientes mayoristas activos con frecuencia ≥3/año.
    *   LTV/CAC ≥3× sostenido por 2 trimestres.
    *   Margen post-envío ≥14% sostenido.

---

## 21. Anexo: Resumen de cambios v3.0 → v4.0

| Área | v3.0 | v4.0 |
|---|---|---|
| Tesis estratégica | Wedge solo-autos primero | Cotizador universal + marketplace + dropshipping integrados desde día 1 |
| ICP | Nicho automotriz | Horizontal (todos los rubros con whitelist NCM expandida gradualmente) |
| Marketplace | Fase 2 condicional | Componente core del MVP (Fase 1.5) |
| Comisión marketplace | N/A | 8-12% bruta (4% neta post-pasarela) |
| Diferenciación cliente | No definida | Particular vs Tienda Mayorista desde MVP (toggle inicial del cotizador) |
| Producto educativo (curso) | Componente core (Fase 0.5) | Removido — no encaja en la visión del founder |
| Whitelist NCM | NCM 87.08 (autopartes) primero | 5-7 categorías al MVP, expansión gradual |
| Stack MVP | Airtable/Sheets | Supabase desde día 1 (más complejidad de datos por dual mode) |
| Burn-rate MVP | ~$700-1.000 USD/mes | ~$1.000-1.500 USD/mes |
| Capital semilla | $8-15k | $15-30k (más superficie de producto) |
| Riesgo principal | Saturación del nicho | Calibración del cotizador en múltiples categorías simultáneas |

---

## Conclusión

La v4.0 alinea el plan con la visión real del founder: **automatizar el dolor concreto que ya vive todos los días** (el cotizador manual por WhatsApp), **expandir a todos los rubros** con un sistema técnicamente sólido (whitelist + manual fallback), y **monetizar tres frentes complementarios** (cotizador con fee logístico + tienda dropshipping + marketplace con comisión real 8-12%).

El producto diferencial **no es ni el marketplace ni el cotizador por separado** — es la **integración de los tres componentes con doble flujo cliente desde el MVP**. Ningún competidor argentino ofrece simultáneamente cotizador transparente + tienda pre-cotizada + marketplace local + diferenciación particular/mayorista.

El éxito depende de tres cosas concretas:
1. **Calibrar el cotizador conservadoramente** y expandir whitelist solo cuando los datos lo justifiquen.
2. **Captar mayoristas activamente** desde el día 1 (es donde está el LTV real).
3. **Medir tasa de fallback a manual** semanalmente para asegurar que el cotizador realmente esté liberando el tiempo del founder, no replicando el cuello de botella.

Si estas tres condiciones se sostienen, el modelo Fase 1 es break-even o superávit en 6-9 meses sin requerir capital externo significativo.
