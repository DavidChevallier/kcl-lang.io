---
slug: 2024-04-17-newsletter
title: KCL Newsletter (2024.04.04 - 2024.04.17)
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Newsletter]
image: /img/biweekly-newsletter.png
---

![](/img/biweekly-newsletter.png)

[KCL](https://github.com/kcl-lang) ist eine auf Einschränkungen basierende rekursive und funktionale Sprache, die von der Cloud Native Computing Foundation (CNCF) gehostet wird und die Erstellung komplexer Konfigurationen und Richtlinien, einschließlich solcher für Cloud-native Szenarien, erleichtert. Mit ihrer fortschrittlichen Programmiersprachentechnologie und -praxis ist KCL darauf ausgerichtet, eine bessere Modularität, Skalierbarkeit und Stabilität für Konfigurationen zu fördern. Sie ermöglicht einfacheres Schreiben von Logik und bietet eine einfache Automatisierung von APIs sowie Integration mit hausinternen Systemen.

Dieser Abschnitt wird die neuesten Nachrichten der KCL language community's aktualisieren, einschließlich Funktionen, Website Updates und den neuesten community nachrichten, um allen ein besseres Verständnis der KCL Community zu ermöglichen!

**_KCL Website: [https://kcl-lang.io](https://kcl-lang.io)_**

## Overview

Vielen Dank an alle Beitragenden für ihre herausragende Arbeit in den letzten zwanzig Tagen (04.04.2024 - 17.04.2024). Hier ist eine Übersicht über die wichtigsten Inhalte.

**🏄 Language Updates**

- Die `instances()` Methode des Schemas hat einen neuen Schlüsselwortparameter `full_pkg` hinzugefügt, um alle Instanzen zu lesen, die dem Schema im Code entsprechen.

```python
schema Person:
    name: str

alice = Person {name = "Alice"}
all_persons = Person.instances(True)
```

- Das Modul `template` wurde hinzugefügt, um templates in KCL zu manipulieren.

```python
import template

content = template.execute("""\
<div class="entry">
{{#if author}}
<h1>{{firstName}} {{lastName}}</h1>
{{/if}}
</div>
""", {
  author: True,
  firstName: "Yehuda",
  lastName: "Katz",
})
```

**⛵️ Toolchain Updates**

- Die OverrideFile API unterstützt das Ändern/Löschen von non-schema type Feldern.
- Neue ListVariable API zum Lesen der values von Variablen in KCL files.

**🔥 SDK Updates**

- The KCL Go SDK has been released in version 0.8.4.
- The KCL Rust SDK has added the `llvm` feature, allowing to choose whether to use the LLVM, defaulting to off. When the LLVM feature is turned off, binary size can be reduced by 90%. Dependencies can be added in the following way:

```shell
cargo add --git https://github.com/kcl-lang/lib
```

- The initial release of the KCL Node.js SDK. Repository link: [https://github.com/kcl-lang/lib/tree/main/nodejs](https://github.com/kcl-lang/lib/tree/main/nodejs). Contributions are welcome.

* `__test__/test_data/schema.k`

```python
schema AppConfig:
    replicas: int

app: AppConfig {
    replicas: 2
}
```

- `execProgram`

```ts
import { execProgram, ExecProgramArgs } from "kcl-lib";

function main() {
  const result = execProgram(new ExecProgramArgs(["__test__/test_data/schema.k"]));
  console.log(result.yamlResult);  // 'app:\n  replicas: 2'
}

main();
```

- `listVariables`

```ts
import { listVariables, ListVariablesArgs } from "kcl-lib";

function main() {
  const result = listVariables(
    ListVariablesArgs("__test__/test_data/schema.k", []),
  );
  console.log(result.variables["app"].value); // 'AppConfig {replicas: 2}'
}

main();
```

- `loadPackage`

```ts
import { loadPackage, LoadPackageArgs } from "kcl-lib";

function main() {
  const result = loadPackage(
    LoadPackageArgs(["__test__/test_data/schema.k"], [], true),
  );
  console.log(result.symbols);
}

main();
```

**💻 IDE Updates**

- Added compilation unit caching to enhance IDE performance.

**🌼 Integration Updates**

- Crossplane KCL Function supports reading Function Context parameters für passing parameters to different functions.
  - Support reading the function context across different functions.
  - Support reading the function details to set sensitive information.
  - Support setting the status field of XR to output user information.
  - Fixed errors related to concurrent requests when issuing clusters under multiple XR resources.
- KCL has released a Nix package, allowing installation of the KCL command line tools with `nix-shell` or `devbox shell`.

## Special Thanks

We'd like to thank all the community participants from the past two weeks.

The following are listed in no particular order:

- Vielen Dank an @bozaro für seinen Beitrag zum KCL Go SDK 🙌
- Vielen Dank an @jheyduk für seinen Beitrag zum Kubectl KCL plugin 🙌
- Vielen Dank an @shashank-iitbhu für seinen Beitrag zum quick fix feature für KCL IDE syntax 🙌
- Vielen Dank an @d4v1d03 für seinen Beitrag zum KCL official website FAQ documentation 🙌
- Vielen Dank an @octonawish-akcodes für seinen Beitrag zum automatic dependency update feature für KCL IDE based on kcl.mod 🙌
- Vielen Dank an @utnim2 für seinen Beitrag zum restart kcl-language-server command für KCL IDE 🙌
- Vielen Dank an @AkashKumar7902 für seinen Beitrag zum KCL package management tool MVS algorithm 🙌
- Vielen Dank an @steeling, @bozaro, @vtomilov, @sanzoghenzo, @folliehiyuki, @markphillips100, @wilsonwang371, @zargor, @aleeriz, @reckless-huang, @zhuxw, @jheyduk, @Vitaly Tomilov, @Sergey Ryabin, @Stephen C, @ytsarev und others für their valuable suggestions und feedback while using KCL recently. 🙌

## Resources

❤️ See [here](https://github.com/kcl-lang/community) to join us!

For more resources, bitte refer to

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.9.0 Milestone](https://github.com/kcl-lang/kcl/milestone/9)
