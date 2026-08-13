---
title: Microsoft Foundry datasets extension overview
description: Learn about the Microsoft Foundry datasets extension, which lets you register and manage versioned Microsoft Foundry datasets from your terminal.
author: m7md7sien
ms.author: mohessie
ms.date: 08/13/2026
ms.service: azure-dev-cli
ms.topic: overview
ms.custom: devx-track-azdevcli, devx-track-ai
ai-usage: ai-generated
---

# Microsoft Foundry datasets extension overview

The Azure Developer CLI (`azd`) Microsoft Foundry datasets extension (`azure.ai.dataset`) registers and manages versioned datasets in a Microsoft Foundry project from your terminal or editor. A dataset is the set of inputs an evaluation runs against, so keeping it versioned is what makes two evaluation results comparable. The extension publishes a local file or folder as a new version, and lists, inspects and removes the versions already registered.

This article introduces the extension. For dataset concepts and how evaluations consume them, see the [Microsoft Foundry documentation](/azure/ai-foundry/).

> [!NOTE]
> `azd` extensions are currently in beta.

## Key features

| Feature | Description |
|---|---|
| Versioned publishing | Registers a local `.jsonl` file or folder as a dataset version, choosing the next version from what the project already carries. |
| Local validation | Refuses a malformed row, an empty dataset, a name the service will not accept, and a folder that could mean more than one dataset, before any request is sent. |
| Version history | Lists every version of a dataset, so you can see what an earlier evaluation ran against. |
| Content retrieval | Reads a dataset's contents back, whether the service hands out a file or the container holding it. |
| Scriptable output | Supports `-o json` on every command and `--no-prompt` for unattended use. |

## Explore the workflow

1. **Prepare**: Write or export the rows you want to evaluate against as a `.jsonl` file.
1. **Register**: Publish the file as the dataset's first version.
1. **Iterate**: Publish a new version when the data changes; earlier versions stay addressable.
1. **Inspect**: List versions and read back what a given version contains.
1. **Clean up**: Delete versions you no longer need.

## Try it

1. Install the extension:

    ```azdeveloper
    azd extension install azure.ai.dataset
    ```

1. Register a local file as a dataset:

    ```azdeveloper
    azd ai dataset create my-dataset --from-file ./data/rows.jsonl
    ```

1. Publish a new version after the data changes:

    ```azdeveloper
    azd ai dataset update my-dataset --from-file ./data/rows.jsonl
    ```

    > [!TIP]
    > A dataset takes a moment to become listable after it is registered. If
    > `update` reports that the dataset does not exist immediately after
    > `create`, run it again.

1. See what is registered, and what versions a dataset has:

    ```azdeveloper
    azd ai dataset list
    ```

    ```azdeveloper
    azd ai dataset versions list my-dataset
    ```

1. Remove a version when you no longer need it:

    ```azdeveloper
    azd ai dataset delete my-dataset --version 1.0
    ```

    > [!WARNING]
    > Deleting a version removes it from the project. An evaluation pinned to that version can no longer run.

## When to use the datasets extension

The `azd` datasets extension is the best fit when you want to:

- **Work primarily from the terminal or editor**, with a scriptable, repeatable workflow.
- **Keep evaluation data under version control** alongside the app it tests, and publish it the same way each time.
- **Publish from CI/CD** without a portal step.

The [Microsoft Foundry evaluations extension](foundry-evaluations-extension.md) can also declare datasets as part of an evaluation configuration and reconcile them for you. Use this extension when you want to manage datasets on their own, and that one when the dataset is one piece of a larger evaluation you keep in a config file.

## Related content

- [Microsoft Foundry evaluations extension overview](foundry-evaluations-extension.md)
- [Extensions overview](overview.md)
- [Azure Developer CLI documentation](/azure/developer/azure-developer-cli/)
- [Microsoft Foundry documentation](/azure/ai-foundry/)
