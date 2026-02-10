# Session Manager Skill

Skill para gestionar sesiones de trabajo con agentes IA, documentando automáticamente el progreso y manteniendo trazabilidad completa.

## 🎯 Propósito

Permite que agentes IA documenten automáticamente su trabajo durante las sesiones de desarrollo, generando archivos de sesión detallados que sirven como fuente de verdad para:

- Mantener contexto entre compactaciones
- Generar CHANGELOGs automáticamente
- Crear release notes de calidad
- Auditar trabajo realizado
- Facilitar handoffs entre sesiones

## 📦 Instalación

1. **Copiar el skill a tu proyecto:**
   ```bash
   cp -r session-manager /path/to/your/skills/
   ```

2. **Habilitar en Claude:**
   - El skill se activará automáticamente cuando esté en la ruta de skills
   - No requiere configuración adicional

## 🚀 Uso

### Inicio automático

El skill detecta automáticamente cuando inicias una nueva conversación y:

1. Crea el directorio `doc/agents-sessions/` si no existe
2. Genera un nuevo archivo de sesión: `YYYYMMDD-XXX-CLAUDE.md`
3. Te informa discretamente de la documentación

**No necesitas hacer nada**, la sesión se documenta automáticamente.

### Durante el trabajo

El skill actualiza el archivo de sesión cuando:

- Completas tareas significativas
- Tomas decisiones técnicas
- Modificas archivos importantes
- Solicitas un resumen del progreso

### Solicitar resumen

```
Usuario: ¿Qué hemos hecho hasta ahora?
Claude: [Genera resumen desde el archivo de sesión]
```

### Cerrar sesión

```
Usuario: Cierra la sesión
Claude: [Completa métricas, estado final, próximos pasos]
```

## 📁 Estructura generada

```
tu-proyecto/
└── doc/
    └── agents-sessions/
        ├── changelog-sessions.md       # Historial de sesiones cerradas
        ├── 20260209-001-CLAUDE.md     # Sesión del 9 feb, primera del día
        ├── 20260209-002-CLAUDE.md     # Sesión del 9 feb, segunda del día
        └── ...
```

## 🎨 Ejemplo de archivo de sesión

```markdown
# Sesión Agente: 20260209-001-CLAUDE

* **Agente de IA:** Claude (Sonnet 4.5)
* **Fecha creación:** 9 de febrero de 2026
* **Hora de inicio:** 14:15
* **Hora de últimos trabajos:** 16:42

---

## 📋 Resumen de la Sesión

Implementación de sistema de autenticación JWT para API REST.

---

## 🎯 Objetivos

- [x] Implementar login con JWT
- [x] Crear middleware de autenticación
- [ ] Integrar con frontend
- [x] Tests unitarios

---

## 💼 Trabajo Realizado

### 14:23 - Implementación de Login con JWT

**Descripción:**
Sistema de autenticación usando JSON Web Tokens con refresh tokens.

**Archivos modificados:**
- `src/auth/login.js` - Nueva función `authenticateUser()`
- `src/middleware/auth.js` - Middleware de validación

**Decisiones técnicas:**
- JWT en lugar de sessions (escalabilidad)
- Tokens expiran en 24h
- RefreshTokens en DB para revocación

**Resultados:**
- ✅ Login funcional
- ✅ Tests pasando (12/12)

---

### 15:30 - Middleware de protección de rutas

[...]

---

## 📊 Métricas de la Sesión

- **Duración total:** 2 horas 27 minutos
- **Archivos modificados:** 8
- **Archivos creados:** 3
- **Tests creados:** 12
- **Líneas añadidas:** ~450
- **Líneas eliminadas:** ~120

---

**Estado final:** Completada
**Próxima sesión:** Integración con frontend React
```

## ⚙️ Configuración

### Niveles de detalle

Por defecto: **CONCISO**

Puedes cambiar el nivel:

