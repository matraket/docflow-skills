# Workflow Detallado - Session Manager

Este documento describe el comportamiento completo del skill en cada fase.

---

## 1. Detección Automática de Nueva Sesión

Al inicio de conversación:

1. Verificar si existe `doc/agents-sessions/`
   - Si NO → Crear directorio
2. Determinar número de sesión: Listar archivos `YYYYMMDD-*-CLAUDE.md`, incrementar contador
3. Crear archivo: `YYYYMMDD-XXX-CLAUDE.md` con metadata inicial
4. Informar discretamente: "Sesión documentada en `doc/agents-sessions/20260209-001-CLAUDE.md`"

---

## 2. Actualización Durante la Sesión (PROACTIVA)

**El agente documenta AUTOMÁTICAMENTE cuando:**
- Se complete tarea significativa (ver [`when-to-document.md`](when-to-document.md))
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

---

## 3. Cierre de Sesión

Cuando el usuario lo solicite:

1. Actualizar métricas finales
2. Completar "Próximos Pasos"
3. Añadir estado final: "Completada"
4. Confirmar: "Sesión documentada y cerrada"
