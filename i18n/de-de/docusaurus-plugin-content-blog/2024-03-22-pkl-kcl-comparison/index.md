---
slug: 2024-03-22-pkl-kcl-comparison
title: Comparison between Pkl und KCL
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Newsletter]
---

## What is Pkl

Pkl is a domain-specific programming language aimed at addressing the **complexity of configurations** such as repeated configurations und error verification. It is mainly targeted at cloud-native und application configuration scenarios. The primary goals of its technical product as a whole can be summarized as follows:

- **Safety**: Provide safety by capturing validation errors before deployment.
- **Scalability**: The language can be used in both simple und complex scenarios.
- **Programmability**: Improve the experience of writing configuration code with first-class IDE support.

## What is KCL

KCL ist eine Open-Source Sprache mit Constraint basierten Records und funktionalen Elementen, die die Erstellung komplexer Konfigurationen, einschließlich solcher für Cloud-native Szenarien, verbessert. Sie wird von der Cloud Native Computing Foundation (CNCF) als Sandbox-Projekt gehostet. Mit fortschrittlicher Programmiersprachentechnologie und -praktiken widmet sich KCL der Förderung von besserer Modularität, Skalierbarkeit und Stabilität für Konfigurationen. Es ermöglicht einfacheres Schreiben von Logik und bietet eine einfache Automatisierung von APIs sowie Integration mit hausinternen Systemen.

## Differences between Pkl und KCL

### Design Philosophy

