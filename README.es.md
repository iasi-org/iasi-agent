🇪🇸 **Castellano** | 🇬🇧 [English](README.md)

# iasi-agent

`iasi-agent` proporciona un agente capaz de ejecutar trabajo dentro de un proyecto IASI.

## IASI

**IASI — Intelligent Assisted Software Engineering** es un enfoque de ingeniería en el que los sistemas inteligentes se integran en el proceso de ingeniería como capacidades activas de ejecución y razonamiento.

IASI no define el trabajo alrededor de un modelo, proveedor, agente o herramienta concretos.

El proyecto define su conocimiento, restricciones, tareas y resultados esperados independientemente de quién termine ejecutándolos.

```text
OUTSIDE → INPUTS → ENGINE → OUTPUTS → OUTSIDE
```

Los agentes operan dentro de ese modelo.

Son ejecutores.

No son el workflow.

## ¿Qué es `iasi-agent`?

`iasi-agent` es un agente diseñado para trabajar directamente con un workspace IASI.

Combina un modelo de lenguaje con las capacidades necesarias para operar sobre un proyecto real.

Un modelo por sí solo puede recibir un prompt y producir una respuesta:

```text
prompt → model → response
```

Eso es útil, pero no es suficiente para ejecutar trabajo de ingeniería.

Un agente necesita acceso al entorno en el que existe el trabajo:

```text
                     ┌───────────┐
                     │   MODEL   │
                     └─────┬─────┘
                           │
                     ┌─────▼─────┐
                     │   AGENT   │
                     └─────┬─────┘
                           │
                       WORKSPACE
```

El agente debe ser capaz de comprender la tarea solicitada, inspeccionar el workspace, leer los artefactos relevantes, utilizar las capacidades disponibles y materializar los resultados solicitados.

## El modelo no es el agente

IASI separa deliberadamente estos conceptos.

**Ollama**, por ejemplo, puede exponer y ejecutar modelos de lenguaje locales.

Es un runtime de modelos.

No es, por sí mismo, un agente IASI.

```text
Ollama
   ↓
model
```

`iasi-agent` añade la capa de ejecución alrededor de ese modelo:

```text
Ollama / model
      ↓
  iasi-agent
      ↓
   workspace
```

Esta separación permite que los modelos y sus runtimes evolucionen independientemente del proceso de ingeniería.

## El agente no es el workflow

Una tarea IASI describe **qué debe hacerse**.

No necesita prescribir **quién debe ejecutarla**.

```text
TASK
 │
 ├── Codex
 ├── iasi-agent
 ├── otro agente
 └── futuro ejecutor
```

El ejecutor puede cambiar sin cambiar la tarea.

Un fallo, un límite de cuota, un servicio cloud no disponible o un cambio de modelo no deberían redefinir el trabajo de ingeniería.

El workflow pertenece a IASI.

El agente ejecuta trabajo dentro de ese workflow.

## Acceso al workspace

`iasi-agent` no está pensado para comportarse simplemente como una interfaz de chat sobre un modelo.

Trabaja con un proyecto.

Eso significa que el workspace forma parte de su contexto de ejecución.

El agente puede necesitar:

- descubrir la estructura del proyecto;
- leer inputs y artefactos de ingeniería;
- comprender la tarea que debe ejecutar;
- inspeccionar outputs existentes cuando sea relevante;
- crear o modificar artefactos cuando la tarea lo permita;
- utilizar las herramientas disponibles;
- validar el resultado de su trabajo.

Tener acceso no implica poder modificar cualquier cosa.

Las reglas del proyecto IASI determinan qué puede leer, crear o modificar el agente.

Por ejemplo, los artefactos inmutables siguen siendo inmutables independientemente del agente que ejecute el trabajo.

## Ejecución local

Uno de los objetivos de `iasi-agent` es convertir la ejecución local en una posibilidad de primer nivel.

Un modelo disponible localmente puede convertirse así en un ejecutor de ingeniería cuando se combina con las capacidades de agente necesarias para operar sobre el workspace.

Conceptualmente:

```text
LOCAL MODEL
    │
    ▼
IASI AGENT
    │
    ▼
WORKSPACE
```

El objetivo no es reproducir un agente cloud concreto.

El objetivo es proporcionar las capacidades necesarias para ejecutar trabajo IASI.

## Ejecutores sustituibles

IASI no debe depender de un agente específico.

`iasi-agent` es uno de los posibles ejecutores dentro de un modelo de ejecución más amplio.

```text
                     IASI TASK
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
       Codex       iasi-agent        otro
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                     RESULT
```

Esto permite sustituir el ejecutor conservando la intención de ingeniería.

La tarea sigue siendo la tarea.

El proyecto sigue siendo el proyecto.

El agente es un ejecutor, no el workflow.
