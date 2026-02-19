---
name: release-generator
description: >
  Cierra una versión del proyecto generando toda la documentación de release.
  Ejecuta un proceso secuencial de 5 pasos: genera el documento detallado de release
  en doc/releases/X.Y.Z.md (usando doc/releases/template.md), mueve los bloques de
  [Unreleased] íntegramente a doc/agents-sessions/changelog-sessions.md, crea la
  sección versionada agregada en CHANGELOG.md, limpia [Unreleased] y actualiza
  doc/releases/README.md.
  Trigger: cerrar versión, generar release, crear release notes, publicar versión,
  release X.Y.Z, cerrar sprint, documentar release.
metadata:
  author: Adrián Cester
  version: "1.0.0"
  scope: [root]
allowed-tools: Read, Edit, Write, Bash
---

# Release Generator

Genera la documentación completa al cerrar una versión. Orquesta 5 operaciones
sobre 4 archivos usando los archivos de sesión y el `[Unreleased]` del CHANGELOG.md
como fuente única de verdad.

## Archivos implicados

| Archivo | Rol |
|---------|-----|
| `CHANGELOG.md` | Fuente de sesiones → recibe sección versionada → `[Unreleased]` queda vacío |
| `doc/agents-sessions/YYYYMMDD-XXX-*.md` | Fuente de detalle para el release note |
| `doc/agents-sessions/changelog-sessions.md` | Destino de los bloques de `[Unreleased]` (íntegros) |
| `doc/releases/template.md` | Base para generar el release note |
| `doc/releases/X.Y.Z.md` | Release note generado (nuevo archivo) |

---

## Workflow (secuencial, respetar orden)

### Paso 0: Confirmar con el usuario

Antes de ejecutar, confirmar:
- Número de versión (`X.Y.Z`)
- Tipo de release: Patch | Minor | Major
- Si el usuario dispone de métricas de tests/cobertura (o si deben obtenerse por comandos)

### Paso 1: Recopilar información

Leer y extraer:
1. Bloques de sesión de `[Unreleased]` en `CHANGELOG.md`
2. Archivos de sesión `doc/agents-sessions/` referenciados en esos bloques
3. Commits del periodo con `git log --oneline` entre hashes inicial y final
4. Fecha actual: `date +%Y-%m-%d` y `date "+%d de %B de %Y"`

Ver guía completa de recopilación: [`references/release-process.md`](references/release-process.md)

### Paso 2: Generar `doc/releases/X.Y.Z.md`

1. Leer `doc/releases/template.md`
2. Crear `doc/releases/X.Y.Z.md` con el contenido del template
3. Rellenar todos los placeholders `[...]` con información real
4. Las secciones VARIABLES (Características, Mejoras, Historial, Lecciones) se redactan
   consolidando la información de todos los archivos de sesión
5. Las secciones FIJAS (Testing, Contribuidores, Migración, Checklist) se rellenan
   con datos del proyecto

Ver detalle de secciones: [`references/release-process.md`](references/release-process.md)

### Paso 3: Mover `[Unreleased]` a `changelog-sessions.md`

**CRÍTICO: Trasladar íntegramente, sin modificar ni resumir.**

1. Leer `doc/agents-sessions/changelog-sessions.md` (crearlo si no existe)
2. Copiar todos los bloques de sesión de `[Unreleased]` al inicio de `changelog-sessions.md`
3. El contenido se traslada exactamente tal como está en `CHANGELOG.md`

### Paso 4: Actualizar `CHANGELOG.md`

Dos operaciones en este orden:

1. **Crear sección versionada** `## [X.Y.Z] - YYYY-MM-DD` con información **agregada**
   de todas las sesiones (no por sesión individual)
2. **Vaciar `[Unreleased]`** → reemplazar todo su contenido por `[Vacío pendiente de iniciar nuevas sesiones]`

Ver formato exacto: [`references/changelog-closure-format.md`](references/changelog-closure-format.md)

---

## Principios CRÍTICOS

- **Fuente de verdad**: `[Unreleased]` + archivos de sesión → no inventar información
- **Íntegridad al mover**: los bloques de sesión van a `changelog-sessions.md` SIN modificar
- **Agregación en CHANGELOG.md**: la sección versionada consolida todas las sesiones, no las lista por separado
- **Orden de pasos**: respetar secuencia (no actualizar CHANGELOG.md antes de generar el release note)
- **Confirmación previa**: siempre confirmar versión con el usuario antes de ejecutar

---

## Referencias Completas

- [`references/release-process.md`](references/release-process.md) — Guía detallada por paso: qué recopilar, cómo redactar secciones, comandos git
- [`references/changelog-closure-format.md`](references/changelog-closure-format.md) — Formato exacto del CHANGELOG.md tras el cierre
