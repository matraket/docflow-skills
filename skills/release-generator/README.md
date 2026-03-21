# Release Generator Skill

Skill para cerrar versiones de un proyecto generando toda la documentación de release de forma automatizada y secuencial.

## 🎯 Propósito

Permite que agentes IA orquesten el cierre completo de una versión del proyecto, generando documentación profesional de release que:

- **Consolida el trabajo** de todas las sesiones del periodo en un release note detallado
- **Automatiza el CHANGELOG.md** generando la sección versionada y limpiando `[Unreleased]`
- **Preserva el historial** moviendo los bloques de sesión a `changelog-sessions.md`
- **Garantiza consistencia** mediante scripts que validan y transforman los datos
- **Requiere confirmación humana** antes de ejecutar cambios irreversibles

## 📦 Instalación

1. **Copiar el skill a tu proyecto:**
   ```bash
   cp -r release-generator /path/to/your/skills/
   ```

2. **Copiar scripts al proyecto (si es necesario):**
   ```bash
   cp release-generator/scripts/*.py /path/to/your/scripts/
   cp release-generator/scripts/*.sh /path/to/your/scripts/
   ```

3. **Habilitar en Claude:**
   - El skill se activará automáticamente cuando esté en la ruta de skills
   - No requiere configuración adicional

4. **Prerrequisitos en el proyecto:**
   - `session-manager` y `changelog-updater` deben haber estado activos durante el desarrollo
   - `CHANGELOG.md` debe existir en la raíz con sección `## [Unreleased]`
   - `doc/releases/template.md` debe existir como base para el release note

## 🚀 Uso

### Activación

El skill se activa cuando el usuario solicita cerrar una versión con frases como:

- "Cerrar versión 1.2.0"
- "Generar release notes"
- "Crear release"
- "Publicar versión"
- "Release X.Y.Z"
- "Cerrar sprint"
- "Documentar release"

### Workflow secuencial

El proceso consta de 6 pasos que deben ejecutarse en orden estricto:

**Paso 0 — Verificar prerrequisitos:**
- Confirmar que `CHANGELOG.md` existe y tiene contenido en `[Unreleased]`
- Verificar que los archivos de sesión referenciados existen
- Confirmar con el usuario: versión, tipo (Patch/Minor/Major) y disponibilidad de métricas

**Paso 1 — Recopilar información:**
- Bloques de sesión de `[Unreleased]` en `CHANGELOG.md`
- Archivos de sesión en `doc/agents-sessions/`
- Commits del periodo con `git log`
- Fecha actual del release

**Paso 2 — Generar `doc/releases/X.Y.Z.md`:**
- Usa `doc/releases/template.md` como base
- Rellena placeholders con información real
- Redacta secciones variables consolidando todas las sesiones

**Paso 3 — Generar borrador en CHANGELOG.md:**
```bash
# Con Python (preferido)
python3 scripts/create_release_draft.py X.Y.Z --type Minor

# Con shell (fallback)
bash scripts/create_release_draft.sh X.Y.Z --type Minor
```

**Paso 4 — Revisar y refinar el borrador:**
- Deduplicar entradas repetidas
- Completar campos `[COMPLETAR]` (tests, cobertura)
- Eliminar aviso `BORRADOR`

**Paso 5 — Mover `[Unreleased]` a `changelog-sessions.md`:**
```bash
# Con Python (preferido)
python3 scripts/move_unreleased.py X.Y.Z

# Con shell (fallback)
bash scripts/move_unreleased.sh X.Y.Z
```

### Solicitar release

```
Usuario: Cerrar versión 1.2.0
Claude: [Verifica prerrequisitos, confirma datos con el usuario, ejecuta los 6 pasos]
```

## 📁 Estructura generada

```
tu-proyecto/
├── CHANGELOG.md                              # Sección [X.Y.Z] agregada, [Unreleased] limpio
├── doc/
│   ├── agents-sessions/
│   │   ├── 20260210-001-adrian-CLAUDE.md      # Archivos de sesión (fuente de detalle)
│   │   ├── 20260215-001-adrian-CLAUDE.md
│   │   └── changelog-sessions.md              # Bloques de [Unreleased] archivados por versión
│   └── releases/
│       ├── template.md                        # Base para generar release notes
│       └── 1.2.0.md                           # Release note generado (nuevo archivo)
```

## 🎨 Ejemplo de sección versionada generada

```markdown
## [1.2.0] - 2026-02-19

* **Fecha de release:** 19 de febrero de 2026
* **Tipo:** Minor
* **Periodo de desarrollo:** 10/02/2026 – 19/02/2026
* **Commits:** 12 commits desde `a1b2c3d` hasta `e4f5g6h`
* **Cobertura de tests:** 87.45% (+3.2% desde 84.25%)
* **Tests:** 120/120 (100%, antes 108/120 - 90.0%)
* **Sesiones de trabajo:**
    - [doc/agents-sessions/20260210-001-adrian-CLAUDE.md](doc/agents-sessions/20260210-001-adrian-CLAUDE.md)
    - [doc/agents-sessions/20260215-001-adrian-CLAUDE.md](doc/agents-sessions/20260215-001-adrian-CLAUDE.md)

### Added

- Añadido sistema de autenticación JWT con refresh tokens
- Nuevo endpoint `POST /api/users/login` con validación de credenciales

### Changed

- Refactorizado módulo de configuración para soportar variables de entorno

### Fixed

- Corregido error de timeout en conexiones WebSocket

### Removed

[Sin cambios]

---
```

## ⚙️ Configuración

### Scripts disponibles

El skill incluye dos pares de scripts (Python y shell como fallback):

