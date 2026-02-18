### Objetivo

Quiero trabajar con **Agentes IA** para que asiste en el desarrollo de software. Quiero que el propio agente tambien vaya generando automáticamente la documentación **trabajo realizado**, **Changelog**, etc a lo largo del desarrollo. Esta información servirá para tener una trazabilidad del trabajo relizado y de las iteracciones con el agente. Por otro lado tamnién define la metodologia a seguir para generar los releases notes de los cierres de versión.

### Enfoque de trabajo

* A medida que vaya creando distintas **sesiones de trabajo en Agente**, el agente debe mantener un **archivo Markdown por sesión**, donde se documentará todo lo realizado durante esa sesión.

* Este **documento de sesión** servirá para mantener, de forma **detallada y completa**, todo lo que se vaya realizando durante la sesión, conservando el **contexto íntegro** incluso cuando Agente realice **compactaciones periódicas** del historial.

* Los **próximos pasos, tareas pendientes, ideas abiertas o anotaciones prospectivas** se registrarán **exclusivamente en los documentos de sesión**, nunca en el `Changelog`, asegurando la alineación y evitando descoordinaciones.

* Cada sesión representará un bloque de trabajo independiente y autocontenido.

### Gestión del Changelog

* En el **Changelog**, dentro de la sección [Unreleased], el asistente deberá:

  * Generar los cambios **agrupados por sesión**, no de forma global.

  * Mantener únicamente información **resumida y sintética**, orientada a alto nivel.

  * Clasificar los cambios de cada sesión según el estándar de Changelog:

    * `Added`
    * `Modified`
    * `Removed` (si aplica)

  * Cada entrada deberá servir para **vincular el cambio con el archivo detallado de la sesión correspondiente**, sin duplicar información.

* Cada día o sesión de trabajo añadirá un **nuevo bloque** dentro de la sección `Unreleased`, claramente diferenciado del resto.

* Dentro de la información incluida en el bloque [Unreleased] **no deberá incorporarse** nada relativo a **próximos pasos, tareas futuras, pendientes o acciones por realizar**; únicamente se reflejará **lo que se haya realizado efectivamente durante la sesión**.

### Organización de archivos

Estructura de directorios en **texto plano**:

```
/
├─doc/
│  ├─ agents-sessions/
│  │  ├─ changelog-sessions.md
│  │  ├─ AAAAMMDD-001-{user}-{agent-alias}.md
│  │  ├─ AAAAMMDD-002-{user}-{agent-alias}.md
│  │  └─ ...
│  │
│  └─ releases/
│     ├─ README.md
│     ├─ template.md
│     ├─ X.Y.Z.md
│     └─ ...
│
└─ CHANGELOG.md
```

* Los **archivos de sesión** de los agentes se almacenarán en el directorio:

  * `doc/agents-sessions/`

* El **Changelog de sesiones** se almacenará en este mismo directorio y se llamará:

  * `changelog-sessions.md`

* El nombre de cada archivo de sesión seguirá el formato:

  * **`AAAAMMDD-YYY-{user}-{agent-alias}.md`**

  donde:

  * `AAAAMMDD` corresponde a la fecha de la sesión.
  * `YYY` es un contador incremental (`001`, `002`, etc.) en caso de existir más de una sesión en el mismo día.
  * `{user}` usuario que inteactua con el agente de IA
  * `{agent-alias}` es un alias asignado a cada agente

  **Ejemplo:** `20260112-001-adrian-CLAUDE.md`

* Los **archivos de documentación de releases** se almacenarán en el directorio:

  * `docs/releases/`

* En este directorio existirá un archivo **`README.md`**, que actuará como la **guía maestra y el índice central** de las notas de lanzamiento del proyecto.

  * Su misión es garantizar la **consistencia en la documentación** de cada versión, proporcionando el **procedimiento estándar**, las **métricas de calidad** y las **herramientas necesarias** para transformar el historial de cambios en un registro profesional y estructurado para desarrolladores y *stakeholders*.
  * Este documento deberá **mantenerse actualizado** a medida que se vayan cerrando los releases.

