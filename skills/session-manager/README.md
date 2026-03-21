# Session Manager Skill

Skill para gestionar sesiones de trabajo con agentes IA, documentando automáticamente el progreso y manteniendo trazabilidad completa.

## 🎯 Propósito

Permite que agentes IA documenten automáticamente su trabajo durante las sesiones de desarrollo, manteniendo un historial completo que:

- **Preserva contexto** ante compactaciones de memoria
- **Registra decisiones técnicas** con reasoning y alternativas consideradas
- **Documenta trabajo realizado** de forma incremental y proactiva
- **Sirve como fuente de verdad** para auditoría del proyecto
- **Facilita handoffs** entre sesiones y colaboradores

## 📦 Instalación

1. **Copiar el skill a tu proyecto:**
   ```bash
   cp -r session-manager /path/to/your/skills/
   ```

2. **Habilitar en Claude:**
   - El skill se activará automáticamente cuando esté en la ruta de skills
   - No requiere configuración adicional

3. **Configurar identificación de desarrollador (recomendado):**
   ```bash
   # En la raíz de tu proyecto, crear/editar .env
   echo "USER_SESSION_MANAGER=tu_nombre" >> .env
   ```
   - Si no se configura, se usará `unknown` como identificador
   - Solo caracteres alfanuméricos (sanitización automática)

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
├── .env                        # USER_SESSION_MANAGER=tu_nombre
└── doc/
    └── agents-sessions/
        ├── 20260209-001-adrian-CLAUDE.md
        ├── 20260209-002-maria-GPT4.md
        ├── 20260211-001-carlos-COPILOT.md
        └── ...
```

**Formato:** `YYYYMMDD-XXX-{user}-{agent}.md`
- `{user}`: Configurado en variable `USER_SESSION_MANAGER` del archivo `.env`
- `{agent}`: Alias del agente IA (ver documentación de agentes)

## 🎨 Ejemplo de archivo de sesión

```markdown
# Sesión Agente: 20260209-001-adrian-CLAUDE

* **Agente de IA:** Claude (Sonnet 4.5)
* **Desarrollador:** adrian
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

Por defecto según el agente que esté usando el skill.

Consulta la tabla de agentes IA comunes y sus alias en la documentación:
- Claude → `CLAUDE`
- GPT-4 → `GPT4`
- GitHub Copilot → `COPILOT`
- [Ver lista completa de agentes](references/agent-aliases.md)

### Idioma

**Por defecto:** Español de España

Toda la documentación se genera en español peninsular:
- Fechas: "9 de febrero de 2026"
- Hora: formato 24h "14:30"
- Vocabulario: ordenador, aplicación, móvil

No configurable - es un estándar del sistema de documentación.

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

- **SKILL.md:** Documentación compacta del skill (126 líneas)
- **references/:** Criterios, ejemplos y workflow detallado
- **CAMBIOS-v1.4.0.md:** Changelog de compactación

## 🤝 Contribuir

Este skill es parte de un sistema de documentación automática para desarrollo asistido por IA.

## 📄 Licencia

MIT License - Ver LICENSE para detalles

---

**Creado por:** Adrián (IPGSoft)  
**Versión:** 1.4.1  
**Última actualización:** 11 de febrero de 2026

---

## 🎯 Skill Session Manager

Sistema proactivo de documentación automática de sesiones de trabajo con agentes IA.

**Core:** SKILL.md reducido a **126 líneas (~2,520 tokens)** - Reducción del **60%** vs v1.3.0

**Progressive Disclosure:** Ejemplos y referencias en `references/` para carga bajo demanda.

---

## 📁 Estructura

```
session-manager/
├── SKILL.md                    (138 líneas - core esencial)
├── README.md
├── CHANGELOG.md          
├── assets/                     (recursos estáticos)
│   └── templates/
│       └── session-file-template.md
└── references/                 (documentación de referencia)
    ├── when-to-document.md
    ├── when-not-to-document.md
    ├── source-of-truth-principle.md
    ├── workflow-detailed.md
    ├── agent-aliases.md                        ← NUEVO v1.4.1
    ├── example-proactive-implementation.md
    ├── example-avoid-duplication.md
    ├── example-complement-partial.md
    ├── example-informational-query.md
    └── example-reactive-instruction.md
```

