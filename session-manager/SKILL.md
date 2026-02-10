---
name: session-manager
description: Gestiona sesiones de trabajo con agentes IA. Crea y mantiene archivos de sesión en doc/agents-sessions/ con formato YYYYMMDD-XXX-{agent-alias}.md, documentando automáticamente el trabajo realizado durante cada interacción.
---

# Session Manager

Skill para gestionar sesiones de trabajo con agentes IA, manteniendo documentación detallada y trazabilidad completa del trabajo realizado.

## Propósito

Este skill permite que los agentes IA documenten automáticamente su trabajo durante las sesiones de desarrollo, manteniendo un historial completo que sobrevive a las compactaciones de contexto y sirve como fuente de verdad para generar releases.

## Principios Fundamentales (CRÍTICO)

### 1. Proactividad Obligatoria

**El agente DEBE documentar automáticamente, SIN que el usuario lo solicite**, cuando se realice trabajo significativo.

**Criterios detallados:** [`references/when-to-document.md`](references/when-to-document.md)

**Contraejemplos:** [`references/when-not-to-document.md`](references/when-not-to-document.md)

**Ejemplo completo:** [`references/example-proactive-implementation.md`](references/example-proactive-implementation.md)

**Comportamiento esperado:**
```
Usuario: "Implementa login con JWT"
↓
Agente: 
1. Implementa funcionalidad
2. DOCUMENTA AUTOMÁTICAMENTE (sin esperar instrucción)
3. Responde: "✓ Login implementado y documentado en sesión"
```

### 2. Atomicidad y No Duplicación

**ANTES de documentar, el agente DEBE:**
1. Leer físicamente el archivo de sesión actual
2. Verificar si la información ya está documentada
3. Decidir: Si YA existe → Informar | Si NO → Documentar | Si parcial → Complementar

**Principio completo:** [`references/source-of-truth-principle.md`](references/source-of-truth-principle.md)

**Ejemplos:**
- Evitar duplicación: [`references/example-avoid-duplication.md`](references/example-avoid-duplication.md)
- Complementar parcial: [`references/example-complement-partial.md`](references/example-complement-partial.md)

**Comportamiento esperado:**
```
Usuario: "Implementé login con JWT" 
↓
Agente:
1. LEE doc/agents-sessions/YYYYMMDD-XXX-CLAUDE.md
2. BUSCA si JWT/login ya documentado
3. Si SÍ → "Ya documentado en sesión (sección 14:23)"
   Si NO → Documenta
```

### 3. Fuente de Verdad Única

**El documento de sesión es la autoridad definitiva** sobre qué se ha hecho, qué decisiones se tomaron y por qué.

**Contexto conversacional:** Volátil, puede estar desactualizado, se pierde con compactaciones.

**Regla de oro:** En caso de contradicción, el documento prevalece sobre la memoria conversacional.

**Detalles:** [`references/source-of-truth-principle.md`](references/source-of-truth-principle.md)

### 4. Comportamiento Reactivo

Aunque proactivo es el core, el agente DEBE responder a instrucciones explícitas:

```
Usuario: "Anota que tenemos pendiente refactorizar X"
→ Agente verifica atomicidad y documenta en "Próximos Pasos"
```

**Ejemplo completo:** [`references/example-reactive-instruction.md`](references/example-reactive-instruction.md)

### 5. Consultas Informativas

**NO documentar** cuando el usuario solo pregunta sobre decisiones pasadas:

```
Usuario: "¿Por qué usamos JWT?"
→ Agente consulta sesión, responde, NO documenta la pregunta
```

**Ejemplo completo:** [`references/example-informational-query.md`](references/example-informational-query.md)

---

## Idioma y Localización

**IMPORTANTE:** Toda la documentación debe generarse en **Español de España**.

- **Fechas:** "DD de MMMM de YYYY" (ej: "9 de febrero de 2026")
- **Hora:** Formato 24h "HH:MM" (ej: "14:30")
- **Vocabulario:** Español de España (ej: "ordenador" no "computadora")
- **Meses:** enero, febrero, marzo, abril, mayo, junio, julio, agosto, septiembre, octubre, noviembre, diciembre

---

## Estructura de Archivos

```
doc/
└── agents-sessions/
    ├── changelog-sessions.md
    ├── 20260209-001-CLAUDE.md
    ├── 20260209-002-CLAUDE.md
    └── ...
```

**Template completo:** [`assets/templates/session-file-template.md`](assets/templates/session-file-template.md)

---

## Comportamiento del Skill

### 1. Detección Automática de Nueva Sesión

Al inicio de conversación:

