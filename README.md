---
layout: default
title: "Plataforma Cotizador + Marketplace + Dropshipping"
nav_order: 1
permalink: /
---

# Plataforma Cotizador + Marketplace + Dropshipping

> **Versión maestra:** 4.1 — 2026-04-28
> **Estado:** documentación de viabilidad técnica y estratégica, en preparación para ronda semilla.

**Frase única del proyecto:** una plataforma argentina que automatiza el proceso de importación bajo régimen Courier para particulares y tiendas, complementada con un marketplace local de comisión competitiva, monetizando tres flujos integrados (fee logístico, dropshipping pre-cotizado y comisión de marketplace).

---

## Estructura del repositorio

```
.
├── README.md                          ← este archivo (índice maestro + home de GitHub Pages)
├── _config.yml                        ← configuración Jekyll para GitHub Pages
├── docs/
│   ├── 00-resumen-ejecutivo.md         ← Para inversores / decisión rápida (1 página)
│   ├── 01-vision-y-tesis.md            ← Problema real, activos, diferencial competitivo
│   ├── 02-producto-cotizador.md        ← Cotizador Híbrido (núcleo del sistema)
│   ├── 03-producto-marketplace.md      ← Comisión 8-12%, posicionamiento, split payment
│   ├── 04-producto-tienda.md           ← Catálogo curado vs Tiendamia, APIs oficiales
│   ├── 05-clientes-dual-mode.md        ← Particular vs Mayorista (transversal)
│   ├── 06-arquitectura-tecnica.md      ← Stack escalonado Fase 1 / 2 / 3
│   ├── 07-compliance-legal.md          ← LLC + SRL, Resolución 4622, asesor aduanero
│   ├── 08-finanzas-unit-economics.md   ← LTV, CAC, break-even, stress test
│   ├── 09-plan-lanzamiento.md          ← Fases con KPIs y deliverables
│   ├── 10-riesgos-y-mitigaciones.md    ← Top 3 + macro + regulatorio + no nombrados
│   ├── 11-equipo-y-operaciones.md      ← Roles, burn rate, soporte dual
│   ├── 12-capital-y-financiamiento.md  ← $15-30k semilla + hitos Pre-Seed
│   ├── 13-adquisicion-mayoristas.md    ← Plan B2B operativo
│   └── 14-pitch-empresas.md            ← Propuesta comercial para empresas
└── archive/
    └── analisis_marketplace_v4_monolitico.md   ← Backup consolidado del documento original
```

---

## Orden de lectura sugerido por audiencia

### Para inversores / Friends & Family (15-20 min)
1. [00 — Resumen Ejecutivo](docs/00-resumen-ejecutivo.md)
2. [01 — Visión y Tesis](docs/01-vision-y-tesis.md)
3. [08 — Finanzas y Unit Economics](docs/08-finanzas-unit-economics.md)
4. [10 — Riesgos y Mitigaciones](docs/10-riesgos-y-mitigaciones.md)
5. [12 — Capital y Financiamiento](docs/12-capital-y-financiamiento.md)

### Para Tech Lead / Desarrolladores (25-35 min)
1. [01 — Visión y Tesis](docs/01-vision-y-tesis.md)
2. [02 — Producto Cotizador](docs/02-producto-cotizador.md)
3. [05 — Clientes Dual Mode](docs/05-clientes-dual-mode.md)
4. [06 — Arquitectura Técnica](docs/06-arquitectura-tecnica.md)
5. [09 — Plan de Lanzamiento](docs/09-plan-lanzamiento.md)

### Para Asesor Legal / Contable (15-20 min)
1. [03 — Producto Marketplace](docs/03-producto-marketplace.md) (sección split payment)
2. [05 — Clientes Dual Mode](docs/05-clientes-dual-mode.md) (sección aduanera)
3. [07 — Compliance Legal](docs/07-compliance-legal.md)
4. [10 — Riesgos y Mitigaciones](docs/10-riesgos-y-mitigaciones.md) (sección regulatoria)

### Para Founder / Operación diaria (lectura completa)
Leer en orden 00 → 13.

### Para Sales / Adquisición B2B
1. [00 — Resumen Ejecutivo](docs/00-resumen-ejecutivo.md)
2. [05 — Clientes Dual Mode](docs/05-clientes-dual-mode.md)
3. [13 — Adquisición de Mayoristas](docs/13-adquisicion-mayoristas.md)
4. [14 — Propuesta B2B a Empresas](docs/14-pitch-empresas.md)

