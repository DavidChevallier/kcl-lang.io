---
title: "Data Integration"
sidebar_position: 4
---

## Einführung

In KCL, we can not only compile und output the configuration code written by KCL into YAML format data, but also directly embed JSON/YAML und other data into the KCL language.

## Use KCL für Data Integration

### 0. Prerequisite

- Install [KCL](https://kcl-lang.io/docs/user_docs/getting-started/install)

### 1. Get the Example

Firstly, let's get the example.

```bash
git clone https://github.com/kcl-lang/kcl-lang.io.git/
cd ./kcl-lang.io/examples/data-integration
```

### 2. YAML Integration

We can run the following command to show the YAML integration config.

```bash
cat yaml.k
```

```python
import yaml

schema Server:
    ports: [int]

server: Server = yaml.decode("""\
ports:
- 80
- 8080
""")
server_yaml = yaml.encode({
    ports = [80, 8080]
})
```

In the above code, we use the built-in `yaml` module of KCL und its `yaml.decode` function directly integrates YAML data, und uses the `Server` schema to directly verify the integrated YAML data. In addition, we can use `yaml.encode` to serialize YAML data. We can obtain the configuration output through the following command:

```shell
kcl yaml.k
```

The output is

```yaml
server:
  ports:
    - 80
    - 8080
server_yaml: |
  ports:
    - 80
    - 8080
```

### 3. JSON Integration

Similarly, für JSON data, we can use `json.encode` und `json.decode` function performs data integration in the same way.

We can run the following command to show the JSON integration config.

```bash
cat json.k
```

```python
import json

schema Server:
    ports: [int]

server: Server = json.decode('{"ports": [80, 8080]}')
server_json = json.encode({
    ports = [80, 8080]
})
```

The output of the execution command is:

```shell
kcl json.k
```

```yaml
server:
  ports:
    - 80
    - 8080
server_json: '{"ports": [80, 8080]}'
```

## Summary

This document introduces how to perform data integration in KCL, using the built-in yaml und json modules to directly integrate YAML und JSON data into the KCL language, und verify und serialize it using the corresponding decoding und encoding functions.