1. Verificar si existe `doc/agents-sessions/`
   - Si NO → Crear directorio + `changelog-sessions.md`
2. Determinar número de sesión: Listar archivos `YYYYMMDD-*-CLAUDE.md`, incrementar contador
3. Crear archivo: `YYYYMMDD-XXX-CLAUDE.md` con metadata inicial
4. Informar discretamente: "Sesión documentada en `doc/agents-sessions/20260209-001-CLAUDE.md`"

### 2. Actualización Durante la Sesión (PROACTIVA)

**El agente documenta AUTOMÁTICAMENTE cuando:**
- Se complete tarea significativa (ver [`references/when-to-document.md`](references/when-to-document.md))
- Se tome decisión técnica con alternativas
- Se modifiquen archivos que afecten lógica de negocio

**ANTES de documentar:**
1. Leer archivo de sesión
2. Verificar atomicidad
3. Si existe → No duplicar
4. Si no existe → Documentar
5. Si parcial → Complementar

**Actualización incremental:**
- NO reescribir archivo completo
- Añadir nueva sección con timestamp
- Actualizar "Hora de últimos trabajos"

### 3. Cierre de Sesión

Cuando el usuario lo solicite:

1. Actualizar métricas finales
2. Completar "Próximos Pasos"
3. Añadir estado final: "Completada"
4. Confirmar: "Sesión documentada y cerrada"

---

## Niveles de Detalle

### Modo CONCISO (por defecto)
- Resumen 2-3 líneas por tarea
- Solo decisiones técnicas importantes
- Métricas básicas

### Modo DETALLADO (cuando se solicita)
- Descripción completa de cada paso
- Todas decisiones con alternativas
- Código relevante incluido
- Métricas exhaustivas

### Modo MINIMAL (para tareas triviales)
- Solo timestamp + título + resultado
- Sin métricas detalladas

---

## Reglas Importantes

### ✅ HACER

- **Documentar PROACTIVAMENTE** sin esperar instrucción
- **Verificar atomicidad** antes de documentar (leer sesión primero)
- **Usar documento como fuente de verdad** (no confiar solo en contexto)
- Documentar decisiones con contexto y alternativas consideradas
- Mantener timestamps precisos (formato 24h)
- Actualizar incrementalmente (no reescribir)
- Usar lenguaje específico (no vago)
- Incluir referencias (commits, archivos, issues)
- Separar "Trabajo Realizado" de "Próximos Pasos"

### ❌ NO HACER

- **NO esperar** a que usuario diga "documenta esto"
- **NO duplicar** información ya documentada
- **NO confiar solo** en memoria conversacional
- **NO documentar** consultas informativas del usuario
- No reescribir archivo completo en actualizaciones
- No usar lenguaje vago ("se mejoró", "se arregló")
- No asumir documentación sin verificar archivo físicamente

---

## Integración con Otras Herramientas

### Con CHANGELOG.md

El archivo de sesión sirve como **fuente de verdad** para `CHANGELOG.md`:
- Cada sesión genera bloque en `Unreleased`
- Información resumida vincula a archivo de sesión
- NO duplicar, solo referenciar

### Con Releases

Al cerrar versión:
- Archivos de sesión → Release notes detallados
- `CHANGELOG.md` se genera desde sesiones
- Archivos permanecen como historial

---

## Ejemplos Completos

Todos los ejemplos están en [`references/`](references/):

1. **Proactividad:** [`example-proactive-implementation.md`](references/example-proactive-implementation.md)
2. **Atomicidad:** [`example-avoid-duplication.md`](references/example-avoid-duplication.md)
3. **Complementar:** [`example-complement-partial.md`](references/example-complement-partial.md)
4. **Consultas:** [`example-informational-query.md`](references/example-informational-query.md)
5. **Reactivo:** [`example-reactive-instruction.md`](references/example-reactive-instruction.md)

---

## Personalización

El skill puede adaptarse según preferencias:

- **Alias del agente:** Por defecto "CLAUDE", configurable
- **Nivel de detalle:** Conciso | Detallado | Minimal
- **Idioma:** Español (default) | Inglés

---

## Conclusión

Este skill mantiene trazabilidad completa del trabajo, asegurando que ninguna información se pierda en compactaciones de contexto.

**Principios clave:**

1. **Proactividad:** Documentación AUTOMÁTICA durante el trabajo
2. **Atomicidad:** SIEMPRE verificar antes de documentar
3. **Fuente de Verdad:** Documento > contexto conversacional

**Resultado:** Cada sesión se convierte en registro permanente, verificable y completo del desarrollo, eliminando documentación manual posterior y preservando conocimiento ante compactaciones de contexto.
