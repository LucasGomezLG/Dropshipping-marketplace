# 07 — Compliance Legal

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.1 — 2026-04-28
> **Audiencia primaria:** founder, asesor aduanero/contable, asesor legal, inversores con foco en riesgo regulatorio.

---

## Principio rector

Operar como **Agente Facilitador**, nunca como Importador Registrado de mercadería de terceros. La empresa nunca inmoviliza capital en stock importado a su nombre. La mercadería ingresa siempre con CUIT del comprador final.

Esto desactiva la mayoría de los riesgos aduaneros y fiscales de raíz.

---

## Modelo Courier (Agente Facilitador)

### Marco legal
- Mercadería ingresa bajo régimen Courier / Puerta a Puerta usando **CUIT del comprador final** (particulares) o régimen comercial con despachante (mayoristas grandes).
- Plataforma brinda "servicio de gestión de compras en el exterior y logística".
- Garantía del producto: del fabricante extranjero, no de la plataforma.
- Responsabilidad por declaraciones aduaneras: del comprador.

### TyC explícitos
Los TyC del cotizador deben establecer claramente:
1. La plataforma es **intermediaria de servicios logísticos**, no vendedora del producto.
2. La factura de origen es del proveedor extranjero, no de la plataforma.
3. Errores en declaración aduanera por parte del usuario son responsabilidad del usuario.
4. Posibles retenciones aduaneras adicionales (por valor declarado, NCM erróneo, restricciones específicas) corren por cuenta del usuario.
5. Plazos de tránsito son estimativos; demoras aduaneras o logísticas no generan derecho a reembolso del fee.

### Validación CUIT y cupo Courier
- Auto-declaración asistida del cliente sobre cupo Courier remanente (Fase 1).
- Webservice padrón AFIP A4/A5 (Fase 2) para validación automática de CUIT y condición fiscal.
- Si el cliente pasó los **12 envíos/año** (límite Courier):
  - **Particular:** sistema bloquea cotización Courier; propone esperar al año siguiente o derivar a régimen comercial.
  - **Mayorista:** sistema deriva automáticamente a régimen comercial con despachante.

---

## Implicancias fiscales del marketplace con comisión

### Cambio respecto a versiones anteriores
La v1.0 / v2.0 evitaban procesar pagos del marketplace para escapar de implicancias fiscales. La v4.0 cobra comisión 8-12% real, lo que **trae responsabilidades nuevas que hay que blindar**.

### Resolución General 4622 AFIP — responsabilidad solidaria
- Si AFIP detecta vendedores no inscriptos operando en la plataforma, **la plataforma puede ser solidariamente responsable** de la deuda fiscal.
- Esto es real y aplicable. No es un riesgo teórico.

### Mitigaciones obligatorias

1. **Verificación obligatoria de CUIT/CUIL + condición ante IVA al alta del vendedor.**
   - Vendedor sin inscripción ante AFIP no puede dar de alta su tienda.
   - Excepción para Monotributo Social documentado.

2. **Webservice padrón AFIP A4/A5** consultado al alta y reverificado cada 6 meses.

3. **TyC explícitos del marketplace:**
   - "La plataforma actúa como intermediaria de pago entre comprador y vendedor."
   - "Cada vendedor es responsable exclusivo de sus obligaciones fiscales (IVA, IIBB, Ganancias)."
   - "La plataforma se reserva el derecho de suspender vendedores con observaciones de inscripción ante AFIP."

4. **Asesor aduanero / contable medio tiempo no opcional.**
   - Costo: $500-1.500 USD/mes según jurisdicción.
   - Monitorea cambios regulatorios mensuales.
   - Revisa TyC trimestralmente.
   - Defiende ante eventual fiscalización.

---

## Estructura legal multi-jurisdicción

Para minimizar exposición fiscal y burocracia operativa, se usa estructura dual:

### LLC en Delaware / Uruguay / Paraguay
- **Propósito:** procesar fees B2B internacionales (cotizaciones de mayoristas, dropshipping, suscripciones futuras).
- **Pagos:** Stripe, Wise, cripto-pasarelas internacionales.
- **Ventaja:** queda fuera del régimen cambiario argentino para los flujos internacionales legítimos.

### SRL / SAS en Argentina
- **Propósito:** operación local — marketplace, alianzas con correos, soporte local.
- **Cobro de comisión del marketplace** se hace desde acá (la pasarela es Mercado Pago argentino).
- **Inscripción tributaria** completa (IVA, IIBB, Ganancias).

