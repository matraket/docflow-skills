# Guía Detallada del Proceso de Release

## Tabla de Contenidos

1. [Paso 1: Recopilar información](#paso-1)
2. [Paso 2: Generar el release note](#paso-2)
3. [Paso 3: Mover Unreleased a changelog-sessions.md](#paso-3)
4. [Paso 4: Actualizar CHANGELOG.md](#paso-4)

---

## Paso 1: Recopilar información

### Desde CHANGELOG.md [Unreleased]

- Identificar todos los bloques de sesión `### YYYYMMDD-XXX-{user}-{agent}`
- Extraer la lista de archivos de sesión referenciados en cada bloque
- Recopilar los cambios listados en Added / Changed / Fixed / Removed de cada bloque

### Desde los archivos de sesión

Leer cada `doc/agents-sessions/YYYYMMDD-XXX-{user}-{agent}.md` para extraer:
- Decisiones técnicas relevantes
- Problemas encontrados y soluciones aplicadas
- Lecciones aprendidas
- Archivos modificados y commits asociados

### Desde git

```bash
# Commits del periodo (entre el último release y HEAD)
git log --oneline [hash-ultimo-release]..HEAD

# Hash del primer commit del periodo
git log --oneline [hash-ultimo-release]..HEAD | tail -1

# Hash del último commit
git log --oneline -1

# Commits agrupados por tipo
git log [hash-inicial]..HEAD --pretty=format:"%s" | grep -oE "^[a-z]+(\([a-z]+\))?:" | sort | uniq -c

# Archivos modificados en el periodo
git diff --stat [hash-inicial]..HEAD
```

### Métricas de tests y cobertura

Si el proyecto tiene scripts de test, ejecutarlos para obtener datos actuales.
Si no están disponibles, solicitar al usuario los valores antes de continuar.

### Fecha del release

```bash
date +%Y-%m-%d          # Para encabezado CHANGELOG: 2026-02-19
date "+%d de %B de %Y"  # Para documentos: 19 de febrero de 2026
```

### Periodo de desarrollo

- **Inicio:** fecha del primer archivo de sesión referenciado en `[Unreleased]`
- **Fin:** fecha actual (del release)

---

## Paso 2: Generar el release note

### Proceso

1. Leer `doc/releases/template.md` íntegramente
2. Crear `doc/releases/X.Y.Z.md` con ese contenido
3. Reemplazar los placeholders globales primero:
   - `[VERSION]` → número de versión (ej: `1.2.0`)
   - `[X.Y.Z]` → ídem
   - `[DD de MMMM de YYYY]` → fecha larga del release
   - `[YYYY-MM-DD]` → fecha corta del release

### Secciones FIJAS (rellenar con datos objetivos)

Estas secciones tienen estructura fija y se rellenan con datos reales:

- **Encabezado**: versión, fecha, tipo de release
- **Calidad y Testing**: tablas de cobertura, número de tests, suites
- **Contribuidores**: extraer de `git shortlog -sn`
- **Documentación Actualizada**: listar archivos creados/modificados en el periodo
- **Migración**: backward compatible (Patch/Minor) o breaking changes (Major)
- **Referencias**: commits del periodo con hashes reales
- **Estadísticas**: commits por tipo, líneas modificadas
- **Checklist**: marcar ítems completados

### Secciones VARIABLES (redactar consolidando sesiones)

Estas secciones requieren síntesis y criterio editorial:

- **Resumen Ejecutivo**: 2-3 párrafos describiendo qué aporta este release al proyecto. Extraer de los bloques de sesión y archivos de sesión.
- **Características Principales**: las features o mejoras más importantes del release, con ejemplos de uso si aplica. Consolidar desde los `Added` de las sesiones.
- **Mejoras y Correcciones**: detallar correcciones y refactorizaciones. Extraer de `Changed` y `Fixed`.
- **Historial de Desarrollo**: describir el proceso seguido, fases si las hubo, metodología. Extraer de los archivos de sesión.
- **Lecciones Aprendidas**: aprendizajes técnicos del periodo. Extraer de secciones "Notas y Aprendizajes" de los archivos de sesión.
- **Problemas Resueltos**: bugs o limitaciones concretas que se resolvieron. Extraer de `Fixed` y archivos de sesión.
- **Roadmap Futuro**: proponer próximas versiones basándose en los "Próximos Pasos" de los archivos de sesión.

### Criterio de nivel de detalle

El release note es el documento **más exhaustivo** del ciclo. A diferencia del CHANGELOG.md (que es resumen), aquí se incluye:
- Código de ejemplo cuando ilustra una funcionalidad importante
- Análisis commit a commit si ayuda a entender el progreso
- Decisiones arquitectónicas con alternativas consideradas
- Métricas con evolución (estado inicial → estado final)

---

## Paso 3: Mover Unreleased a changelog-sessions.md

### Proceso

1. Leer `doc/agents-sessions/changelog-sessions.md`
   - Si no existe → crear con cabecera:
     ```markdown
     # Changelog de Sesiones

     Histórico detallado de sesiones de trabajo. Cada bloque corresponde a una
     sesión de agente IA. Este archivo es el registro completo trasladado desde
     las secciones [Unreleased] del CHANGELOG.md al cerrar cada versión.

     ---
     ```
2. Extraer todos los bloques de sesión de `[Unreleased]` (desde el primer `###` hasta el último `---`)
3. Insertar esos bloques al inicio del archivo (después de la cabecera), añadiendo una referencia a la versión que se cierra:
   ```markdown
   ## Sesiones del release [X.Y.Z] - YYYY-MM-DD

   [bloques de sesión íntegros]
   ```

**CRÍTICO:** El contenido se copia tal cual, sin modificar redacción ni estructura.

---

## Paso 4: Actualizar CHANGELOG.md

Ver formato exacto: [`changelog-closure-format.md`](changelog-closure-format.md)

### Operación 1: Crear sección versionada

Insertar inmediatamente después de `---` (separador que cierra `[Unreleased]`) y antes del release anterior.

La información de Added/Changed/Fixed/Removed se **agrega** de todas las sesiones:
- Eliminar duplicados
- Agrupar entradas relacionadas
- Mantener concisión (igual que en los bloques de sesión individuales)

### Operación 2: Vaciar [Unreleased]

Reemplazar el contenido de `[Unreleased]` por:
```markdown
## [Unreleased]

[Vacío pendiente de iniciar nuevas sesiones]

---
```

