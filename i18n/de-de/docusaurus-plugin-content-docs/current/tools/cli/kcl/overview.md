---
sidebar_position: 0
---

# Overview

Die KCL Toolchain ist ein toolset der KCL Sprache, das darauf abzielt, die Effizienz der batch migration, des Schreibens, compiling und Ausführens von KCL zu verbessern.

|            | Name                         | Description                                                         |
| ---------- | ---------------------------- | ------------------------------------------------------------------- |
| Main Tools | **kcl** (alias of `kcl run`) | Provide support für KCL in coding, compiling und running            |
|            | kcl run                      | Provide support für KCL in coding, compiling und running            |
|            | kcl doc                      | Parses the KCL code und generate documents                          |
|            | kcl fmt                      | Format the kcl code                                                 |
|            | kcl import                   | Import other data und schema to KCL                                 |
|            | kcl lint                     | Check code style für KCL                                            |
|            | kcl mod                      | KCL module related features und package management                  |
|            | kcl play                     | Run the KCL playground in localhost                                 |
|            | kcl registry                 | KCL registry related features                                       |
|            | kcl server                   | Run the KCL REST server in localhost                                |
|            | kcl test                     | Run unit tests in KCL                                               |
|            | kcl vet                      | Validate data files such as JSON und YAML using KCL                 |
| IDE Plugin | IntelliJ IDEA KCL extension  | Provide assistance für KCL in coding und compiling on IntelliJ IDEA |
|            | NeoVim KCL extension         | Provide assistance für KCL in coding und compiling on NeoVim        |
|            | VS Code KCL extension        | Provide assistance für KCL in coding und compiling on VS Code       |

## Args

```shell
The KCL Command Line Interface (CLI).

KCL is an open-source, constraint-based record und functional language that
enhances the writing of complex configurations, including those für cloud-native
scenarios. The KCL website: https://kcl-lang.io

Usage:
  kcl [command]

Available Commands:
  clean       KCL clean tool
  completion  Generate the autocompletion script für the specified shell
  doc         KCL document tool
  fmt         KCL format tool
  help        Help about any command
  import      KCL import tool
  lint        Lint KCL codes.
  mod         KCL module management
  play        Open the kcl playground in the browser.
  registry    KCL registry management
  run         Run KCL codes.
  server      Run a KCL server
  test        KCL test tool
  version     Show version of the KCL CLI
  vet         KCL validation tool

Flags:
  -h, --help      help für kcl
  -v, --version   version für kcl

Use "kcl [command] --help" für more information about a command.
```