### Logs forenses inmutables
- Tabla `events_audit` append-only con hash encadenado.
- Retención mínima 5 años (cumplimiento aduanero).
- Cada cotización, cada pago, cada cambio de estado de un envío queda registrado de forma indeleble.
- **Defensa legal ante eventual fiscalización** AFIP / Aduana.

---

## Diferenciación aduanera particular vs mayorista

| Aspecto | Particular | Mayorista |
|---|---|---|
| Régimen aduanero | Courier puerta a puerta | Courier (≤ USD 3.000) o despacho a plaza con despachante |
| Cupo anual | 12 envíos / USD 3.000 c/u | Sin cupo si va por régimen comercial |
| Despachante | No requerido | Requerido en despacho a plaza |
| IVA importación | 21% + 20% adicional (no recuperable) | Computable como crédito fiscal si es Responsable Inscripto |
| Documentación | DNI + factura de origen | CUIT + factura de origen + DDJJ Aduanera + DJAI/SIRA si aplica |
| Tiempo de despacho | 5-15 días post arribo | 7-30 días post arribo |
| Canal de entrega | Correo Argentino / Andreani directo | Logística B2B coordinada |

Detalle complementario en [05 — Clientes Dual Mode](05-clientes-dual-mode.md).

---

## Restricciones específicas por categoría

Estos productos requieren autorizaciones, certificaciones o no pueden ingresar bajo régimen Courier estándar:

| Organismo | Productos | Implicancia |
|---|---|---|
| **SENASA** | Alimentos, bebidas, productos de origen animal o vegetal | Certificación + inspección sanitaria |
| **ANMAT** | Cosméticos, productos médicos, medicamentos, suplementos | Habilitación específica |
| **ENACOM / Resolución 92 SeCom** | Electrónica, dispositivos eléctricos | Certificación de seguridad eléctrica |
| **COPANT / Resolución 404** | Textiles | Etiquetado obligatorio |
| **INTI** | Juguetes para niños < 36 meses | Certificación de seguridad |

**Estos productos están en blacklist permanente del cotizador.** Detalle en [02 — Producto Cotizador](02-producto-cotizador.md).

---

## Cobertura cambiaria — implicancia legal

La operatoria de cobertura cambiaria con liquidación T+0 a USDT/USDC tiene matices regulatorios:
- En Argentina, comprar y vender criptoactivos para uso interno está permitido.
- La declaración de ganancias por diferencia de cambio (entre el momento del cobro y el momento de la conversión) puede ser interpretada como ganancia financiera por AFIP.
- **Mitigación:** la conversión se hace a través de la LLC offshore, no de la SRL local. Los pesos cobrados en Argentina se transfieren a la LLC vía mecanismo legal (Wise, exchange con liquidación cripto) y desde allí se opera en USDT/USDC.

**Esta operatoria debe ser validada con asesor cambiario antes de operar.** No es zona pacífica regulatoriamente.

---

## Bypass transaccional — postura legal

- **Tolerancia pasiva:** la plataforma no persigue ni penaliza el cierre de venta por WhatsApp / off-platform.
- **No facilitación activa:** la plataforma **no construye herramientas que parezcan facilitar evasión** (ningún botón "venta off-platform"; ningún bot que descuente stock por venta externa explícita).
- Lo que sí se ofrece: **ERP de inventario genérico** que sirve igual aunque la venta sea on o off platform.

Detalle de razonamiento en [10 — Riesgos y Mitigaciones](10-riesgos-y-mitigaciones.md).

---

## Compliance del producto formación / curso (referencia histórica)

El curso $50-$100 fue parte del v3.0 y se descartó en v4.0. Si en algún momento se reactivara, las consideraciones legales son:
- Sin promesas de retorno financiero (caería bajo regulación de oferta pública / publicidad engañosa).
- Facturación correcta (LLC si es internacional, SRL si es local).
- Considerar plataformas tipo Hotmart / Stripe Atlas para simplificar fiscalmente.

---

## Próximas lecturas

- [05 — Clientes Dual Mode](05-clientes-dual-mode.md): diferenciación aduanera detallada.
- [10 — Riesgos y Mitigaciones](10-riesgos-y-mitigaciones.md): riesgos macro y regulatorios con mitigaciones técnicas.
- [03 — Producto Marketplace](03-producto-marketplace.md): split payment y responsabilidad fiscal del marketplace.
