# Changelog Updater Skill

Skill para mantener la sección `[Unreleased]` del `CHANGELOG.md` actualizada automáticamente, agrupando cambios por sesión de trabajo.

## 🎯 Propósito

Permite que agentes IA mantengan el CHANGELOG.md actualizado de forma automática durante las sesiones de desarrollo, complementando a session-manager:

- **Resume trabajo realizado** en entradas concisas de alto nivel
- **Agrupa por sesión** dentro de la sección `[Unreleased]`
- **Coordina con session-manager** usando el mismo nombre de sesión
- **Mantiene trazabilidad** enlazando cada bloque al archivo de sesión detallado
- **Sigue Keep a Changelog** con categorías estándar (Added, Changed, Fixed, Removed)

## 📦 Instalación

1. **Copiar el skill a tu proyecto:**
   ```bash
   cp -r changelog-updater /path/to/your/skills/
   ```

2. **Habilitar en Claude:**
   - El skill se activará automáticamente cuando esté en la ruta de skills
   - Requiere session-manager activo para coordinación de nombres de sesión

3. **Asegurar que existe CHANGELOG.md:**
   - El skill trabaja sobre un `CHANGELOG.md` existente en la raíz del proyecto
   - Si no existe, lo crea con la cabecera estándar basada en [Keep a Changelog](https://keepachangelog.com/es/1.0.0/)

## 🚀 Uso

### Inicio automático

El skill detecta automáticamente cuando se inicia una nueva sesión (coordinado con session-manager) y:

1. Lee `CHANGELOG.md` y localiza la sección `[Unreleased]`
2. Crea un nuevo bloque de sesión `### YYYYMMDD-XXX-{user}-{agent}` al inicio de `[Unreleased]`
3. Calcula timestamps con comandos (`date +%Y%m%d`, `date +%H:%M`)
4. Informa: "Bloque de sesión creado en CHANGELOG.md"

**No necesitas hacer nada**, el bloque se crea automáticamente.

### Durante el trabajo

El skill actualiza el bloque de sesión cuando session-manager documenta trabajo significativo:

- Verifica atomicidad (lee CHANGELOG.md antes de escribir)
- Añade la entrada en la categoría correcta (Added / Changed / Fixed / Removed)
- Actualiza "Hora de últimos trabajos" con timestamp actual
- Actualiza incrementalmente (no reescribe la sección completa)

### Cierre de sesión

```
Usuario: Cierra la sesión
Claude: [Verifica categorías vacías, actualiza timestamp final, confirma cierre]
```

## 📁 Estructura generada

```
tu-proyecto/
├── CHANGELOG.md
│   └── ## [Unreleased]
│       ├── ### 20260218-002-adrian-CLAUDE   ← sesión más reciente
│       └── ### 20260218-001-adrian-CLAUDE   ← sesión anterior
└── doc/
    └── agents-sessions/
        ├── 20260218-001-adrian-CLAUDE.md     ← detalle (session-manager)
        └── 20260218-002-adrian-CLAUDE.md     ← detalle (session-manager)
```

**Formato del bloque:** `### YYYYMMDD-XXX-{user}-{agent}`
- `{user}`: Configurado en variable `USER_SESSION_MANAGER` del archivo `.env`
- `{agent}`: Alias del agente IA (mismo que session-manager)

## 🎨 Ejemplo de bloque de sesión

### Al crear el bloque (inicio de sesión)

```markdown
### 20260218-001-adrian-CLAUDE

* **Fecha de sesión:** 18 de febrero de 2026
* **Hora de inicio:** 10:15
* **Hora de últimos trabajos:** 10:15
* **Documento de sesión:** [doc/agents-sessions/20260218-001-adrian-CLAUDE.md](doc/agents-sessions/20260218-001-adrian-CLAUDE.md)

#### Added

[Sin cambios]

#### Changed

[Sin cambios]

#### Fixed

[Sin cambios]

#### Removed

[Sin cambios]

---
```

### Tras trabajo significativo (durante la sesión)

```markdown
### 20260218-001-adrian-CLAUDE

* **Fecha de sesión:** 18 de febrero de 2026
* **Hora de inicio:** 10:15
* **Hora de últimos trabajos:** 12:40
* **Documento de sesión:** [doc/agents-sessions/20260218-001-adrian-CLAUDE.md](doc/agents-sessions/20260218-001-adrian-CLAUDE.md)

#### Added

- Añadido componente de autenticación JWT con soporte para refresh tokens
- Nuevo endpoint `POST /api/users/login` con validación de credenciales

#### Changed

[Sin cambios]

#### Fixed

- Corregido error en validación de formulario cuando el email contiene mayúsculas

#### Removed

[Sin cambios]

---
```

## ⚙️ Configuración

### Categorías de cambios

| Categoría | Cuándo usarla |
|-----------|--------------|
| `Added` | Nueva funcionalidad, nuevos archivos, nuevas features |
| `Changed` | Modificación de funcionalidad existente, refactorizaciones |
| `Fixed` | Corrección de bugs o errores |
| `Removed` | Eliminación de código, archivos o funcionalidades |

### Nivel de detalle

El CHANGELOG.md contiene **resúmenes de alto nivel**. El detalle completo vive en el archivo de sesión.

| En CHANGELOG.md | En archivo de sesión |
|-----------------|---------------------|
| "Añadido componente de autenticación JWT" | Código completo, alternativas consideradas, decisiones técnicas |
| "Corregido error en validación de formulario" | Stack trace, causa raíz, solución aplicada |
| "Refactorizado módulo de pagos" | Archivos modificados, pasos de refactorización |

**Regla práctica:** Una entrada = 1-2 líneas máximo.

### Idioma

**Por defecto:** Español de España

Toda la documentación se genera en español peninsular:
- Fechas: "18 de febrero de 2026"
- Hora: formato 24h "14:30"

No configurable - es un estándar del sistema de documentación.

## 🎯 Casos de uso

### 1. Documentación automática de progreso

Cada sesión de trabajo genera un bloque en `[Unreleased]` con entradas concisas. Al revisar el CHANGELOG.md se obtiene una visión rápida del progreso.

### 2. Preparación de releases

Al cerrar una versión, los bloques de sesión en `[Unreleased]` se consolidan en una sección de versión (`## [1.0.0] - YYYY-MM-DD`).

### 3. Complemento de session-manager

Session-manager documenta el detalle en archivos de sesión. Changelog-updater resume ese trabajo en el CHANGELOG.md. Cada bloque enlaza al archivo de sesión correspondiente para navegación rápida.

### 4. Auditoría rápida de cambios

Sin abrir archivos de sesión individuales, el CHANGELOG.md ofrece una vista consolidada de todo el trabajo realizado, organizado cronológicamente.

## 📚 Documentación relacionada

- **SKILL.md:** Documentación compacta del skill (137 líneas)
- **references/changelog-structure.md:** Estructura canónica del CHANGELOG.md
- **references/update-rules.md:** Reglas detalladas de actualización con ejemplos
- **assets/templates/session-block.md:** Template del bloque de sesión

## 🤝 Contribuir

Este skill es parte de un sistema de documentación automática para desarrollo asistido por IA.

## 📄 Licencia

MIT License - Ver LICENSE para detalles

---

**Creado por:** Adrián (IPGSoft)
**Versión:** 1.0.0
**Última actualización:** 18 de febrero de 2026

---

## 🎯 Skill Changelog Updater

Mantenimiento automático de la sección `[Unreleased]` del CHANGELOG.md, agrupando cambios por sesión de trabajo.

**Core:** SKILL.md de **137 líneas** - Complementario a session-manager.

**Progressive Disclosure:** Reglas, estructura y templates en `references/` y `assets/` para carga bajo demanda.

---

## 📁 Estructura

```
changelog-updater/
├── SKILL.md                    (137 líneas - core esencial)
├── README.md
├── assets/                     (recursos estáticos)
│   └── templates/
│       └── session-block.md
└── references/                 (documentación de referencia)
    ├── changelog-structure.md
    └── update-rules.md
```

**Cumple con:** [agentskills.io](https://agentskills.io) especificaciones oficiales

---

## 📖 Cómo Usar

### Para Claude (agentes IA)

1. **Lee SKILL.md** (~137 líneas, principios core)
2. **Si necesita estructura del CHANGELOG:** Lee `references/changelog-structure.md`
3. **Si necesita reglas de actualización:** Lee `references/update-rules.md`
4. **Si necesita template de bloque:** Lee `assets/templates/session-block.md`

### Para Desarrolladores

**Leer documentación:**
- Empezar por `SKILL.md` para entender el sistema
- Consultar `references/changelog-structure.md` para la estructura canónica
- Consultar `references/update-rules.md` para reglas y ejemplos
- Consultar `assets/templates/session-block.md` para el template de bloque

**Extender skill:**
- Añadir regla de actualización: Editar `references/update-rules.md`
- Modificar estructura: Editar `references/changelog-structure.md`
- SKILL.md permanece estable (~137 líneas)

---

## 🎯 Principios Fundamentales

1. **Coordinación:** Sincronizado con session-manager (mismo nombre de sesión)
2. **Atomicidad:** Verifica antes de documentar (no duplica)
3. **Alto nivel:** Solo resúmenes, el detalle vive en el archivo de sesión
4. **Sin próximos pasos:** Solo se registra lo efectivamente realizado

**Detalles:** Ver `SKILL.md` sección "Principios Core (CRÍTICO)"

---

## 📗 Documentación

- **SKILL.md** - Instrucciones core para Claude (137 líneas)
- **references/changelog-structure.md** - Estructura canónica del CHANGELOG.md
- **references/update-rules.md** - Reglas detalladas con ejemplos
- **assets/templates/session-block.md** - Template del bloque de sesión

---

## 🙏 Agradecimientos

- **session-manager** por la base arquitectónica de documentación por sesiones
- **Keep a Changelog** por el formato estándar de changelog

---

## 📞 Soporte

Para reportar issues o sugerir mejoras, contactar al autor o abrir issue en el repositorio del proyecto.

---

> **Creado por:** Adrian (IPGSoft)
> **Última actualización:** 2026-03-21
