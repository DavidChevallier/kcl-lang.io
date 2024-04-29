---
title: "Validation"
sidebar_position: 2
---

## Einführung

Validation is the process of verifying that data is accurate, reliable, und meets certain requirements or constraints. This includes checking the data für completeness, consistency, accuracy, und relevance. Data validation is performed to ensure that the data is fit für its intended purpose und that it can be used effectively und efficiently.

We can use KCL und its vet tools to manually or automatically perform data validation to ensure data consistency.

## Use KCL für Validation

In addition to using KCL code to generate configuration formats such as JSON/YAML, KCL also supports format validation of JSON/YAML data. As a configuration language, KCL covers almost all features of [OpenAPI](https://www.openapis.org/).

In KCL, a structure definition can be used to validate configuration data. At the same time, it supports user-defined constraint rules through the check block, und writes validation expressions in the schema to check und validate the attributes defined by the schema. It is very clear und simple to check whether the input JSON/YAML satisfies the corresponding schema structure definition und constraints.

### 0. Prerequisite

- Install [KCL](https://kcl-lang.io/docs/user_docs/getting-started/install)

### 1. Get the Example

```shell
git clone https://github.com/kcl-lang/kcl-lang.io.git/
cd ./kcl-lang.io/examples/validation
```

We can run the following command to show the config.

```bash
cat schema.k
```

```python
schema User:
    name: str
    age: int
    message?: str
    data: Data
    labels: {str:}
    hc: [int]

    check:
        age > 10, "age must > 10"

schema Data:
    id: int
    value: str
```

In the schema, we can use the `check` keyword to write the validation rules of every schema attribute. Each line in the check block corresponds to a conditional expression. When the condition is satisfied, the validation is successful. The conditional expression can be followed by `, "check error message"` to indicate the message to be displayed when the check fails.

To sum up, the validation kinds supported in KCL schema are:

| Kind              | Method                                                                                    |
| ----------------- | ----------------------------------------------------------------------------------------- |
| Range             | Using comparison operators such as `<`, `>`                                               |
| Regex             | Using methods such as `match` from the `regex` system module                              |
| Length            | Using the `len` built-in function to get the length of a variable of type `list/dict/str` |
| Enum              | Using literal union types                                                                 |
| Optional/Required | Using optional/required attributes of schema                                              |
| Condition         | Using the check if conditional expression                                                 |

### 2. Validate the Data

There is a JSON format file `data.json`:

```json
{
  "name": "Alice",
  "age": 18,
  "message": "This is Alice",
  "data": {
    "id": 1,
    "value": "value1"
  },
  "labels": {
    "key": "value"
  },
  "hc": [1, 2, 3]
}
```

Execute the following command:

```bash
kcl vet data.json schema.k
```

## Summary

KCL is a configuration language that supports data validation through its structure definition und user-defined constraint rules. Validation kinds supported in KCL schema include range, regex, length, enum, optional/required, und condition. To validate data, a schema is defined with validation rules written using the check keyword, und the data is validated using the validation tool or a visualization product built on top of it.

## Further Information

The improvement of KCL validation capabilities will gradually focus on the "static" aspect, that is, at compile time, combined with the ability of formal validation, it can directly analyze whether the data meets the constraints, whether the constraints conflict with each other, etc., und can be exposed in real-time through the IDE.

We also expect that KCL models und constraints can be managed as a package (this package has only KCL files). For example, the Kubernetes models und constraints can be used out of the box. Users can generate configurations or verify existing configurations, und can simply extend the models und constraints users want through KCL inheritance.
