---
layout: default
title: "Arquitectura Técnica"
nav_order: 8
---

# 06 — Arquitectura Técnica (Stack Definitivo)

> **Documento parte de:** [Plataforma Cotizador + Marketplace + Dropshipping](../README.md)
> **Versión:** 4.3 — 2026-04-28 (Refactor: Stack Serverless Optimizado)
> **Audiencia primaria:** Tech Lead, desarrolladores, inversores.

---

## Principio rector: "Escalabilidad Serverless con Foco en el Margen"

El stack elegido prioriza la **velocidad de desarrollo** y el **costo operativo $0 inicial**, sin sacrificar la robustez necesaria para manejar transacciones financieras y trámites aduaneros. Se opta por una arquitectura **Full-stack Serverless**.

---

## 1. El Stack Core (La Santísima Trinidad)

### Frontend: Next.js (App Router) + TypeScript
- **TypeScript (Mandatorio):** No se acepta JavaScript puro. Al manejar cálculos de aranceles, impuestos y dinero, la seguridad de tipos es crítica para evitar errores catastróficos.
- **App Router:** Uso de Server Components para rendimiento máximo y SEO (vital para el marketplace).
- **Estilizado:** **Vanilla CSS / CSS Modules**. Se evita Tailwind CSS para mantener un control total sobre el diseño "premium" y evitar deuda técnica estética a largo plazo.

### Backend-as-a-Service: Supabase (PostgreSQL)
- **PostgreSQL:** Base de datos relacional robusta para gestionar pedidos, usuarios y cotizaciones.
- **Auth & RLS:** Gestión nativa de usuarios y reglas de seguridad a nivel de fila (Row Level Security) para aislar datos entre particulares, mayoristas y vendedores.
- **Edge Functions:** Para lógica ligera que debe ejecutarse cerca del usuario.

### Orquestación de Flujos: Next.js API Routes + Inngest
- **API Routes:** Para la lógica del Cotizador Híbrido y procesamiento de pagos.
- **Inngest (Clave):** Dado que un proceso de importación dura entre 15 y 45 días, se requiere un orquestador de flujos de larga duración. Inngest permite programar lógica que "duerme" y se activa ante eventos (ej. "si el flete no cambió de estado en 7 días, disparar alerta").

---

## 2. Infraestructura y Despliegue

| Componente | Proveedor | Justificación |
|---|---|---|
| **Hosting & Edge** | **Vercel** | Despliegue atómico, CDN global y excelente integración con Next.js. |
| **Base de Datos** | **Supabase** | Backend completo, Postgres gestionado y escalabilidad elástica. |
| **Workflows** | **Inngest** | Gestión de procesos asincrónicos y de larga duración (importación). |
| **Cache** | **Upstash Redis** | Cache de baja latencia para cotizaciones frecuentes y catálogo curado. |

---

## 3. Integraciones Críticas (Ecosistema)

### Pagos y Finanzas
- **Mercado Pago API:** Split payment para el marketplace y cobros locales en Argentina.
- **Stripe:** Cobro de fees logísticos a mayoristas internacionales.
- **Binance / Bitso / Lemon API:** Consulta de cotización USDT/USDC en tiempo real para asegurar la **Cobertura Cambiaria T+0**.

### Logística y Comunicación
- **AfterShip API:** Tracking unificado de 900+ couriers internacionales.
- **WhatsApp Business API (vía Twilio/Meta):** Notificaciones críticas y soporte prioritario para mayoristas.
- **Resend / Mailgun:** Email transaccional con alta entregabilidad.

---

## 4. Evolución de la Arquitectura (Fases)

### Fase 1 (MVP - Digitalización)
- Foco: Cotizador Híbrido + Checkout + Tracking básico.
- Costo infra: **~$0 - $50 USD/mes** (usando tiers gratuitos de Vercel y Supabase).

### Fase 2 (Escala B2B y Marketplace)
- Foco: Panel de Vendedores + ERP de Stock + Panel Mayorista.
- Incorporación de **Inngest** para flujos de cobro y seguimiento de 45 días.
- Costo infra: **~$150 - $300 USD/mes**.

### Fase 3 (Optimización Algorítmica)
- Foco: Lotes de Demanda (Bin-packing).
- Incorporación de un microservicio en **Python (FastAPI)** solo si la complejidad de optimización matemática lo requiere.

---

## 5. Seguridad y Compliance Técnico

1. **Cero persistencia de tarjetas:** Delegación total a pasarelas (PCI-DSS compliance).
2. **Logs forenses:** Cada cambio de estado en una cotización genera un evento inmutable en Supabase.
3. **Variables de Entorno (Secrets):** Gestión estricta vía Vercel/Supabase Vault; nunca en el repositorio.
4. **Auditoría de Margen:** Middleware que compara el `calculated_fee` con el `actual_cost` post-despacho y genera alertas automáticas.

---

## 6. North Star Técnica

> **"Un sistema que se estira ante el tráfico, pero no cuesta nada cuando el founder duerme."**

Este stack permite al proyecto Anti-ML competir en tecnología con gigantes como Tiendamia, operando con una fracción ínfima de su presupuesto de ingeniería.