```
Usuario: Usa modo detallado para documentar
Claude: [Documenta con más exhaustividad]

Usuario: Vuelve a modo conciso
Claude: [Reduce nivel de detalle]
```

### Alias del agente

Por defecto: **CLAUDE**

```
Usuario: Usa "GPT4" como alias en las sesiones
Claude: [Cambia a 20260209-001-GPT4.md]
```

### Idioma

**Por defecto:** Español de España

Toda la documentación se genera en español peninsular:
- Fechas: "9 de febrero de 2026"
- Hora: formato 24h "14:30"
- Vocabulario: ordenador, aplicación, móvil

No configurable - es un estándar del sistema de documentación.

## 🔄 Integración con CHANGELOG

Los archivos de sesión sirven como fuente para actualizar `CHANGELOG.md`:

1. **Durante desarrollo:** Cada sesión genera entrada en `Unreleased`
2. **Al cerrar versión:** Se consolida en bloque versionado
3. **Información detallada:** Permanece en archivos de sesión

Ver: [changelog-updater skill](../changelog-updater/) para automatizar esto.

## 🎯 Casos de uso

### 1. Desarrollo en múltiples sesiones

Trabaja hoy en autenticación, mañana en API, pasado en frontend. Cada sesión queda documentada independientemente.

### 2. Handoff entre agentes

Sesión 1 con Claude, Sesión 2 con otro agente. El archivo de sesión anterior mantiene el contexto completo.

### 3. Auditoría de trabajo

Revisa qué se hizo en cada sesión, cuándo, y por qué se tomaron decisiones específicas.

### 4. Generación de releases

Al cerrar versión 1.0.1, se consolidan todas las sesiones en documentación de release profesional.

## 📚 Documentación relacionada

- **SKILL.md:** Documentación completa del skill
- **instrucciones-documentacion.md:** Metodología completa del sistema
- **doc/releases/README.md:** Guía de releases

## 🤝 Contribuir

Este skill es parte de un sistema de documentación automática para desarrollo asistido por IA.

**Skills relacionados:**
- `changelog-updater`: Mantiene CHANGELOG.md actualizado
- `release-generator`: Genera notas de release desde sesiones

## 📝 Licencia

MIT License - Ver LICENSE para detalles

---

**Creado por:** Adrián (IPGSoft)  
**Versión:** 1.3.0-modular (estructura assets/)  
**Última actualización:** 9 de febrero de 2026

## 🎯 Skill Session Manager

Sistema proactivo de documentación automática de sesiones de trabajo con agentes IA.

**Core:** SKILL.md reducido a **257 líneas (~1300 tokens)** - Reducción del **75%**

**Estructura modular:** Ejemplos y referencias en `assets/` para carga bajo demanda.

---

## 📁 Estructura

```
session-manager/
├── SKILL.md                    (257 líneas - core esencial)
├── README.md
├── evals/
│   └── evals.json
├── assets/                     (recursos estáticos)
│   └── templates/
│       └── session-file-template.md
└── references/                 (documentación de referencia)
    ├── when-to-document.md
    ├── when-not-to-document.md
    ├── source-of-truth-principle.md
    ├── example-proactive-implementation.md
    ├── example-avoid-duplication.md
    ├── example-complement-partial.md
    ├── example-informational-query.md
    └── example-reactive-instruction.md
```

