---
layout: default
title: "Riesgos y Mitigaciones"
nav_order: 12
---

# 10 — Riesgos y Mitigaciones

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, inversores, asesor legal, equipo de producto.

---

## Top 3 Riesgos Críticos a 6 meses

### Riesgo #1 — Quema de margen por mala calibración del Cotizador
**Probabilidad:** Alta.
**Por qué:** el cotizador universal cubre múltiples categorías de NCM desde el día 1. Calibrar simultáneamente 10+ categorías es exponencialmente más complejo que calibrar 1 sola. Un solo envío de un producto voluminoso o de categoría con arancel 35% + certificación obligatoria puede liquidar el margen acumulado de 20-30 envíos rentables.

**Mitigación técnica:**
1. **Whitelist conservadora** al inicio (máximo 5-7 categorías de NCM); expansión gradual mes a mes solo con datos.
2. **Captura obligatoria de dimensiones** además del peso (peso volumétrico).
3. **Cobertura cambiaria 15-25%** + liquidación T+0 a USDT/USDC vía exchange con buena liquidez.
4. **Circuit breaker de margen por categoría:** si una categoría tuvo margen < 10% en sus últimos 10 envíos, esa categoría cae a manual hasta recalibrar.
5. **Auditoría automática post-envío** con alerta a operaciones (Slack/email).
6. **Panel "Health Check" del cotizador:** dashboard que muestra margen real vs predicho por categoría; señal temprana de calibración rota.
7. **Modo asistido los primeros 60 días:** founder confirma manualmente cada cotización antes de cobrar.

### Riesgo #2 — Asfixia regulatoria (AFIP / Aduana / pasarelas)
**Probabilidad:** Media-Alta.
**Por qué:** cambios de régimen Courier, cepos cambiarios, recorte de franquicia anual, congelamiento de fondos por sospecha de evasión, **activación de responsabilidad solidaria del marketplace** (Resolución General 4622).

**Mitigación técnica:**
1. **Estructura legal multi-jurisdicción:** LLC en Delaware/Uruguay/Paraguay para procesar fees B2B; SRL/SAS en Argentina solo para operación local.
2. **Compliance Layer en cotizador:** validación automática de CUIT + cupo Courier remanente antes de cotizar.
3. **Verificación AFIP padrón al alta** de vendedores del marketplace (webservice A4/A5).
4. **Logs forenses inmutables (event sourcing)** firmados con timestamp/hash; defensa legal ante eventual fiscalización.
5. **Smart Routing automático** entre rutas (Miami / Shanghai / SP) según restricciones detectadas en tiempo real.
6. **Tolerancia pasiva al bypass, no facilitación activa:** retiro de herramientas que parezcan inducir evasión (ningún botón "venta off-platform").
7. **Asesor aduanero / contable medio tiempo no opcional** ($500-1.500 USD/mes).

### Riesgo #3 — Cuello de botella manual al escalar
**Probabilidad:** Media.
**Por qué:** el cotizador automatiza la mayoría de los casos, pero **la blacklist deriva todo lo complejo a manual**. Si la blacklist es muy amplia, el founder sigue siendo el cuello de botella (la motivación original del proyecto desaparece).

**Mitigación técnica:**
1. **Métrica de tasa de fallback a manual** como KPI semanal: target < 25% de las cotizaciones.
2. **Si la tasa supera 35%**, sprint dedicado a expandir whitelist en categorías frecuentes.
3. **Eventualmente, contratar un operador de cotizaciones manuales** (junior, $400-800 USD/mes en Argentina) que descomprima al founder mientras se calibran más NCMs.
4. **Plantillas de respuesta rápida** para casos manuales recurrentes (reduce tiempo de cotización manual de 20 min a 5 min).

---

## Riesgos Macroeconómicos

### Riesgo Aduanero (cierre de importaciones)
**Escenario:** restricción del régimen Courier, nuevo cepo a divisas, recorte de franquicia anual.

