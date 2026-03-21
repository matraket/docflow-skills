# doc-work-skill

## Proposito

Coleccion de 3 skills integrados que forman una cadena de documentacion automatizada para proyectos asistidos por IA. Cubren desde el registro de sesiones de trabajo hasta la generacion de release notes, garantizando trazabilidad completa del proceso de desarrollo.

## Flujo de Trabajo

```
 ┌────────────────────┐   sesiones    ┌────────────────────┐   cambios   ┌────────────────────┐
 │                    │  documentadas │                    │  agrupados  │                    │
 │   session-manager  ├──────────────►│  changelog-updater ├────────────►│  release-generator │
 │                    │               │                    │             │                    │
 └────────┬───────────┘               └─────────┬──────────┘             └─────────┬──────────┘
          │                                     │                                  │
          │  Registra trabajo                   │  Actualiza seccion               │  Cierra version,
          │  y decisiones de                    │  [Unreleased] del                │  genera release
          │  cada sesion IA                     │  CHANGELOG.md                    │  notes y archiva
          │                                     │                                  │  sesiones
          ▼                                     ▼                                  ▼
 ┌───────────────────────┐              ┌────────────────────┐             ┌───────────────────────┐
 │ changelog-sessions.md │              │ CHANGELOG.md       │             │ Release Notes         │
 │                       │              │                    │             │ changelog-sessions.md │
 └───────────────────────┘              └────────────────────┘             └───────────────────────┘
```

1. **session-manager** registra el trabajo realizado durante cada sesion de IA, preservando contexto y decisiones tecnicas.
2. **changelog-updater** toma esa informacion y actualiza la seccion `[Unreleased]` del `CHANGELOG.md`, agrupando los cambios por sesion.
3. **release-generator** cierra la version, genera las release notes detalladas y preserva el historico de sesiones en `changelog-sessions.md`.

## Skills

### session-manager

Documenta automaticamente el trabajo de sesiones IA preservando contexto, decisiones tecnicas y trazabilidad completa ante compactaciones de memoria.

Ver documentacion completa: [session-manager/README.md](skills/session-manager/README.md)

### changelog-updater

Mantiene la seccion `[Unreleased]` del `CHANGELOG.md` actualizada automaticamente, agrupando cambios por sesion de trabajo.

Ver documentacion completa: [changelog-updater/README.md](skills/changelog-updater/README.md)

### release-generator

Automatiza el cierre completo de versiones generando release notes detallados, actualizando CHANGELOG.md y preservando historico de sesiones en `changelog-sessions.md`.

Ver documentacion completa: [release-generator/README.md](skills/release-generator/README.md)

## Instalacion

1. Clonar o copiar las carpetas de los skills que se necesiten al directorio de skills del proyecto destino.
2. Registrar los skills en el archivo `CLAUDE.md` del proyecto, en la tabla de skills disponibles.
3. Configurar los archivos de documentacion base (`CHANGELOG.md`, `changelog-sessions.md`) segun las instrucciones de cada skill.

Los tres skills estan disenados para funcionar en conjunto como cadena, pero cada uno puede usarse de forma independiente si solo se necesita una parte del flujo.

## Documentacion Adicional

- [instrucciones-documentacion.md](instrucciones-documentacion.md) -- Instrucciones de documentacion del proyecto.
- [doc_releases_README.md](doc_releases_README.md) -- Documentacion del proceso de releases.
- [doc_releases_template.md](doc_releases_template.md) -- Template para release notes.
- [AGENTS.md](AGENTS.md) -- Guia para agentes del proyecto.

## Licencia

MIT License

---

> **Creado por:** Adrian (IPGSoft)
> **Ultima actualizacion:** 2026-03-21