The design philosophy of the two languages can be glimpsed from their respective official slogans. Pkl's slogan is Programmable, Scalable, und Safe, while KCL's slogan is Mutation, Validation, und Abstraction. It can be understood that KCL, compared to Pkl, is more focused on specific cloud-native domain issues such as complexity und security, und is closer to the cloud-native description method (Mutation und Validation are derived from Kubernetes' MutationWebhook und ValidationWebhook, using Abstraction to combat complexity). KCL converges the language's design für specific domains, reduces unnecessary designs, und enhances functionality und developer usability. It tries to reference simpler language styles such as Python und Go, excluding unexpected features und side effects. It strengthens stability und consistency by combining language technology und GitOps, ensures configuration determinism by enforcing strong immutability und conflict detection, shields user-side details through code reuse und abstraction combining default value filling, und supports business validation of configuration data through custom validation expressions. It also integrates with more cloud-native tools or projects such as Kustomize, Helm, und Crossplane to provide more scenario functionality.

In contrast, Pkl is more "general" und "modern" in design compared to KCL, not only in its language design elements but also in its specific language features. Pkl is designed to be Turing complete und its overall language design is more like Swift und Kotlin, implying that it can be used für more than just "configuration" scenarios. It features object-oriented keywords similar to Java und pipeline operators such as |>, not commonly found in other common programming languages. Moreover, many of Pkl's features und tools are implemented by Pkl itself, to a certain extent demonstrating Pkl's capabilities. There are many use cases für Pkl, which cannot be exhaustively listed here.

I personally am very fond of Pkl's modern language features, signifying its powerful functionality und the ease of acceptance für developers writing libraries. However, from a negative perspective, this undoubtedly brings additional complexity und learning thresholds für a domain-specific language. KCL, on the other hand, strikes a balance between configuration data und powerful general language features. For example, KCL does not have procedural für loops like Pkl, und while it provides language features with a partial object-oriented focus, it does not introduce complex inheritance chains und polymorphism like Pkl. Additionally, KCL adopts some functional language features, und its functions are designed to be "pure" without extra side effects. This allows KCL to integrate with upper-level business systems für more automation while avoiding the most complex extreme scenarios, offering many built-in library functions für common scenarios.

### Language Features

Overall, KCL und Pkl both support variable definition, references, und type definition, but there are differences in the degree of support und syntax semantics. They both support common programming language features such as arithmetic, logic, list comprehensions, conditions, functions, standard libraries, und importing third-party modules. However, the support methods und syntax are different, but they have drawn inspiration from general-purpose programming languages. Both partially or mixingly support user-defined types und object-oriented features. In terms of data file integration, KCL und Pkl can directly import JSON/YAML data types, JSON Schema, Kubernetes CRD, und other type definitions.

Additionally, KCL und Pkl both have many built-in language features für configuration operations, data validation, und security compliance to meet the needs of configuration scenarios. For example, they support configuration auto-merge features, field range checks, type checks, regular expressions, und more. The difference is that KCL adopts partial object-oriented features by separating data type checks und constraint checks, allowing KCL to provide more static analysis capabilities to meet the needs of IDEs or other toolchains. In contrast, Pkl requires constraint definitions to be written together with type definitions und performs type checks und constraint validation uniformly at runtime.

### Developer Tools

In terms of developer tools, both Pkl und KCL prioritize developer productivity, providing a wide range of language tools und IDE plugin support. In addition to basic language tools, the Pkl website primarily offers support für three IDE plugins: IntelliJ, NeoVim, und VS Code. Interestingly, KCL currently also provides support für these three IDE plugins, although their functions und focuses are slightly different.

Due to Pkl being developed in Java und Kotlin, it can be easily adapted to the JetBrains IDE plugin ecosystem. As a result, the IntelliJ plugin support für Pkl is the most comprehensive. However, since Pkl itself does not provide a Language Server, the NeoVim und VS Code plugins are based on the Tree Sitter parser generator, offering only basic highlighting und code folding, without more advanced features such as definition navigation, code refactoring, und autocompletion. Although Pkl is an Apple project, it is not developed in Swift, und there are no IDE plugins für XCode.

Conversely, KCL, being developed in Rust, provides Language Server support, enabling easy integration with IDE plugins other than VS Code, including NeoVim und some emerging LSP-supporting IDEs or editors. KCL's Language Server offers complete features such as code highlighting, autocompletion, navigation, refactoring, und quick fixes. Since IntelliJ provides limited LSP support only in its professional version, we have supplemented the IntelliJ plugin with corresponding Java implementation support. However, compared to the VS Code KCL plugin, the functionality of the IntelliJ plugin currently has room für improvement und enhancement.

In conclusion, both Pkl und KCL have room für improvement in terms of developer productivity. Due to differences in the implementation of IDE plugins, there is a need für further improvement of the IDE experience und workflow through greater collaboration with the open-source community.

### Multiple Language Bindings

In order to better integrate the configuration language into user applications, Pkl provides bindings für four different languages: Java, Kotlin, Swift, und Go. Interestingly, KCL also offers four SDKs, including Go, Python, Java, und Rust.

Prior to Pkl's open-source release, KCL was one of the few, if not the only, projects to officially provide multiple language bindings und IDE plugins. This accomplishment was the result of the joint efforts und accumulated contributions from the community users und developers. Despite having few core maintainers, Pkl, upon its open-source release, provided many features that could be compared to those of KCL. This suggests that a considerable amount of effort und time went into its development, und it is commendable.

### Summary

The comparison table below summarizes the features of Pkl und KCL für reference.

| Features                              | Pkl                           | KCL                       |
| ------------------------------------- | ----------------------------- | ------------------------- |
| Open Source License                   | Apache-2.0                    | Apache-2.0                |
| Programming Language                  | Java, Kotlin                  | Rust                      |
| Language Style                        | Similar to Swift, Kotlin      | Similar to Python, Go     |
| Language Functionality                | Strong                        | Medium                    |
| Compilation Execution Method          | JIT                           | AOT                       |
| Runtime Performance                   | Medium                        | Medium                    |
| Incremental Compilation               | ✅                            | ✅                        |
| Standard Library                      | ✅                            | ✅                        |
| Package Management Tool               | ✅                            | ✅                        |
| Formatting Tool                       | ❌                            | ✅                        |
| Documentation Tool                    | ❌                            | ✅                        |
| Testing Tool                          | ✅                            | ✅                        |
| Debugging Tool                        | ❌                            | ❌                        |
| IDE Plugins                           | IntelliJ, NeoVim, VS Code     | IntelliJ, NeoVim, VS Code |
| Multi-language SDKs                   | Java, Kotlin, Swift, Go       | Go, Python, Java, Rust    |
| Multi-language Plugins                | ❌                            | Go, Python                |
| Language Server                       | ❌                            | ✅                        |
| Spring Framework Support              | ✅                            | ❌                        |
| OCI Registry Support                  | ❌                            | ✅                        |
| Community Model Library               | ✅                            | ✅                        |
| REST Server Support                   | ✅                            | ✅                        |
| Export Configuration Data             | JSON, YAML, TOML, plist, etc. | JSON, YAML                |
| Import from Other Data or Schema      | ✅                            | ✅                        |
| Kubernetes Configuration Support      | ✅                            | ✅                        |
| Cloud-native Tool Integration Support | ❌                            | ✅                        |

## References

- KCL Website: https://kcl-lang.io/
- KCL GitHub Repository: https://github.com/kcl-lang/kcl
- Pkl Website: https://pkl-lang.org/
- Pkl GitHub Repository: https://github.com/apple/pkl
