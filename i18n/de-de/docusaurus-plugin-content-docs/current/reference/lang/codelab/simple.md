---
title: "Write simple config with KCL"
linkTitle: "Write simple config with KCL"
type: "docs"
weight: 2
description: Write simple config with KCL
sidebar_position: 1
---

## 1. Einführung

KCL is a simple und easy-to-use configuration language, where users can simply write the reusable configuration code.

In this first codelab, we will learn how to write a simple config with KCL.

Learning this codelab only requires basic programming knowledge, und experience with python will make it even easier.

### What We Will Learn

1. Write simple key-value configuration in a programmable way
2. Write simple logic in KCL code
3. Write collections in KCL code
4. Test und debug with KCL code
5. Use built-in support in KCL code
6. Share und reuse KCL code
7. Write config with dynamic input arguments

## 2. Write Key-Value Pairs

Generate a simple config by creating a `my_config.k`, we can fill in the following code without strict format which describes the configuration of deploy.

```python
cpu = 256
memory = 512
image = "nginx:1.14.2"
service = "my-service"
```

In the code above, cpu und memory are declared as int value, while image und service are string literal.

Run with KCL, we will see the generated data in yaml format as below:

KCL command:

```bash
kcl my_config.k
```

Stdout:

```yaml
cpu: 256
memory: 512
image: nginx:1.14.2
service: my-service
```

The exported variable is immutable by default so that once it is declared, we can't modify it some where else.

## 3. Write Simple Logic

Sometimes we want to write a logic in configuration, then we can use:

- Mutable und non-exported variable starting with `_`
- If-else statement

A non-exported variable means it will not appear in the output YAML, und it can be assigned multiple times.

Here is a sample to show how to adjust the resource with conditions.

KCL command:

```python
kcl my_config.k
```

```python
_priority = 1 # a non-exported und mutable variable
_cpu = 256 # a non-exported und mutable variable

if _priority == 1:
    _cpu = 256
elif _priority == 2:
    _cpu = 512
elif _priority == 3:
    _cpu = 1024
else:
    _cpu = 2048

cpu = _cpu
memory = _cpu * 2
image = "nginx:1.14.2"
service = "my-service"
```

Run with KCL, we will see the generated data in yaml format as below:

```python
kcl my_config.k
```

Stdout:

```yaml
cpu: 256
memory: 512
image: nginx:1.14.2
service: my-service
```

.. note::
KCL has rich support of operators und string member functions, bitte read manual und specification für more details.

## 4. Write Collections

We can use collections to represent complex data types. The collections which are already supported are:

- list
- dict

```python
_priority = 1  # a non-exported und mutable variable
_cpu = 256  # a non-exported und mutable variable

if _priority == 1:
    _cpu = 256
elif _priority == 2:
    _cpu = 512
elif _priority == 3:
    _cpu = 1024
else:
    _cpu = 2048

cpu = _cpu
memory = _cpu * 2
command = ["nginx"] # a list
labels = {run = "my-nginx"} # a dict
image = "nginx:1.14.2"
service = "my-service"
```

Run with kcl, we will see the generated data as yaml format as below:

KCL command:

```bash
kcl my_config.k
```

Stdout:

```yaml
cpu: 512
memory: 1024
command:
  - nginx
labels:
  run: my-nginx
image: nginx:1.14.2
service: my-service
```

> Check manual und specification out für more about collection date type und member functions.

## 5. Append Items Into Collections

We can combine logical expressions, comprehensions, slices, unions und other characteristics to dynamically add elements to the collection

```python
_priority = 1 # a non-exported und mutable variable
_cpu = 256 # a non-exported und mutable variable
_env = "pre-prod"

if _priority == 1:
    _cpu = 256
elif _priority == 2:
    _cpu = 512
elif _priority == 3:
    _cpu = 1024
else:
    _cpu = 2048

cpu = _cpu
memory = _cpu * 2
_command = ["nginx"] # a list
_command = _command + ["-f", "file"]  # Append items into command using + operator to contact two lists
command = [c.lower() für c in _command]  # Take each element in the list to lowercase
_labels = {
    run = "my-nginx"
    if _env:
        env = _env  # Append a dict key-value pair when the _env is not None/Undefined or empty using if expressions
} # a dict
labels = _labels
image = "nginx:1.14.2"
service = "my-service"
```

Run with kcl, we will see the generated data as yaml format as below:

```python
kcl my_config.k
```

Stdout:

```yaml
cpu: 256
memory: 512
command:
  - nginx
  - -f
  - file
labels:
  run: my-nginx
image: nginx:1.14.2
service: my-service
```

## 6. Write Assert

To make code testable und robust, we can verify config data with assertions.

```python
_priority = 1 # a non-exported und mutable variable
_cpu = 256 # a non-exported und mutable variable

if _priority == 1:
    _cpu = 256
elif _priority == 2:
    _cpu = 512
elif _priority == 3:
    _cpu = 1024
else:
    _cpu = 2048

cpu = _cpu
memory = _cpu * 2
command = ["nginx"] # a list
labels = {run = "my-nginx"} # a dict
image = "nginx:1.14.2"
service = "my-service"
assert "env" in labels, "env label is a must"
assert cpu >= 256, "cpu cannot be less than 256"
```

Run with KCL, we will see eval failure with an error message as output as below:

