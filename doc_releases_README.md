# Release Notes - ButtonEventSet

Esta carpeta contiene las notas de release (release notes) oficiales del proyecto ButtonEventSet, organizadas por versión.

## 📂 Estructura

```
doc/releases/
├── README.md           # Este archivo (guía del sistema de releases)
├── template.md         # Template para generar nuevos release notes
└── [futuras versiones]
```

## 📋 Archivos Disponibles

| Versión | Fecha | Tipo | Tamaño | Descripción |
|---------|-------|------|--------|-------------|
| [template.md](template.md) | - | Template | 572 líneas | Guía para generar futuros releases |

## 🎯 Propósito

Los release notes sirven para:

1. **Documentar cambios importantes** de cada versión de forma detallada
2. **Comunicar a stakeholders** qué incluye cada release
3. **Guiar a desarrolladores** en migraciones entre versiones
4. **Mantener historial completo** del proyecto
5. **Servir de referencia** para futuras decisiones arquitectónicas

## 📝 Cómo Generar un Nuevo Release Note

### Paso 1: Preparar el CHANGELOG.md

Asegúrate de que la sección correspondiente en `CHANGELOG.md` esté completa con:
- ✅ Todas las entradas de `Added`, `Changed`, `Removed`, `Fixed`
- ✅ Referencias a commits específicos
- ✅ Descripciones claras de cada cambio

### Paso 2: Usar el Template

1. Copiar `template.md` a `[version].md`:
   ```bash
   cp doc/releases/template.md doc/releases/X.Y.Z.md
   ```

2. Reemplazar todos los placeholders:
   - `[VERSION]` → Número de versión (ej: `1.0.2`)
   - `[DD de MMMM de YYYY]` → Fecha completa (ej: `15 de febrero de 2026`)
   - `[X.Y.Z]` → Versión completa en todos los lugares
   - `[DESCRIPCIÓN]` → Contenido específico del release
   - `[N]` → Números reales (tests, commits, líneas, etc.)

3. Completar las secciones VARIABLES:
   - **Características Principales**: Según lo implementado
   - **Mejoras y Correcciones**: Del CHANGELOG.md
   - **Historial de Desarrollo**: Proceso seguido
   - **Lecciones Aprendidas**: Aprendizajes técnicos

4. Ajustar secciones FIJAS con datos reales:
   - Actualizar tablas de cobertura
   - Completar métricas de tests
   - Añadir commits específicos
   - Actualizar roadmap futuro

### Paso 3: Actualizar CHANGELOG.md

Reemplazar el encabezado de la sección con:

```markdown
## [X.Y.Z] - YYYY-MM-DD

**Fecha de release:** [DD de MMMM de YYYY]  
**Tipo:** [Tipo de release]  
**Desarrollo:** [Fechas del desarrollo]  
**Commits:** [N] commits desde `[hash-inicial]` hasta `[hash-final]`  
**Cobertura de tests:** [XX.XX]% ([±X.X]% desde [versión anterior])  
**Tests:** [N]/[T] tests pasando ([X]%, antes [N]/[T] - [X]%)  
[Campos adicionales según el tipo de release]
```

### Paso 4: Verificar Calidad

- [ ] Todos los placeholders `[...]` han sido reemplazados
- [ ] Todos los enlaces funcionan correctamente
- [ ] Las métricas son consistentes con los tests reales
- [ ] Los ejemplos de código son correctos
- [ ] Las referencias a commits existen
- [ ] El documento tiene estructura clara y navegable

### Paso 5: Revisar con el Checklist

Al final del release note hay un checklist de release. Verifica que:
- [x] Todos los ítems marcados como completados realmente lo estén
- [ ] Los ítems pendientes están claramente marcados

## 🔍 Estructura del Template

### Secciones FIJAS (mantienen estructura)

Estas secciones están en todos los releases con la misma estructura:

1. **Encabezado** - Metadata básica (versión, fecha, tipo)
2. **Resumen Ejecutivo** - Overview del release
3. **Calidad y Testing** - Tablas de cobertura y tests
4. **Contribuidores** - Quién trabajó en el release
5. **Documentación Actualizada** - Qué docs se crearon/modificaron
6. **Migración** - Guía para actualizar desde versión anterior
7. **Referencias** - Enlaces a docs y commits
8. **Resultados y Conclusiones** - Resumen de logros
9. **Roadmap Futuro** - Próximos pasos
10. **Licencia** - Info legal (fija)
11. **Soporte** - Contactos (fijo)
12. **Agradecimientos** - Créditos
13. **Estadísticas** - Métricas del release
14. **Checklist** - Verificación de completitud

### Secciones VARIABLES (cambian por release)

Estas secciones varían completamente según el release:

