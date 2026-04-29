# 02 — Producto: Cotizador Híbrido

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** Tech Lead, founder, asesor aduanero.

---

## Por qué un Cotizador Híbrido y no un "Cotizador por Tramos"

El primer impulso es proponer una tarifa plana por peso y precio (Tramo A: hasta $100 USD/3kg → fee fijo $10; Tramo B: > $500 USD → fee fijo $50). Es elegante en PowerPoint pero **se rompe en operación real** por al menos cinco variables que ningún cotizador puede ignorar:

### 2.1. Peso volumétrico (la trampa #1)
Las courier internacionales (DHL, UPS, FedEx) cobran por el **mayor entre peso real y peso volumétrico** (`L×W×H/5000` cm). Ejemplos:
- 3 kg de tornillos (volumen 0,001 m³) → ganás.
- 3 kg de almohadas (volumen 0,2 m³) → el flete real cuesta **6-15× tu fee**.

Un fee plano basado solo en peso real puede generar pérdidas masivas en productos voluminosos. Un solo envío mal asignado de un sillón inflable o decoración voluminosa puede comerse el margen de 20 envíos rentables.

### 2.2. Posición Arancelaria (NCM / HS Code)
Derecho de importación 0-35% según NCM, más:
- Tasa estadística 3% (con tope).
- IVA importación 21% + 20% adicional anticipado.
- IIBB retención ~3%.
- Impuesto PAIS / retenciones cripto-cambiarias según régimen vigente.
- DJAI / SIRA: trabas administrativas.

Un fee plano no puede absorber simultáneamente bijouterie sin restricción y electrónica con litio (35% arancel + LCM + Resolución 92 SeCom para seguridad eléctrica).

### 2.3. Restricciones específicas
- **SENASA:** alimentos, bebidas, productos de origen animal o vegetal.
- **ANMAT:** cosméticos, productos médicos, medicamentos, suplementos.
- **ENACOM / Resolución 92 SeCom:** electrónica, dispositivos eléctricos.
- **Certificación COPANT / Resolución 404:** textiles.
- **Resolución INTI:** juguetes para niños menores de 36 meses.

### 2.4. Régimen Courier Argentina (límites duros)
- Límite USD 3.000/envío.
- **12 envíos/año por CUIT.**
- Máximo 50 kg.
- Presunción comercial por cantidad de unidades idénticas.

### 2.5. Tipo de cambio
Una devaluación del 15% durante 30-45 días de tránsito **liquida el margen** si no hay cobertura.

---

## Diseño del Cotizador Híbrido

### Flujo del cotizador

1. **Toggle inicial:** "soy particular" / "soy tienda" (define todo el flujo posterior — ver [05](05-clientes-dual-mode.md)).
2. **Categoría obligatoria** desde dropdown limitado a NCMs validados (whitelist).
3. **Captura de variables clave:**
   - Link del producto (Amazon, eBay, AliExpress, Mercado Livre Brasil).
   - Peso declarado.
   - Dimensiones declaradas (alto × ancho × largo) — el sistema calcula peso volumétrico y toma el mayor entre real y volumétrico.
   - Valor en origen (USD).
   - País de origen.
   - Cantidad de unidades.
4. **Cálculo automático:** `precio_origen + arancel(NCM) + IVA + IVA_adicional + tasa_estadística + IIBB + flete + fee_logístico + cobertura_cambiaria`.
5. **Resultado:** precio total en USD y ARS, plazo estimado de entrega, tracking previsto, condiciones legales.
6. **CTA:** "Confirmá y pagá" o "Guardar cotización para después" (la cotización tiene validez 48-72hs por riesgo cambiario).

### Tabla matriz multi-dimensional
`categoría_NCM × peso_facturable × valor × ruta × tipo_cliente`

Fórmula:
```
fee_base
  + spread_arancelario(NCM)
  + cobertura_cambiaria(15-25%)
  + descuento_volumen(si tipo_cliente = wholesale)
```

---

## Whitelist + Blacklist (criterio operativo)

### Whitelist Fase 1 (cotizan automático)
Categorías de bajo riesgo y alta densidad de valor:
- **Repuestos automotores** (NCM 87.08) — partes y accesorios de vehículos.
- **Bijouterie y accesorios** (sin metales preciosos > 1g).
- **Herrajes, tornillería, ferretería pequeña.**
- **Accesorios de marroquinería.**
- **Textiles sin etiquetado especial.**
- **Herramientas manuales** (no eléctricas, no a batería).
- **Decoración no voluminosa** (< 0,1 m³).
- **Accesorios de tecnología sin litio:** cables, soportes, fundas, organizadores.