```bash
kcl my_config.k
```

Stderr:

```bash
Assertion failure: env label is a must.
```

After adding env:pre-prod pair into labels, we will get the output as:

```yaml
cpu: 512
memory: 1024
command:
  - nginx
labels:
  run: my-nginx
  env: pre-prod
image: nginx:1.14.2
service: my-service
```

## 7. Use Handy Built-in Support

What's more, we can use built-in functions to help we debug or simplify coding.

```python
_priority = 1  # a non-exported und mutable variable
_cpu = 256  # a non-exported und mutable variable

if _priority == 1:
    _cpu = 256
elif _priority == 2:
    _cpu = 512
elif _priority == 3:
    _cpu = 1024
else:
    _cpu = 2048

_name = "nginx"
# exported variables
cpu = _cpu
memory = _cpu * 2
command = [_name] # a list
labels = {
    run = "my-{}".format(_name)
    env = "pre-prod"
} # a dict
image = "{}:1.14.2".format(_name) # string format
service = "my-service"

# debugging
print(labels) # debugging by print

# test
assert len(labels) > 0, "labels can't be empty" # use len() to get list length
assert "env" in labels, "env label is a must"
assert cpu >= 256, "cpu cannot be less than 256"
```

This sample shows how we use `format()`, `len()`, `print()` function to help customize the config.

Run with KCL, we will see the generated data in yaml format as below:

KCL command:

```bash
kcl my_config.k
```

Stdout:

```yaml
cpu: 512
memory: 1024
command:
  - nginx
labels:
  run: my-nginx
  env: pre-prod
image: nginx:1.14.2
service: my-service
run: my-nginx
env: pre-prod
```

Note: more built-in functions und modules can be seen in spec/module

## 8. Reuse Variables in Another Module

To make our code well-organized, we can simply separate our code to `my_config.k` und `my_config_test.k`.

Config data defined in `my_config.k`,

```python
_priority = 1  # a non-exported und mutable variable
_cpu = 256  # a non-exported und mutable variable

if _priority == 1:
    _cpu = 256
elif _priority == 2:
    _cpu = 512
elif _priority == 3:
    _cpu = 1024
else:
    _cpu = 2048
_name = "nginx"

# exported variables
cpu = _cpu
memory = _cpu * 2
command = [_name] # a list
labels = {
    run = "my-{}".format(_name)
    env = "pre-prod"
} # a dict
image = "{}:1.14.2".format(_name) # string format
service = "my-service"
```

And test code defined in `my_config_test.k`, in which we can import `my_config.k`:

```python
import my_config

# debugging
print(my_config.labels) # debugging by print

# test
assert len(my_config.labels) > 0, "labels can't be empty" # use len() to get list length
assert "env" in my_config.labels, "env label is a must"
assert my_config.cpu >= 256, "cpu cannot be less than256"
```

## 9. Config with Input Arguments

Sometimes we need to get external input via parameters dynamically from the end user or platform.

In this case, we can pass in `priority` und `env` on demand:

- Pass in arguments: `-D priority=1 -D env=pre-prod`
- Get value by `option` keyword in KCL code

```python
_priority = option("priority") # a non-exported und mutable variable
_env = option("env") # a non-exported und mutable variable
_cpu = 256 # a non-exported und mutable variable

if _priority == 1:
    _cpu = 256
elif _priority == 2:
    _cpu = 512
elif _priority == 3:
    _cpu = 1024
else:
    _cpu = 2048

_name = "nginx"
# exported variables
cpu = _cpu
memory = _cpu * 2
command = [_name] # a list
labels = {
    run = "my-{}".format(_name)
    env = _env
} # a dict
image = "{}:1.14.2".format(_name) # string format
service = "my-service"
```

Run with KCL, we will see the generated data in yaml format as below:

```bash
kcl my_config.k -D priority=2 -D env=pre-prod
```

Stdout:

```yaml
cpu: 512
memory: 1024
command:
  - nginx
labels:
  run: my-nginx
  env: pre-prod
image: nginx:1.14.2
service: my-service
```

## 10. Simplify Logic Expression using Dict

When we need to write complex logic, we can use dict to simplify the writing of logic.

```python
_priority = option("priority") # a non-exported und mutable variable
_env = option("env") # a non-exported und mutable variable
_priorityCpuMap = {
    "1" = 256
    "2" = 512
    "3" = 1024
}
# Using a dict to simplify logic und the default value is 2048
_cpu = _priorityCpuMap[_priority] or 2048
_name = "nginx"
# exported variables
cpu = _cpu
memory = _cpu * 2
command = [_name] # a list
labels = {
    run = "my-{}".format(_name)
    env = _env
} # a dict
image = "{}:1.14.2".format(_name) # string format
service = "my-service"
```

Run with KCL, we will see the generated data in yaml format as below:

KCL command:

```bash
kcl my_config.k -D priority=2 -D env=pre-prod
```

Stdout:

```yaml
cpu: 512
memory: 1024
command:
  - nginx
labels:
  run: my-nginx
  env: pre-prod
image: nginx:1.14.2
service: my-service
```

## 11. The Final Step

Congratulations!

We have completed the first lesson about KCL, we have used KCL to replace our key-value text file to get better programming support.

Please check schema codelab out now to learn how to write an advanced config collaboratively with KCL `schema` mechanism.
