---
name: session-manager
description: Gestiona sesiones de trabajo con agentes IA. Crea y mantiene archivos de sesión en doc/agents-sessions/ con formato YYYYMMDD-XXX-{agent-alias}.md, documentando automáticamente el trabajo realizado durante cada interacción.
---

# Session Manager

Skill para gestionar sesiones de trabajo con agentes IA, manteniendo documentación detallada y trazabilidad completa del trabajo realizado.

## Propósito

Este skill permite que los agentes IA documenten automáticamente su trabajo durante las sesiones de desarrollo, manteniendo un historial completo que sobrevive a las compactaciones de contexto y sirve como fuente de verdad para generar releases.

## Cuándo usar este skill

- Al inicio de cada nueva sesión de trabajo
- Durante el desarrollo cuando se completan tareas significativas
- Al finalizar una sesión de trabajo
- Cuando el usuario solicita explícitamente documentar el progreso

## Estructura de archivos

```
doc/
└── agents-sessions/
    ├── changelog-sessions.md
    ├── 20260209-001-CLAUDE.md
    ├── 20260209-002-CLAUDE.md
    └── ...
```

## Formato del archivo de sesión

Cada archivo sigue esta estructura:

```markdown
# Sesión Agente: YYYYMMDD-XXX-{agent-alias}

* **Agente de IA:** [Nombre del agente]
* **Fecha creación:** [DD de MMMM de YYYY]
* **Hora de inicio:** [HH:MM]
* **Hora de últimos trabajos:** [HH:MM]

---

## 📋 Resumen de la Sesión

[Breve descripción de los objetivos y contexto de la sesión]

---

## 🎯 Objetivos

- [ ] Objetivo 1
- [ ] Objetivo 2
- [ ] Objetivo 3

---

## 💼 Trabajo Realizado

### [HH:MM] - [Título de la tarea]

**Descripción:**
[Descripción detallada de qué se hizo]

**Archivos modificados:**
- `path/to/file1.ext` - [Descripción del cambio]
- `path/to/file2.ext` - [Descripción del cambio]

**Decisiones técnicas:**
- [Decisión 1 y su justificación]
- [Decisión 2 y su justificación]

**Resultados:**
- ✅ [Resultado positivo 1]
- ✅ [Resultado positivo 2]
- ⚠️ [Advertencia o nota si aplica]

---

### [HH:MM] - [Siguiente tarea]

[Repetir estructura para cada tarea completada]

---

## 🔄 Próximos Pasos

- [ ] Tarea pendiente 1
- [ ] Tarea pendiente 2
- [ ] Idea para explorar 1

---

## 📝 Notas y Aprendizajes

### Lecciones Técnicas

- [Aprendizaje 1]
- [Aprendizaje 2]

### Decisiones Arquitectónicas

- [Decisión 1 con contexto y alternativas consideradas]

### Problemas Encontrados

**[Problema 1]:**
- **Descripción:** [Qué pasó]
- **Solución:** [Cómo se resolvió]
- **Prevención:** [Cómo evitarlo en el futuro]

---

## 📊 Métricas de la Sesión

- **Duración total:** [X horas Y minutos]
- **Archivos modificados:** [N]
- **Archivos creados:** [N]
- **Commits realizados:** [N]
- **Tests creados/modificados:** [N]
- **Líneas añadidas:** ~[N]
- **Líneas eliminadas:** ~[N]

---

## 🔗 Referencias

- Commits: `hash1`, `hash2`, ...
- Issues relacionados: #XX, #YY
- Documentación consultada: [enlaces]
- PRs relacionados: #XX

---

**Estado final:** [Completada | En progreso | Pausada]
**Próxima sesión:** [Descripción breve de qué continuar]
```

## Comportamiento del skill

### 1. Detección automática de nueva sesión

Al inicio de una conversación, el skill debe:

1. **Verificar si existe directorio** `doc/agents-sessions/`
   - Si NO existe: Crearlo y crear archivo `changelog-sessions.md`
   - Si existe: Continuar

2. **Determinar número de sesión del día:**
   - Listar archivos con patrón `YYYYMMDD-*-{agent-alias}.md`
   - Incrementar contador (001, 002, 003...)

3. **Crear archivo de sesión:**
   - Nombre: `YYYYMMDD-XXX-{agent-alias}.md`
   - Rellenar metadata inicial (fecha, hora, agente)
   - Inicializar estructura vacía

4. **Informar al usuario discretamente:**
   - "Sesión documentada en `doc/agents-sessions/20260209-001-CLAUDE.md`"

### 2. Actualización durante la sesión

Durante el trabajo, el skill debe actualizar el archivo cuando:

- Se complete una tarea significativa
- Se tome una decisión técnica importante
- Se modifiquen/creen archivos relevantes
- El usuario solicite documentar el progreso

**Actualización incremental:**
- NO reescribir todo el archivo
- Añadir nueva sección con timestamp
- Actualizar "Hora de últimos trabajos"
- Marcar objetivos completados

### 3. Cierre de sesión

Al finalizar o cuando el usuario lo solicite:

1. **Actualizar métricas finales**
2. **Completar sección "Próximos Pasos"**
3. **Añadir estado final**
4. **Confirmar al usuario:**
   - "Sesión documentada y cerrada."

## Niveles de detalle