* En este mismo directorio existirá un archivo **`template.md`**, que actuará como **plantilla base obligatoria** para generar los nuevos documentos de release.

* Cada archivo de release se nombrará con el **número de versión**, seguido de la extensión `.md`, y se generará **a partir de `template.md`**.

  **Ejemplo:** al cerrar el release **1.0.1**, el archivo será `1.0.1.md`.

### Estructura del archivo de sesión

Cada archivo de sesión se generará automáticamente con la siguiente estructura base:

```markdown
# Sesión Agente: YYYYMMDD-YYY-{agent-alias}

* **Agente de IA:** ...
* **Fecha creación:** ...
* **Hora de inicio:** ...
* **Hora de últimos trabajos:** ...

[El contenido del archivo se generará automáticamente en el transcurso de la sesión]
```

### Estructura canónica del CHANGELOG

La siguiente es la **estructura obligatoria** del archivo `CHANGELOG.md`, que deberá respetarse en todo momento.

```markdown
# Changelog

Todos los cambios notables en este proyecto serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es/1.0.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/lang/es/).

## [Unreleased]

### YYYYMMDD-XXX-{user}-{agent-alias}

* **Fecha de sesión:** ...
* **Hora de inicio:** ...
* **Hora de últimos trabajos:** ...
* **Documento de sesión:** [doc/agents-sessions/YYYYMMDD-XXX-USER-AGENTEIA.md](doc/agents-sessions/YYYYMMDD-XXX-USER-AGENTEIA.md)

#### Added

    [Se actualizará durante la sesión]

#### Changed

    [Se actualizará durante la sesión]

#### Fixed

    [Se actualizará durante la sesión]

#### Removed

    [Se actualizará durante la sesión]

---

[Resto de bloques de sesión]

---

## [X.Y.Z] - YYYY-MM-DD

* **Fecha de release:** ...
* **Tipo:** ...
* **Periodo de desarrollo:** ...
* **Commits:** XX commits desde `abc1234` hasta `abc5467`
* **Cobertura de tests:** XX.XX% (+X.X% desde YY.YY%)
* **Tests:** XXX/YYY tests pasando (ZZ.Z%, antes XXX/YYY - ZZ.Z%)
* **Sesiones de trabajo:**
    - [doc/agents-sessions/YYYYMMDD-XXX-USER-AGENTEIA.md](doc/agents-sessions/YYYYMMDD-XXX-USER-AGENTEIA.md)
    - [doc/agents-sessions/YYYYMMDD-YYY-USER-AGENTEIA.md](doc/agents-sessions/YYYYMMDD-YYY-USER-AGENTEIA.md)
    - ...

[Información introductoria si procede]

### Added

    [Información agregada de todas las sesiones de trabajo]

### Changed

    [Información agregada de todas las sesiones de trabajo]

### Fixed

    [Información agregada de todas las sesiones de trabajo]

### Removed

    [Información agregada de todas las sesiones de trabajo]

[Información de finalización / footer si procede]

---

[Resto de releases]
```


### Cierre de versión

* Cuando llegue el momento de cerrar una versión:

  * La sección [Unreleased], **junto con los archivos de cada sesión**, servirá como **fuente única de verdad** para:

    * Generar la documentación final de la versión.
    * Crear la nueva sección versionada correspondiente dentro del `CHANGELOG.md`.

  * Dentro de cada **bloque de release**, la información dejará de organizarse por **bloques de sesión** y pasará a presentarse de forma **agregada**, siguiendo la estructura estándar de los *Changelogs*:

    * `Added`
    * `Modified`
    * `Removed`
    * etc.

  * En el bloque de release **únicamente se hará referencia al conjunto de archivos de sesión** utilizados para su elaboración, sin repetir el detalle individual de cada sesión.

  * Los **archivos de documentación del release** mantendrán una información **exhaustiva y detallada** de todos los cambios, modificaciones y eliminaciones realizados en dicho release.

  * La sección correspondiente a ese release dentro del **`CHANGELOG.md`** contendrá **únicamente una versión resumida** de esos cambios, alineada con las buenas prácticas de los *Changelogs*.

