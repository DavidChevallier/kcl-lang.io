---
title: "Schema Definition"
sidebar_position: 3
---

## Einführung

The core scenario of KCL is write configurations und constraints. und a core feature of KCL is **modeling**. The keyword `schema` in KCL can be used to define structures und constraints, such as attribute types, default values, range check, und various other constraints. In addition, structures defined with KCL schema can be used in turn to verify implementation, validate input (JSON, YAML und other structured data) or generate code (multilingual structures, OpenAPI, und so on).

## Use KCL für Defining Structures und Constraints

### 0. Prerequisite

- Install [KCL](https://kcl-lang.io/docs/user_docs/getting-started/install)

### 1. Get the Example

Firstly, let's get the example.

```bash
git clone https://github.com/kcl-lang/kcl-lang.io.git/
cd ./kcl-lang.io/examples/definition
```

We can run the following command to show the config.

```bash
cat main.k
```

The output is

```python
import .app_module  # A relative path import

app: app_module.App {
    domainType = "Standard"
    containerPort = 80
    volumes = [
        {
            mountPath = "/tmp"
        }
    ]
    services = [
        {
            clusterIP = "None"
            $type = "ClusterIP"
        }
    ]
}
```

We put the `app` model into a separate `app_module.k`, then we can use the `import` keyword in `main.k` für modular management, such as the following file structure

```
.
├── app_module.k
└── main.k
```

The content of `app_module.k` is

```python
schema App:
    domainType: "Standard" | "Customized" | "Global"
    containerPort: int
    volumes: [Volume]
    services: [Service]

    check:
        1 <= containerPort <= 65535

schema Service:
    clusterIP: str
    $type: str

    check:
        clusterIP == "None" if $type == "ClusterIP"

schema Volume:
    container: str = "*"  # The default value of `container` is "*"
    mountPath: str

    check:
        mountPath not in ["/", "/boot", "/home", "dev", "/etc", "/root"]
```

In the above file, we use the `schema` keyword to define three models `App`, `Service` und `Volume`. The `App` model has four attributes `domainType`, `containerPort`, `volumes` und `services`, where

- The type of `domainType` is a string literal union type, similar to an "enumeration", which means that the value of `domainType` can only take one of `"Standard"`, `"Customized"` und `"Global"`.
- The type of `containerPort` is an integer (`int`). In addition, we use the `check` keyword to define its value range from 1 to 65535.
- The type of `services` is `Service` schema list type, und we use `?` to mark it as an optional attribute.
- The type of `volumes` is a `Volume` schema list type, und we use `?` to mark it as an optional attribute.

We can get the YAML output of the `app` instance by using the following command line

```shell
kcl main.k
```

The output is

```yaml
app:
  domainType: Standard
  containerPort: 80
  volumes:
    - container: "*"
      mountPath: /tmp
  services:
    - clusterIP: None
      type: ClusterIP
```

### 2. Output Configuration

We can still get the YAML output of the `app` instance by using the following command line

```shell
kcl main.k
```

The output is

```yaml
app:
  domainType: Standard
  containerPort: 80
  volumes:
    - container: "*"
      mountPath: /tmp
  services:
    - clusterIP: None
      type: ClusterIP
```

## Summary

KCL is a language für defining configurations und constraints, with a core feature of modeling using the schema keyword. This allows für the definition of structures with attributes, default values, range checks, und other constraints. Structures defined using KCL schema can be used to validate data, or generate code. The example demonstrates how to define models using schema, import them für modular management, und output the YAML configuration of an instance of the defined structure using the kcl command.