**Cumple con:** [agentskills.io](https://agentskills.io) especificaciones oficiales

---

## 🆕 Cambios en v1.4.1

**Desarrollo colaborativo y precisión temporal:**

- ✅ **Nueva nomenclatura:** `YYYYMMDD-XXX-{user}-{agent}.md` para multi-desarrollador
- ✅ **Variable USER_SESSION_MANAGER:** Identificación automática desde `.env`
- ✅ **Sanitización usuario:** Solo `[a-zA-Z0-9]` en nombres
- ✅ **Timestamps dinámicos:** Cálculo obligatorio con comandos (nunca estáticos)
- ✅ **Alias de agentes:** Tabla estandarizada en `references/agent-aliases.md`
- ✅ **Notificación configuración:** Si USER_SESSION_MANAGER no existe
- ✅ **Compatibilidad:** Sesiones v1.4.0 siguen funcionando

Ver [CAMBIOS-v1.4.1.md](CAMBIOS-v1.4.1.md) para detalles completos.

---

## 🆕 Cambios en v1.4.0

**Compactación agresiva con Progressive Disclosure:**

- ✅ **SKILL.md compactado:** 317 → 126 líneas (-60%), 6,340 → 2,520 tokens (-60%)
- ✅ **SESSION_DATE eliminado:** Era alucinación del agente, feature nunca existió (57 líneas)
- ✅ **Secciones eliminadas:** Propósito, Conclusión, Personalización (redundantes)
- ✅ **Workflow movido:** Detalles paso a paso en `references/workflow-detailed.md`
- ✅ **Principios compactados:** Explicativo → Prescriptivo (18 líneas → 2 líneas cada uno)
- ✅ **Tabla niveles detalle:** 15 líneas lista → 5 líneas tabla
- ✅ **Frontmatter extendido:** Metadata + allowed-tools + triggers

Ver [CAMBIOS-v1.4.0.md](CAMBIOS-v1.4.0.md) para detalles completos.

---

## 🆕 Cambios en v1.3.0-modular

**Refactorización mayor con estructura según agentskills.io:**

- ✅ **SKILL.md reducido:** 594 → 257 líneas (-57%), 5275 → 1300 tokens (-75%)
- ✅ **Estructura oficial:** Cumple con especificaciones de agentskills.io
- ✅ **references/** al nivel raíz con ejemplos y criterios
- ✅ **assets/** solo para recursos estáticos (templates)
- ✅ **Carga bajo demanda:** Claude solo lee lo que necesita
- ✅ **Mantenibilidad:** Archivos pequeños y específicos

---

## 🆕 Cambios en v1.3.0 (behavioral)

**Transformación fundamental: De reactivo a proactivo**

- ✅ **Nueva sección:** "Principios de Documentación (CRÍTICO)" (ahora en SKILL.md core)
- ✅ **Proactividad obligatoria:** Agente documenta automáticamente SIN instrucción explícita
- ✅ **Atomicidad garantizada:** Siempre verifica archivo antes de documentar
- ✅ **Fuente de verdad:** Documento de sesión > contexto conversacional
- ✅ **Criterios concretos:** 17 criterios explícitos (ahora en assets/references/)

---

## 📖 Cómo Usar

### Para Claude (agentes IA)

1. **Lee SKILL.md** (~2,760 tokens, principios core)
2. **Si necesita ejemplos:** Lee `references/example-*.md`
3. **Si necesita criterios:** Lee `references/when-*.md` o `references/source-of-truth-principle.md`
4. **Si necesita workflow detallado:** Lee `references/workflow-detailed.md`
5. **Si necesita alias de agentes:** Lee `references/agent-aliases.md`
6. **Si necesita template:** Lee `assets/templates/session-file-template.md`

### Para Desarrolladores

**Leer documentación:**
- Empezar por `SKILL.md` para entender el sistema
- Consultar `references/example-*.md` para casos de uso específicos
- Consultar `references/when-*.md` para criterios detallados
- Consultar `references/workflow-detailed.md` para proceso completo

**Extender skill:**
- Añadir ejemplo: Crear archivo en `references/example-*.md`
- Añadir criterio: Editar archivos en `references/`
- SKILL.md permanece estable (~126 líneas)

---

## 🎯 Principios Fundamentales

1. **Proactividad:** Documenta automáticamente durante el trabajo
2. **Atomicidad:** Verifica antes de documentar (no duplica)
3. **Fuente de Verdad:** Documento de sesión es autoridad definitiva

**Detalles:** Ver `SKILL.md` sección "Principios Core (CRÍTICO)"

---

## 📊 Estadísticas

| Métrica | v1.0.0 | v1.3.0 Original | v1.3.0 Modular | v1.4.0 | v1.4.1 |
|---------|--------|-----------------|----------------|--------|--------|
| Líneas SKILL.md | 363 | 594 | 257 | 126 | **138** |
| Tokens SKILL.md | ~2,800 | ~5,275 | ~1,300 | ~2,520 | **~2,760** |
| Secciones inline | 12 | 15 | 11 | 6 | **6** |
| Ejemplos inline | 3 | 7 | 5 | 0 | **0** |
| Referencias externas | 0 | 0 | 3 | 9 | **10** |
| Extensibilidad | Baja | Baja | Alta | Alta | **Alta** |
| Progressive Disclosure | ❌ | ⚠️ | ✅ | ✅ | **✅** |
| Multi-desarrollador | ❌ | ❌ | ❌ | ❌ | **✅** |

---

## 📗 Documentación

- **SKILL.md** - Instrucciones core para Claude (138 líneas)
- **CAMBIOS-v1.4.1.md** - Desarrollo colaborativo y timestamps dinámicos
- **CAMBIOS-v1.4.0.md** - Compactación y eliminación SESSION_DATE
- **references/agent-aliases.md** - Tabla de alias de agentes IA
- **references/workflow-detailed.md** - Workflow paso a paso
- **references/when-to-document.md** - Criterios proactividad
- **references/when-not-to-document.md** - Criterios exclusión
- **references/source-of-truth-principle.md** - Principio atomicidad
- **references/example-*.md** - Ejemplos completos de uso
- **assets/templates/** - Templates de archivos de sesión

---

## 🔄 Migración

### De v1.4.0 a v1.4.1

**Cambios en comportamiento:**
- ⚠️ **Nueva nomenclatura:** Archivos ahora incluyen `{user}` y `{agent}`
- ✅ **Retrocompatible:** Sesiones v1.4.0 siguen siendo legibles

**Archivos nuevos:**
- `references/agent-aliases.md`
- `CAMBIOS-v1.4.1.md`

**Configuración requerida:**
```bash
# Añadir a .env en raíz del proyecto
echo "USER_SESSION_MANAGER=tu_nombre" >> .env
```

**Actualización:**
```bash
# Reemplazar SKILL.md
cp SKILL-v1.4.1.md SKILL.md

# Añadir nuevo archivo de referencias
cp references/agent-aliases.md references/
```

---

### De v1.3.0 a v1.4.0

**Cambios en comportamiento:**
- ✅ **Ninguno** - Solo optimización interna

**Archivos nuevos:**
- `references/workflow-detailed.md`
- `CAMBIOS-v1.4.0.md`

**Archivos eliminados:**
- Ninguno

**Actualización:**
```bash
# Reemplazar SKILL.md
cp SKILL-v1.4.0.md SKILL.md

# Añadir workflow detallado
cp references/workflow-detailed.md references/
```

---

## 🙏 Agradecimientos

- **Anthropic** por documentación de Progressive Disclosure
- **prowler-pr skill** por servir como ejemplo de compactación
- **Usuario** por detectar alucinación SESSION_DATE

---

## 📞 Soporte

Para reportar issues o sugerir mejoras, contactar al autor o abrir issue en el repositorio del proyecto.

---

> **Creado por:** Adrian (IPGSoft)
> **Última actualización:** 2026-03-21
