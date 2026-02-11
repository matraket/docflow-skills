---
name: session-manager
description: >
  Gestiona sesiones de trabajo con agentes IA. Documenta automáticamente 
  trabajo significativo en doc/agents-sessions/ con formato YYYYMMDD-XXX-{agent}.md.
  Trigger: documentar sesión, crear sesión, actualizar sesión, verificar atomicidad,
  fuente de verdad, cierre de sesión, trabajo significativo completado.
metadata:
  author: adrianalbr
  version: "1.3.0"
  scope: [root]
allowed-tools: Read, Edit, Write, Bash
---

# Session Manager

Documenta automáticamente trabajo con agentes IA, preservando conocimiento ante compactaciones.

## Principios Core (CRÍTICO)

### 1. Proactividad (AUTOMÁTICA)
Documentar cuando: trabajo significativo | decisión técnica | modificación lógica negocio  
Ver: [`when-to-document.md`](references/when-to-document.md), [`example-proactive-implementation.md`](references/example-proactive-implementation.md)

### 2. Atomicidad (SIEMPRE)
ANTES de documentar: Leer archivo → Buscar contenido → Decidir (documentar | skip | complementar)  
Ver: [`source-of-truth-principle.md`](references/source-of-truth-principle.md), [`example-avoid-duplication.md`](references/example-avoid-duplication.md), [`example-complement-partial.md`](references/example-complement-partial.md)

### 3. Fuente de Verdad
Documento > contexto conversacional (en conflictos)  
Ver: [`source-of-truth-principle.md`](references/source-of-truth-principle.md)

### 4. Reactivo
Usuario solicita explícitamente → Verificar atomicidad → Documentar  
Ver: [`example-reactive-instruction.md`](references/example-reactive-instruction.md)

### 5. Consultas NO Se Documentan
Usuario pregunta sobre decisiones pasadas → Consultar sesión → Responder → NO documentar pregunta  
Ver: [`example-informational-query.md`](references/example-informational-query.md)

---

## Localización

**Idioma:** Español de España  
**Fechas:** DD de MMMM de YYYY (9 de febrero de 2026)  
**Hora:** 24h HH:MM (14:30)  
**Vocabulario:** ordenador (no computadora)

---

## Estructura

```
doc/agents-sessions/
├── YYYYMMDD-001-CLAUDE.md
└── YYYYMMDD-002-CLAUDE.md
```

**Template:** [`session-file-template.md`](assets/templates/session-file-template.md)

---

## Workflow

**Nueva sesión:**
1. Verificar/crear `doc/agents-sessions/`
2. Secuencia: listar `YYYYMMDD-*-CLAUDE.md` → incrementar
3. Crear `YYYYMMDD-XXX-CLAUDE.md`
4. Informar: "Sesión YYYYMMDD-XXX-CLAUDE.md creada"

**Durante (AUTOMÁTICO):**
1. Leer archivo → Verificar atomicidad → Si nuevo: documentar | Si existe: skip | Si parcial: complementar
2. Actualizar incrementalmente (NO reescribir completo)
3. Añadir sección con timestamp
4. Actualizar "Hora últimos trabajos"

**Cierre:**
Actualizar métricas → Completar "Próximos Pasos" → Estado: "Completada"

Ver workflow detallado: [`workflow-detailed.md`](references/workflow-detailed.md)

---

## Niveles de Detalle

| Modo | Contenido | Métricas |
|------|-----------|----------|
| **CONCISO** (default) | Resumen 2-3 líneas/tarea, decisiones importantes | Básicas |
| **DETALLADO** | Pasos completos, todas decisiones, código relevante | Exhaustivas |
| **MINIMAL** | Timestamp + título + resultado | N/A |

---

## ✅ HACER

- Documentar PROACTIVAMENTE (sin esperar instrucción)
- Verificar atomicidad SIEMPRE (leer sesión primero)
- Usar documento como fuente de verdad (no solo contexto)
- Documentar decisiones con contexto + alternativas
- Timestamps precisos (formato 24h)
- Actualizar incrementalmente (no reescribir)
- Lenguaje específico (no vago)
- Incluir referencias (commits, archivos, issues)
- Separar "Trabajo Realizado" vs "Próximos Pasos"

## ❌ NO HACER

- NO esperar instrucción "documenta esto"
- NO duplicar información ya documentada
- NO confiar solo en memoria conversacional
- NO documentar consultas informativas del usuario
- No reescribir archivo completo en actualizaciones
- No lenguaje vago ("se mejoró", "se arregló")
- No asumir documentación sin verificar archivo físicamente

---

## Referencias Completas

Ver [`references/`](references/) para:
- Cuándo documentar / cuándo NO
- Principio fuente de verdad
- Ejemplos completos (proactividad, atomicidad, reactivo, consultas)
- Workflow detallado paso a paso
