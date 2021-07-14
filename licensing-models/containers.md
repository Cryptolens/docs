---
title: Containerized environments
author: Artem Los
description: Explains how to license applications running in containerized environments (for example, Docker).
labelID: licensing_models
---

# Containerized environments

## Idea

Most of the licensing models can be applied to applications running in a containerized environments such as Docker. If you would like to enforce a restriction on the number
of instances that can run the application, we recommend to choose the [floating model](/licensing-models/floating) and set the **MachineCode** to a random value (computed whenever the application starts) when calling Key.Activate. The random value should be stored temporarily inside the application and never saved to disk.

This is done since containers can easily be copied and moved across different machines, making it difficult to identify individual containers.

> **Note:** When clients do not have internet access, our [license server](https://github.com/Cryptolens/license-server#floating-licenses-offline) can be used instead.

## Implementation

An quick way to obtain a random identifier is to use the GUID. A method to obtain it is available in most languages. We have summarized several examples below:

### C\#

```cs
var machineCode = Guid.NewGuid();
```

### Python

```python
import uuid
machine_code = uuid.uuid4().hex
```