**Mitigación:**
- Sin stock inmovilizado ni deuda en dólares → la empresa no quiebra.
- **Pivote local:** alianzas con importadores mayoristas locales (que ya tienen stock en el país) o fabricantes nacionales (B2B local), cobrando fee por matchmaking.
- **El marketplace local sigue funcionando** incluso si las importaciones se traban, generando ingresos por comisión.
- **Limitación del pivote:** elimina el moat informacional internacional. Es supervivencia, no estrategia.

### Riesgo Cambiario (devaluación brusca)
**Escenario:** devaluación abrupta licúa el margen cobrado en pesos durante los 30-45 días de tránsito.

**Mitigación:**
- Cobertura cambiaria 15-25% explícita en el cotizador.
- Conversión inmediata T+0 a USDT/USDC vía Binance / Bitso / Lemon Cash.
- Mínimo 2 exchanges contratados (no concentrar liquidación en uno solo).

### Riesgo de Concentración de Proveedores
**Escenario:** un solo forwarder o consolidador concentra > 50% del volumen y aumenta tarifas o quiebra.

**Mitigación:**
- Mínimo 2 forwarders por ruta, con allocación dinámica por costo y SLA.
- Smart Routing dinámico — el sistema cambia entre rutas en tiempo real.

---

## Riesgos Regulatorios Específicos del Marketplace

### Responsabilidad solidaria por evasión de vendedores (Resolución General 4622)
**Escenario:** AFIP detecta vendedores no inscriptos operando en la plataforma → la plataforma puede ser solidariamente responsable de la deuda fiscal.

**Mitigación:**
- Verificación obligatoria de CUIT + condición fiscal del vendedor al alta.
- Webservice padrón AFIP A4/A5 para validar automáticamente.
- TyC explícitos delimitando rol de intermediaria.
- Reverificación cada 6 meses.
- Asesor contable monitorea cambios regulatorios mensualmente.

### Operatoria cambiaria de cobertura USDT/USDC
**Escenario:** AFIP interpreta la diferencia de cambio entre cobro y conversión como ganancia financiera no declarada.

**Mitigación:**
- Conversión a través de la LLC offshore, no de la SRL local.
- Pesos cobrados en Argentina se transfieren a la LLC vía mecanismo legal.
- Operatoria validada con asesor cambiario antes de operar.

---

## Riesgos no nombrados explícitamente en versiones anteriores

### 1. Disponibilidad de APIs oficiales
La cotización automática depende de extraer datos de Amazon Product Advertising API, eBay Browse API, AliExpress Open Platform. Estas APIs **requieren aprobación previa** (caso de Amazon) y pueden rechazarte si no demostrás volumen.

**Mitigación:** plan B con captura manual de datos por el cliente desde el día 1. El cotizador NO debe depender 100% de las APIs para funcionar.

### 2. Riesgo del partner de exchange para liquidación T+0
Binance / Bitso / Lemon Cash están sujetos a regulación cambiaria local que puede cambiar de un día para otro. Un solo cambio regulatorio puede dejar tu liquidación congelada por semanas.

**Mitigación:** mínimo 2 exchanges + cuenta en partner offshore (Wise / Mercury con LLC).

### 3. Tiempo real de aprendizaje del cotizador
La calibración inicial probablemente esté **mal en 20-30% de los casos** durante los primeros 60 días. Esto es invisible hasta que los envíos llegan a aduana.

**Mitigación:** durante Fase 1 mantener el cotizador en **modo asistido** — el cliente ve la cotización pero el founder la confirma manualmente antes de cobrar. Después de 200-300 envíos exitosos, pasar a modo automático puro.

### 4. Competencia futura de Mercado Libre Importaciones
ML está experimentando con su propio servicio de cross-border B2C. Si lo escala, te aplasta en el segmento particular.