| Script | Función |
|--------|---------|
| `scripts/create_release_draft.py` | Genera borrador de sección versionada en CHANGELOG.md |
| `scripts/create_release_draft.sh` | Ídem, fallback shell sin Python |
| `scripts/move_unreleased.py` | Mueve bloques de `[Unreleased]` a `changelog-sessions.md` y limpia |
| `scripts/move_unreleased.sh` | Ídem, fallback shell sin Python |

### Parámetros de los scripts

**create_release_draft:**
```bash
python3 scripts/create_release_draft.py <version> [--type Patch|Minor|Major]
```
- `version` (obligatorio): número de versión semántica (ej: `1.2.0`)
- `--type` (opcional, default: `Patch`): tipo de release

**move_unreleased:**
```bash
python3 scripts/move_unreleased.py <version>
```
- `version` (obligatorio): número de versión semántica

### Idioma

**Por defecto:** Español de España

Toda la documentación se genera en español peninsular:
- Fechas: "19 de febrero de 2026"
- Hora: formato 24h
- Categorías: Added, Changed, Fixed, Removed (estándar Keep a Changelog)

No configurable - es un estándar del sistema de documentación.

## 🎯 Casos de uso

### 1. Cierre de sprint con múltiples sesiones

Varias sesiones de trabajo acumuladas en `[Unreleased]`. El skill consolida todo en un release note detallado y una sección versionada limpia en CHANGELOG.md.

### 2. Release Patch tras corrección de bugs

Una o dos sesiones con correcciones puntuales. El skill genera la documentación completa manteniendo la trazabilidad.

### 3. Release Major con breaking changes

Múltiples sesiones con cambios significativos. El release note captura decisiones arquitectónicas, lecciones aprendidas y guía de migración.

### 4. Auditoría de releases

Cada release queda documentado con enlaces a las sesiones de trabajo originales, commits del periodo y métricas de calidad.

## 📚 Documentación relacionada

- **SKILL.md:** Documentación compacta del skill (149 líneas)
- **references/release-process.md:** Guía detallada por paso: qué recopilar, cómo redactar secciones, comandos git
- **references/changelog-closure-format.md:** Formato exacto del CHANGELOG.md tras el cierre

## 🎯 Principios Fundamentales

1. **Fuente de verdad:** `[Unreleased]` + archivos de sesión son la única fuente — no inventar información
2. **Integridad al mover:** Los bloques de sesión van a `changelog-sessions.md` SIN modificar
3. **Agregación:** La sección versionada consolida todas las sesiones, no las lista por separado
4. **Orden estricto:** Respetar la secuencia de pasos (no actualizar CHANGELOG.md antes del release note)
5. **Confirmación previa:** Siempre confirmar versión y tipo con el usuario antes de ejecutar

## 📁 Estructura del skill

```
release-generator/
├── SKILL.md                                (149 líneas - core esencial)
├── README.md
├── references/                             (documentación de referencia)
│   ├── release-process.md                  — Guía detallada del proceso completo
│   └── changelog-closure-format.md         — Formato exacto del CHANGELOG.md tras cierre
└── scripts/                                (automatización)
    ├── create_release_draft.py             — Genera borrador de sección versionada
    ├── create_release_draft.sh             — Ídem, fallback shell
    ├── move_unreleased.py                  — Mueve [Unreleased] y limpia CHANGELOG.md
    └── move_unreleased.sh                  — Ídem, fallback shell
```

## 📖 Cómo Usar

### Para Agentes de IA (Claude Code, Codex, OpenCode, etc)

1. **Lee SKILL.md** (~3,000 tokens, workflow completo)
2. **Si necesita detalle del proceso:** Lee `references/release-process.md`
3. **Si necesita formato del CHANGELOG:** Lee `references/changelog-closure-format.md`
4. **Para generar borrador:** Ejecuta `scripts/create_release_draft.py`
5. **Para mover bloques:** Ejecuta `scripts/move_unreleased.py`

### Para Desarrolladores

**Leer documentación:**
- Empezar por `SKILL.md` para entender el sistema
- Consultar `references/release-process.md` para el proceso paso a paso
- Consultar `references/changelog-closure-format.md` para el formato final esperado

**Extender skill:**
- Añadir nuevo script: Crear en `scripts/`
- Añadir documentación de referencia: Crear en `references/`
- SKILL.md permanece estable (~149 líneas)

## 📗 Documentación

- **SKILL.md** - Instrucciones core para Claude (149 líneas)
- **references/release-process.md** - Guía detallada por paso
- **references/changelog-closure-format.md** - Formato exacto del CHANGELOG.md
- **scripts/create_release_draft.py** - Genera borrador de sección versionada (Paso 3)
- **scripts/create_release_draft.sh** - Ídem, fallback shell sin Python
- **scripts/move_unreleased.py** - Mueve [Unreleased] y limpia CHANGELOG.md (Paso 5)
- **scripts/move_unreleased.sh** - Ídem, fallback shell sin Python

## 🤝 Contribuir

Este skill es parte de un sistema de documentación automática para desarrollo asistido por IA.

## 📄 Licencia

MIT License - Ver LICENSE para detalles

---

**Creado por:** Adrián (IPGSoft)
**Versión:** 1.0.0
**Última actualización:** 21 de marzo de 2026

---

## 🙏 Agradecimientos

- **session-manager** y **changelog-updater** por la base del sistema de documentación por sesiones
- **Keep a Changelog** por el formato estándar de changelog

---

## 📞 Soporte

Para reportar issues o sugerir mejoras, contactar al autor o abrir issue en el repositorio del proyecto.

---

> **Creado por:** Adrian (IPGSoft)
> **Última actualización:** 2026-03-21