El skill adapta el nivel de detalle según el contexto:

### Modo CONCISO (por defecto)
- Resumen de 2-3 líneas por tarea
- Solo decisiones técnicas importantes
- Métricas básicas

### Modo DETALLADO (cuando se solicita)
- Descripción completa de cada paso
- Todas las decisiones con alternativas consideradas
- Código relevante incluido
- Métricas exhaustivas

### Modo MINIMAL (para tareas triviales)
- Solo timestamp + título + resultado
- Sin métricas

## Integración con otras herramientas

### Con CHANGELOG.md

El archivo de sesión sirve como **fuente de verdad** para actualizar `CHANGELOG.md`:
- Cada sesión genera un bloque en `Unreleased`
- La información se resume pero vincula al archivo de sesión
- NO duplicar información, solo referenciar

### Con releases

Al cerrar una versión:
- Los archivos de sesión se usan para generar el release note detallado
- El `CHANGELOG.md` se genera desde los archivos de sesión
- Los archivos de sesión permanecen como historial

## Reglas importantes

### ✅ HACER

- Documentar decisiones técnicas con su contexto
- Mantener timestamps precisos
- Actualizar incrementalmente (no reescribir)
- Usar lenguaje claro y específico
- Incluir referencias (commits, files, issues)
- Marcar tareas completadas vs pendientes
- Separar trabajo realizado de próximos pasos

### ❌ NO HACER

- No reescribir el archivo completo en cada actualización
- No incluir próximos pasos en secciones de trabajo realizado
- No duplicar información entre archivo de sesión y CHANGELOG
- No usar lenguaje vago ("se mejoró", "se arregló")
- No omitir decisiones técnicas importantes
- No mezclar sesiones (una sesión = un archivo)

## Ejemplos de uso

### Ejemplo 1: Inicio de sesión

**Usuario:** "Vamos a trabajar en el módulo de autenticación"

**Skill:**
1. Crea `doc/agents-sessions/20260209-001-CLAUDE.md`
2. Rellena metadata
3. Añade objetivo: "Implementar módulo de autenticación"
4. Responde: "✓ Sesión iniciada y documentada en `doc/agents-sessions/20260209-001-CLAUDE.md`. ¿Por dónde empezamos?"

### Ejemplo 2: Durante el trabajo

**Contexto:** El agente acaba de implementar login con JWT

**Skill:**
1. Actualiza archivo de sesión:
   ```markdown
   ### 14:23 - Implementación de Login con JWT
   
   **Descripción:**
   Implementado sistema de autenticación usando JSON Web Tokens.
   
   **Archivos modificados:**
   - `src/auth/login.js` - Nueva función `authenticateUser()`
   - `src/middleware/auth.js` - Middleware de validación de tokens
   
   **Decisiones técnicas:**
   - Usar JWT en lugar de sessions por escalabilidad
   - Tokens expiran en 24h por seguridad
   - RefreshTokens almacenados en DB para revocación
   
   **Resultados:**
   - ✅ Login funcional con tokens
   - ✅ Middleware de protección de rutas
   - ✅ Tests unitarios pasando (12/12)
   ```

2. NO interrumpe el flujo de trabajo
3. Solo menciona si el usuario pregunta por el progreso

### Ejemplo 3: Usuario solicita resumen

**Usuario:** "¿Qué hemos hecho hasta ahora?"

**Skill:**
1. Lee el archivo de sesión actual
2. Genera resumen ejecutivo:
   ```
   Resumen de sesión 20260209-001-CLAUDE (2h 15min):
   
   ✅ Completado:
   - Sistema de autenticación JWT
   - Middleware de protección de rutas
   - 12 tests unitarios
   
   📝 En progreso:
   - Integración con frontend
   
   📂 Archivos: 8 modificados, 3 creados
   
   Documentación completa: doc/agents-sessions/20260209-001-CLAUDE.md
   ```

## Inicialización de sistema

Si el usuario está comenzando a usar este sistema por primera vez:

1. **Crear estructura de directorios:**
   ```
   mkdir -p doc/agents-sessions
   mkdir -p doc/releases
   ```

2. **Crear `changelog-sessions.md`:**
   ```markdown
   # Changelog de Sesiones
   
   Historial detallado de todas las sesiones de trabajo con agentes IA.
   
   ## Formato
   
   Cada entrada representa una sesión completa de trabajo.
   
   ---
   
   [Las sesiones se irán añadiendo aquí al cerrar versiones]
   ```

3. **Verificar existencia de `CHANGELOG.md`:**
   - Si no existe, crearlo con estructura estándar
   - Si existe, respetar su contenido actual

## Personalización

El skill puede adaptarse según preferencias del usuario:

- **Alias del agente:** Por defecto "CLAUDE", pero configurable
- **Nivel de detalle:** Conciso | Detallado | Minimal
- **Frecuencia de actualización:** Automática | Manual | Milestones
- **Idioma:** Español (default) | Inglés | Bilingüe

---

## Conclusión

Este skill permite mantener una trazabilidad completa del trabajo realizado con agentes IA, asegurando que ninguna información se pierda en compactaciones de contexto y facilitando la generación de documentación de releases de alta calidad.

**Principio fundamental:** La documentación se genera automáticamente durante el trabajo, no como tarea posterior.