1. **Características Principales** - Qué se implementó
2. **Mejoras y Correcciones** - Cambios específicos
3. **Historial de Desarrollo** - Proceso seguido
4. **Lecciones Aprendidas** - Aprendizajes técnicos
5. **Problemas Resueltos** - Bugs corregidos

## 📚 Tipos de Release

### Release Inicial (v1.0.0)

**Características:**
- Presentación completa del componente
- Todas las funcionalidades core documentadas
- Historial desde el inicio del proyecto
- Duración: Meses de desarrollo

**Ejemplo:** [1.0.0.md](1.0.0.md)

### Release de Mantenimiento (vX.Y.Z - PATCH)

**Características:**
- Correcciones de bugs
- Mejoras de calidad (tests, cobertura)
- Sin breaking changes
- Duración: Días/semanas

**Ejemplo:** [1.0.1.md](1.0.1.md)

### Release de Funcionalidades (vX.Y.0 - MINOR)

**Características:**
- Nuevas funcionalidades
- API pública extendida
- Backward compatible
- Duración: Semanas/meses

**Pendiente de ejemplo**

### Release con Breaking Changes (vX.0.0 - MAJOR)

**Características:**
- Cambios incompatibles
- Requiere migración
- Sección especial: ⚠️ Breaking Changes
- Duración: Meses

**Pendiente de ejemplo**

## 🔗 Relación con Otros Documentos

```
CHANGELOG.md
    ↓ (información de cambios)
doc/releases/[version].md
    ↓ (versión expandida y detallada)
[Tag de Git]
    ↓ (release oficial)
[Publicación]
```

### Flujo de Documentación

1. **Durante desarrollo**: Actualizar `CHANGELOG.md` con cada commit
2. **Al finalizar versión**: Generar release note desde template
3. **Antes de publicar**: Revisar y completar release note
4. **Al publicar**: Crear tag de Git y release en GitLab/GitHub
5. **Post-publicación**: Actualizar README.md si hay cambios en API

## 📐 Convenciones

### Formato de Fechas

- **En nombres de archivo**: `X.Y.Z.md` (ej: `1.0.1.md`)
- **En encabezados**: `DD de MMMM de YYYY` (ej: `9 de enero de 2026`)
- **En tablas**: `YYYY-MM-DD` (ej: `2026-01-09`)

### Referencias a Commits

- Usar hash corto de 7 caracteres: `` `a1b2c3d` ``
- Incluir mensaje del commit después del hash
- Formato: `` `[hash]` - [tipo]([scope]): [mensaje] ``

### Referencias a Issues

- Formato: `#[número]`
- Ejemplo: `#42`, `#123`

### Métricas

- Tests: `[N]/[Total] ([X]%)` (ej: `120/120 (100%)`)
- Cobertura: `[XX.XX]%` con 2 decimales (ej: `89.87%`)
- Mejora: `+[X.X]%` o `-[X.X]%` (ej: `+3.8%`)

### Checkmarks

- Completado: `✅`
- Pendiente: `[ ]`
- En progreso: `[-]` (solo en docs de desarrollo)
- Advertencia: `⚠️`

## 🛠️ Herramientas de Ayuda

### Contar Tests

```bash
npm test -- --verbose | grep -E "✓|✗" | wc -l
```

### Obtener Cobertura

```bash
npm test -- --coverage --coverageReporters=text
```

### Listar Commits de un Release

```bash
git log --oneline [hash-inicial]..[hash-final]
```

### Contar Commits por Tipo

```bash
git log [hash-inicial]..[hash-final] --pretty=format:"%s" | \
  grep -oE "^[a-z]+(\([a-z]+\))?:" | sort | uniq -c
```

### Ver Archivos Modificados

```bash
git diff --stat [hash-inicial]..[hash-final]
```

## 📖 Recursos Adicionales

- **Keep a Changelog**: https://keepachangelog.com/es/1.0.0/
- **Semantic Versioning**: https://semver.org/lang/es/
- **Conventional Commits**: https://www.conventionalcommits.org/es/v1.0.0/
- **AGENTS.md**: Guía completa para desarrollo con TDD

## ✅ Checklist para Nuevo Release Note

- [ ] Copiar `template.md` a `[version].md`
- [ ] Reemplazar todos los placeholders `[...]`
- [ ] Completar secciones VARIABLES con información del CHANGELOG
- [ ] Actualizar tablas de cobertura con datos reales
- [ ] Añadir todos los commits con sus hashes
- [ ] Verificar que todos los enlaces funcionen
- [ ] Revisar que las métricas sean consistentes
- [ ] Probar ejemplos de código si aplica
- [ ] Actualizar roadmap futuro
- [ ] Revisar ortografía y formato
- [ ] Actualizar encabezado del CHANGELOG.md correspondiente
- [ ] Añadir entrada en esta tabla (README.md de releases)

---

**Última actualización:** 27 de enero de 2026  
**Mantenido por:** Equipo de Desarrollo Web - IPGSoft