**Cumple con:** [agentskills.io](https://agentskills.io) especificaciones oficiales

---

## 🆕 Cambios en v1.3.0-modular

**Refactorización mayor con estructura según agentskills.io:**

- ✅ **SKILL.md reducido:** 594 → 257 líneas (-57%), 5275 → 1300 tokens (-75%)
- ✅ **Estructura oficial:** Cumple con especificaciones de agentskills.io
- ✅ **references/** al nivel raíz con ejemplos y criterios
- ✅ **assets/** solo para recursos estáticos (templates)
- ✅ **Carga bajo demanda:** Claude solo lee lo que necesita
- ✅ **Mantenibilidad:** Archivos pequeños y específicos

Ver [REORGANIZACION-AGENTSKILLS.md](doc/REORGANIZACION-AGENTSKILLS.md) para detalles.

---

## 🆕 Cambios en v1.3.0 (behavioral)

**Transformación fundamental: De reactivo a proactivo**

- ✅ **Nueva sección:** "Principios de Documentación (CRÍTICO)" (ahora en SKILL.md core)
- ✅ **Proactividad obligatoria:** Agente documenta automáticamente SIN instrucción explícita
- ✅ **Atomicidad garantizada:** Siempre verifica archivo antes de documentar
- ✅ **Fuente de verdad:** Documento de sesión > contexto conversacional
- ✅ **Criterios concretos:** 17 criterios explícitos (ahora en assets/references/)

Ver [CAMBIOS-v1.3.0.md](doc/CAMBIOS-v1.3.0.md) para detalles de comportamiento.

---

## 🆕 Cambios en v1.0.2

- ✅ **Idioma verificado en TODOS los evals** (8/8)
- ✅ **Criterios medibles** en lugar de subjetivos
- ✅ **Verificación de calidad** vs solo existencia
- ✅ **+7 assertions nuevas, 11 mejoradas** (52 → 59 total)

Ver [CAMBIOS-v1.0.2.md](doc/CAMBIOS-v1.0.2.md) para detalles completos.

---

## 🆕 Cambios en v1.0.1

- ✅ Assertions mejoradas: verifican calidad, no solo existencia
- ✅ Criterio "brief" ahora concreto: <200 caracteres
- ✅ Especificación explícita: **todo en Español de España**
- ✅ Verificación de estructura completa de secciones

Ver [CAMBIOS-v1.0.1.md](doc/CAMBIOS-v1.0.1.md) para detalles de v1.0.1.

---

## 📖 Cómo Usar

### Para Claude (agentes IA)

1. **Lee SKILL.md** (~1300 tokens, principios core)
2. **Si necesita ejemplos:** Lee `references/example-*.md`
3. **Si necesita criterios:** Lee `references/when-*.md` o `references/source-of-truth-principle.md`
4. **Si necesita template:** Lee `assets/templates/session-file-template.md`

### Para Desarrolladores

**Leer documentación:**
- Empezar por `SKILL.md` para entender el sistema
- Consultar `references/example-*.md` para casos de uso específicos
- Consultar `references/when-*.md` para criterios detallados

**Extender skill:**
- Añadir ejemplo: Crear archivo en `references/example-*.md`
- Añadir criterio: Editar archivos en `references/`
- SKILL.md permanece estable (~257 líneas)

---

## 🎯 Principios Fundamentales

1. **Proactividad:** Documenta automáticamente durante el trabajo
2. **Atomicidad:** Verifica antes de documentar (no duplica)
3. **Fuente de Verdad:** Documento de sesión es autoridad definitiva

**Detalles:** Ver `SKILL.md` sección "Principios Fundamentales"

---

## 📊 Estadísticas

| Métrica | v1.0.0 | v1.3.0 Original | v1.3.0 Modular |
|---------|--------|-----------------|----------------|
| Líneas SKILL.md | 363 | 594 | **257** |
| Tokens SKILL.md | ~2800 | ~5275 | **~1300** |
| Ejemplos | 3 | 7 | 5 (en assets/) |
| Referencias | 0 | 0 | 3 (en assets/) |
| Extensibilidad | Baja | Baja | **Alta** |

---

## 🔗 Documentación

- **SKILL.md** - Instrucciones core para Claude
- **REORGANIZACION-AGENTSKILLS.md** - Cumplimiento de especificaciones oficiales
- **REFACTORIZACION-v1.3.0.md** - Análisis de estructura modular
- **CAMBIOS-v1.3.0.md** - Cambios de comportamiento proactivo
- **references/** - Ejemplos completos y criterios detallados
- **assets/templates/** - Templates de archivos de sesión