* La información extendida y detallada de cada sesión:

  * Se moverá o consolidará en un **Changelog de sesiones** (o histórico de sesiones), manteniendo un nivel de detalle mayor que el Changelog principal.
  * La información que se traspase desde el bloque [Unreleased] del *Changelog* al archivo de **Changelog de sesiones** **no deberá modificarse** en ningún caso; se tomará el contenido del bloque [Unreleased] y se **trasladará íntegramente**, preservando exactamente su redacción y estructura.
  * Una vez completado este proceso y cerrada la versión, la sección [Unreleased] quedará **vacía**, lista para iniciar un **nuevo flujo de trabajo** basado en nuevas sesiones de Agente.

al cerrar la version el CHANGELOG.md quedará de la siguiente forma

```markdown
# Changelog

Todos los cambios notables en este proyecto serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es/1.0.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/lang/es/).

## [Unreleased]

[Vacío pendiente de iniciar nuevas sesiones]

---

## [X.Y.Z] - YYYY-MM-DD <-- SESION QUE SE ACABA DE CERRAR

* **Fecha de release:** ...
* **Tipo:** ...
* **Periodo de desarrollo:** ...
* **Commits:** XX commits desde `abc1234` hasta `abc5467`
* **Cobertura de tests:** XX.XX% (+X.X% desde YY.YY%)
* **Tests:** XXX/YYY tests pasando (ZZ.Z%, antes XXX/YYY - ZZ.Z%)
* **Sesiones de trabajo:**
    - [doc/agents-sessions/YYYYMMDD-ZZZ-USER-AGENTEIA.md](doc/agents-sessions/YYYYMMDD-ZZZ-USER-AGENTEIA.md)
    - ...

[Información introductoria si procede]

### Added

    [Información agregada de todas las sesiones de trabajo]

### Changed

    [Información agregada de todas las sesiones de trabajo]

### Fixed

    [Información agregada de todas las sesiones de trabajo]

### Removed

    [Información agregada de todas las sesiones de trabajo]

[Información de finalización / footer si procede]

---

## [X.Y.Z] - YYYY-MM-DD

* **Fecha de release:** ...
* **Tipo:** ...
* **Periodo de desarrollo:** ...
* **Commits:** XX commits desde `abc1234` hasta `abc5467`
* **Cobertura de tests:** XX.XX% (+X.X% desde YY.YY%)
* **Tests:** XXX/YYY tests pasando (ZZ.Z%, antes XXX/YYY - ZZ.Z%)
* **Sesiones de trabajo:**
    - [doc/agents-sessions/YYYYMMDD-XXX-USER-AGENTEIA.md](doc/agents-sessions/YYYYMMDD-XXX-USER-AGENTEIA.md)
    - [doc/agents-sessions/YYYYMMDD-YYY-USER-AGENTEIA.md](doc/agents-sessions/YYYYMMDD-YYY-USER-AGENTEIA.md)
    - ...

[Información introductoria si procede]

### Added

    [Información agregada de todas las sesiones de trabajo]

### Changed

    [Información agregada de todas las sesiones de trabajo]

### Fixed

    [Información agregada de todas las sesiones de trabajo]

### Removed

    [Información agregada de todas las sesiones de trabajo]

[Información de finalización / footer si procede]

---

[Resto de releases]
```



### Resultado esperado

* Un **Changelog principal** limpio, estructurado y alineado con buenas prácticas.
* Un **histórico detallado por sesiones** que permita trazabilidad completa del trabajo realizado.
* Un flujo de documentación progresivo, automático y sin retrabajo.