### Blacklist permanente (derivan a manual asistido)
- **Electrónica con baterías de litio:** drones, celulares, notebooks, herramientas eléctricas a batería, power banks.
- **Alimentos, bebidas, suplementos** (SENASA).
- **Cosméticos, medicamentos** (ANMAT).
- **Replicas / falsificaciones.**
- **Juguetes para niños < 36 meses** (Resolución INTI).
- **Productos voluminosos** (> 0,1 m³ declarado).
- **Envíos > $1.000 USD por cliente particular** sin verificación previa de cupo Courier.
- **Envíos que rompen ratios de "uso personal"** (ej. 50 unidades idénticas → presunción comercial).

### Expansión de la whitelist
La whitelist se expande mes a mes solo cuando los datos de margen real lo justifican. **Nunca abrir una categoría nueva por intuición.** El criterio: ≥10 envíos exitosos manuales con margen ≥14% en esa categoría → candidata a whitelist automatizada.

---

## Cobertura cambiaria

- Precios mostrados en USD, cobro en pesos al TC del momento + spread del 15-25%.
- **Conversión inmediata T+0** a USDT/USDC vía exchange con buena liquidez (Binance, Bitso, Lemon Cash).
- Riesgo cambiario durante 30-45 días de tránsito → neutralizado.
- Mínimo 2 exchanges contratados para evitar concentración de liquidación.

---

## Circuit breaker de margen

Auditoría automática post-envío comparando fee cobrado vs costo real:
- Si los últimos 10 envíos de **una categoría** tuvieron margen < 10% → categoría se desactiva del cotizador automático y deriva a manual hasta recalibrar.
- Si los últimos 10 envíos **totales** tuvieron margen < 8% → cotizador automático completo se desactiva temporalmente.
- Alerta a operaciones por Slack / email.

**Panel interno de "Health Check":** dashboard que muestra margen real vs predicho por categoría; señal temprana de calibración rota antes de quemar mucho margen.

---

## Validación CUIT y cupo Courier (crítico)

Antes de cotizar, el sistema solicita CUIT del comprador y consulta cupo Courier remanente del año:
- **Auto-declaración asistida** del cliente (Fase 1).
- **Webservice padrón AFIP A4/A5** cuando esté disponible (Fase 2).

Si el cliente pasó los 12 envíos/año:
- **Particular:** sistema bloquea cotización Courier y propone esperar al año siguiente o derivar a régimen comercial (otro producto, otro fee).
- **Mayorista:** sistema deriva automáticamente a régimen comercial (despacho a plaza con despachante), que es la modalidad correcta para volumen reventa.

---

## Ingesta del link del producto

El cliente pega un link de Amazon, eBay, AliExpress o Mercado Livre Brasil. El sistema:

1. Extrae automáticamente título, precio, peso y dimensiones declaradas vía **APIs oficiales** (no scrapping):
   - Amazon: Product Advertising API (requiere aprobación previa, plan B con captura manual).
   - eBay: Browse API.
   - AliExpress: Open Platform API.
2. Si el link es de un sitio sin API integrada, deriva a **captura manual de datos por el cliente**.
3. **Sin scrapping ad-hoc** bajo ningún escenario — viola ToS y se bloquea en semanas.

---

## Modo asistido los primeros 60 días (decisión clave)

Durante los primeros 60 días post-lanzamiento del cotizador automático:
- El cliente ve la cotización inmediata.
- **El founder confirma manualmente** la cotización antes de cobrar (revisión humana).
- Se acumula data de calibración real.

**Por qué:** la calibración inicial probablemente esté mal en 20-30% de los casos durante los primeros 60 días. Esto es invisible hasta que los envíos llegan. El modo asistido captura esos errores antes de cobrar de menos.

**Después de 200-300 envíos exitosos** con calibración estable → pasar a modo automático puro (sin confirmación humana).

Este es uno de los compromisos clave del modelo (ver [10 — Riesgos y Mitigaciones](10-riesgos-y-mitigaciones.md)).

---

## KPIs específicos del cotizador

- **Tasa de fallback a manual** (target < 25% de las cotizaciones).
- **Conversión visita → cotización** (target ≥ 8% / mínimo viable ≥ 5%).
- **Conversión cotización → compra** (≥ 30% particular, ≥ 50% mayorista).
- **Margen post-envío promedio** (target ≥ 14%; circuit breaker < 10%).
- **Tasa de incidentes aduaneros** (< 3%).

---

## Próximas lecturas

- [05 — Clientes Dual Mode](05-clientes-dual-mode.md): cómo cambia el flujo del cotizador según el toggle inicial.
- [06 — Arquitectura Técnica](06-arquitectura-tecnica.md): qué stack soporta el cotizador.
- [07 — Compliance Legal](07-compliance-legal.md): por qué la validación CUIT y la blacklist son blindajes legales.