---

## Tabla de versiones

| Versión | Fecha | Cambios principales |
|---|---|---|
| 1.0 | inicial | Marketplace 3-5% como Caballo de Troya. **Inviable financieramente.** |
| 2.0 | corrección financiera | Marketplace 0% comisión + SaaS Freemium + Cotizador Híbrido. Viable pero ICP horizontal sin foco. |
| 3.0 | pivot estratégico | Wedge solo-autos + curso onboarding pago. Bootstrappable pero scope estrecho. |
| 4.0 | visión del founder | Cotizador universal + tienda + marketplace 8-12% con dual mode particular/mayorista. |
| 4.1 | 2026-04-28 | Refactor a documentos modulares + agregado del plan de adquisición B2B. |
| **4.2** | **2026-04-28** | **Refactor Roadmap: Foco radical en automatización de flujo manual y escala de audiencia (Autos → Universal).** |

---

## Estado actual (2026-04-28)

**Decisiones cerradas:**
- Cotizador automático universal (no wedge vertical).
- Comisión marketplace 8-12% (no 3-5%, no 0%).
- Dual mode particular vs mayorista desde MVP.
- Curso educativo del v3.0 descartado.
- Stack base: Next.js + Supabase + microservicio cotizador.
- Estructura legal multi-jurisdicción (LLC + SRL) confirmada.

**Próximos pasos inmediatos:**
1. Definir funnel concreto de adquisición de mayoristas ([13](docs/13-adquisicion-mayoristas.md)).
2. Iniciar setup legal (LLC Delaware/Uruguay + SRL local).
3. Onboarding del Asesor Aduanero medio tiempo.
4. Build Fase 1 — Cotizador Híbrido en modo asistido (cliente ve cotización, founder confirma manualmente los primeros 60 días).

**Veredicto técnico:** viable bajo cuatro condiciones operativas (disciplina de scope, plan B2B explícito, audiencia de autos como canal inicial, modo asistido los primeros 60 días). Detalle en [10 — Riesgos](docs/10-riesgos-y-mitigaciones.md).

---

## Convenciones de los documentos

- Cada archivo arranca con header de metadata (versión, fecha, audiencia primaria).
- Cada archivo es razonablemente standalone: incluye contexto mínimo + links cruzados.
- Decisiones que afectan múltiples documentos se centralizan en uno y se referencian (no se duplican).
- Cualquier cambio de decisión clave debe reflejarse en este README + en los documentos afectados.

---

## Backup

El documento monolítico v4.0 (consolidado, sin separar) está preservado en [`archive/analisis_marketplace_v4_monolitico.md`](archive/analisis_marketplace_v4_monolitico.md). Útil si necesitás compartir todo de una sola vez con un destinatario que prefiere PDF largo.

---

## Cómo navegar este repositorio

### Opción 1 — GitHub directo
Cliqueá los links del índice y se renderiza el Markdown automáticamente.

### Opción 2 — GitHub Pages (recomendado para compartir con stakeholders)
El sitio está configurado con **Jekyll + tema `cayman`**. Para activarlo:

1. Ir a `Settings` del repositorio en GitHub.
2. Sección **Pages** (menú lateral).
3. En **Source**, elegir `Deploy from a branch`.
4. Rama: `main` / carpeta: `/ (root)`.
5. Guardar.

GitHub Pages procesa con Jekyll y publica el sitio en pocos minutos en una URL del tipo:
`https://lucasgomezlg.github.io/Dropshipping-marketplace/`

Los links relativos `.md` se convierten automáticamente a `.html` durante el build, por lo que la navegación funciona idéntica a la de GitHub.

### Opción 3 — Lectura local
Cloná el repo y abrí los archivos `.md` con cualquier editor que renderice Markdown (VS Code, Obsidian, Typora).

---

## Contribuir / Iterar

Para sugerir cambios:
1. Crear un branch nuevo (`git checkout -b mejora/<tema>`).
2. Editar el archivo correspondiente en `docs/`.
3. Si el cambio afecta múltiples documentos (ej. cambia una decisión clave), actualizar también este README + los documentos afectados.
4. Commit + push + Pull Request.

**Convención de versiones:**
- Cambios menores (typos, claridad): incrementar versión decimal (ej. 4.1 → 4.2).
- Cambios estructurales (decisión de negocio, nuevo componente): incrementar versión mayor (ej. 4.x → 5.0).