**Mitigación:**
- Profundidad en mayoristas (segmento que ML no atiende).
- Comunidad maker (lock-in cultural).
- Velocidad de iteración (startup vs gigante corporativo).
- Diferenciación por dual mode + cotizador transparente + comisión más baja.

### 5. Canibalización entre componentes
Un vendedor del marketplace que paga 8-12% de comisión y descubre que puede importar al por mayor por su cuenta usando el cotizador, eventualmente **deja de vender por la plataforma y solo usa el dropshipping**.

**Análisis:**
- Si el vendedor termina vendiendo on-platform: la plataforma pierde la comisión del marketplace pero gana el fee logístico (mayor en margen $) → neto positivo.
- Si el vendedor termina vendiendo off-platform: perdés ambas cosas.

**Mitigación:**
- Lock-in transversal: features condicionales (sello, tarifas Tramo A, microcréditos) requieren actividad consistente en plataforma.
- Comunidad maker como ancla cultural.
- ERP genérico de stock que sirve igual para ventas on/off platform → el vendedor sigue usando la plataforma como sistema operativo.

### 6. Operación dual duplica el costo de soporte
Atender 30 particulares con tracking automático es barato; atender 3 mayoristas con asesor dedicado, factura A, despachante y reposición programada es **otro tipo de trabajo**. Esto presiona el equipo a sumar un Account Manager B2B antes de lo previsto.

**Mitigación:** sumar B2B Manager en Fase 2 (mes 6+, $600-1.200 USD/mes). Detalle en [11 — Equipo y Operaciones](11-equipo-y-operaciones.md).

---

## Riesgos del plan (operativos)

### Atraso de Fase 1.5 (Marketplace Beta)
Probable atraso de 2-3 meses respecto al plan original.
**Mitigación:** empezar el outreach a vendedores en paralelo a Fase 1; tener el marketplace listo en código aunque sin usuarios.

### Sobre-construcción en Fase 2
ERP + bot + microcréditos + AFIP + cache es mucho para un trimestre.
**Mitigación:** priorizar ERP y bot WhatsApp; microcréditos pueden esperar a Fase 3.

### Tentación de saltar al marketplace abierto antes de cumplir KPIs Fase 1.5
**Mitigación:** validar antes de abrir; mantener disciplina de cumplir KPIs.

---

## Veredicto de Riesgo

**El proyecto es viable bajo cuatro condiciones operativas:**

1. **Disciplina de scope.** Construir Cotizador (mes 0-3), después marketplace beta (mes 3-6), después marketplace abierto (mes 6-12). No paralelizar.

2. **Plan explícito de adquisición de mayoristas desde el día 1.** Sin esto, el LTV proyectado se derrumba y el modelo cae al stress test pesimista. Detalle en [13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md).

3. **Aprovechar la audiencia de autos como canal de entrada del cotizador** aunque el producto sea universal. CAC orgánico real subutilizado.

4. **Modo asistido los primeros 60 días** del cotizador automático antes de pasar a auto puro. Sin esto, el riesgo de quema de margen se materializa con alta probabilidad.

**Si las cuatro se cumplen:** break-even sostenible en 6-9 meses, base sólida para Pre-Seed con métricas reales en mes 12-15.

**Si fallan dos o más:** el proyecto entra en zona de "freemium trap" — muchos visitantes en el cotizador, marketplace con tráfico bajo, pocos mayoristas, y el founder vuelve a ser el cuello de botella manual que quería evitar. En ese escenario el burn-rate consume el capital semilla en 4-6 meses sin tracción.

---

## Próximas lecturas

- [09 — Plan de Lanzamiento](09-plan-lanzamiento.md): cómo se mitigan los riesgos operativamente en cada fase.
- [13 — Adquisición de Mayoristas](13-adquisicion-mayoristas.md): el plan que cierra el agujero crítico de captación B2B.
- [12 — Capital y Financiamiento](12-capital-y-financiamiento.md): cuándo levantar capital con métricas reales.
