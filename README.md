🇪🇸 [Versión en castellano](README.es.md) | 🇬🇧 **English**

# iasi-agent

`iasi-agent` provides an agent capable of executing work inside an IASI project.

## IASI

**IASI — Intelligent Assisted Software Engineering** is an approach to engineering in which intelligent systems are integrated into the engineering process as active execution and reasoning capabilities.

IASI does not define the work around a particular model, provider, agent or tool.

The project defines its knowledge, constraints, tasks and expected results independently from whoever eventually executes them.

```text
OUTSIDE → INPUTS → ENGINE → OUTPUTS → OUTSIDE
```

Agents operate inside that model.

They are executors.

They are not the workflow.

## What is `iasi-agent`?

`iasi-agent` is an agent designed to work directly with an IASI workspace.

It combines a language model with the capabilities required to operate on a real project.

A model by itself can receive a prompt and produce a response:

```text
prompt → model → response
```

That is useful, but it is not enough to execute engineering work.

An agent needs access to the environment in which the work exists:

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

The agent must be able to understand the requested task, inspect the workspace, read the relevant artifacts, use the capabilities available to it and materialize the requested results.

## The model is not the agent

IASI deliberately separates these concepts.

**Ollama**, for example, can expose and execute local language models.

It is a model runtime.

It is not, by itself, an IASI agent.

```text
Ollama
   ↓
model
```

`iasi-agent` adds the execution layer around that model:

```text
Ollama / model
      ↓
  iasi-agent
      ↓
   workspace
```

This distinction allows models and model runtimes to evolve independently from the engineering process.

## The agent is not the workflow

An IASI task describes **what must be done**.

It does not need to prescribe **who must execute it**.

```text
TASK
 │
 ├── Codex
 ├── iasi-agent
 ├── another agent
 └── future executor
```

The executor may change without changing the task.

A failure, quota limit, unavailable cloud service or change of model should not redefine the engineering work.

The workflow belongs to IASI.

The agent executes work within that workflow.

## Workspace access

`iasi-agent` is not intended to behave merely as a chat interface over a model.

It works with a project.

That means the workspace is part of its execution context.

The agent may need to:

- discover the project structure;
- read inputs and engineering artifacts;
- understand the task being executed;
- inspect existing outputs when relevant;
- create or modify artifacts where the task permits it;
- use available tools;
- validate the result of its work.

Access does not imply unrestricted mutation.

The rules of the IASI project determine what the agent may read, create or modify.

For example, immutable artifacts remain immutable regardless of which agent performs the work.

## Local execution

One of the purposes of `iasi-agent` is to make local execution a first-class possibility.

A locally available model can therefore become an engineering executor when combined with the agent capabilities required to operate on the workspace.

Conceptually:

```text
LOCAL MODEL
    │
    ▼
IASI AGENT
    │
    ▼
WORKSPACE
```

The objective is not to reproduce a particular cloud agent.

The objective is to provide the capabilities required to execute IASI work.

## Replaceable executors

IASI must not depend on one specific agent.

`iasi-agent` is one possible executor in a wider execution model.

```text
                     IASI TASK
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
       Codex       iasi-agent       other
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                     RESULT
```

This makes execution replaceable while preserving the engineering intent.

The task remains the task.

The project remains the project.

The agent is an executor, not the workflow.
