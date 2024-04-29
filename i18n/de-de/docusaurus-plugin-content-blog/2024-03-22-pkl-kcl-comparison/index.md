---
slug: 2024-03-22-pkl-kcl-comparison
title: Vergleich zwischen Pkl und KCL
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Newsletter]
---

## What is Pkl

Pkl ist eine domänenspezifische Programmiersprache, die darauf abzielt, **die Komplexität von Konfigurationen** wie wiederholte Konfigurationen und Fehlerprüfung anzugehen. Sie zielt hauptsächlich auf Cloud-native und Anwendungskonfigurationsszenarien ab. Die Hauptziele ihres technischen Produkts insgesamt lassen sich wie folgt zusammenfassen:

**Sicherheit**: Bietet Sicherheit, indem Validierungsfehler vor der Bereitstellung erfasst werden.
**Skalierbarkeit**: Die Sprache kann in einfachen und komplexen Szenarien verwendet werden.
**Programmierbarkeit**: Verbessert die Erfahrung beim Schreiben von Konfigurationscode durch eine first-class IDE support.

## What is KCL

KCL ist eine Open Source Sprache mit constraints-basierten Records und funktionalen Elementen, die die Erstellung komplexer Konfigurationen, einschließlich solcher für Cloud-native Szenarien, verbessern. Sie wird von der Cloud Native Computing Foundation (CNCF) als Sandbox Projekt gehostet. Mit fortschrittlicher Programmiersprachentechnologie und -praktiken widmet sich KCL der Förderung von besserer Modularität, Skalierbarkeit und Stabilität für Konfigurationen. Es ermöglicht einfacheres Schreiben von Logiken und bietet eine einfache Automatisierung von APIs sowie Integration mit hausinternen Systemen.

## Die Unterschiede zwischen Pkl und KCL

### Design Philosophy

Die Designphilosophie der beiden Sprachen wird durch ihre offiziellen Slogans verdeutlicht. Der Slogan von Pkl lautet "Programmierbar, Skalierbar und Sicher", während KCL mit "Mutation, Validierung und Abstraktion" wirbt. Es lässt sich verstehen, dass KCL im Vergleich zu Pkl mehr auf spezifische Cloud-native Domänenfragen wie Komplexität und Sicherheit fokussiert ist und der cloud-nativen Beschreibungsmethode näher steht (Mutation und Validierung leiten sich von Kubernetes' MutationWebhook und ValidationWebhook ab und verwenden Abstraktion, um Komplexität entgegenzuwirken). KCL vereint das Design der Sprache für spezifische Domänen, reduziert unnötige Designs und verbessert Funktionalität und Entwicklerbenutzerfreundlichkeit. Es versucht, einfachere Sprachstile wie Python und Go zu referenzieren, indem unerwartete Funktionen und Seiteneffekte ausgeschlossen werden. Es stärkt Stabilität und Konsistenz durch die Kombination von Sprachtechnologie und GitOps, gewährleistet Konfigurationsdeterminismus durch die Durchsetzung starker Unveränderlichkeit und Konflikterkennung, schirmt benutzerseitige Details durch Code-Wiederverwendung und Abstraktion unter Verwendung von Standardwertfüllung ab und unterstützt die geschäftliche Validierung von Konfigurationsdaten durch benutzerdefinierte Validierungsausdrücke. Es integriert sich auch mit mehr Cloud-native Tools oder Projekten wie Kustomize, Helm und Crossplane, um mehr Szenariofunktionalität bereitzustellen.

Im Gegensatz dazu ist Pkl im Design im Vergleich zu KCL eher "allgemein" und "modern", nicht nur in seinen Sprachelementen, sondern auch in seinen spezifischen Sprachmerkmalen. Pkl ist darauf ausgelegt, Turing-vollständig zu sein, und sein gesamtes Sprachdesign ähnelt eher Swift und Kotlin, was darauf hindeutet, dass es für mehr als nur "Konfigurations" -Szenarien verwendet werden kann. Es verfügt über objektorientierte Schlüsselwörter ähnlich wie Java und Pipeline-Operatoren wie |> , die in anderen gängigen Programmiersprachen nicht häufig vorkommen. Darüber hinaus werden viele der Funktionen und Tools von Pkl selbst implementiert, was in gewissem Maße die Fähigkeiten von Pkl zeigt. Es gibt viele Anwendungsfälle für Pkl, die hier nicht erschöpfend aufgeführt werden können.

Ich persönlich bin sehr angetan von den modernen Sprachmerkmalen von Pkl, die seine leistungsstarke Funktionalität und die Akzeptanz für Entwickler, die Bibliotheken schreiben, kennzeichnen. Aus negativer Sicht bringt dies zweifellos zusätzliche Komplexität und Lernschwellen für eine domänenspezifische Sprache mit sich. KCL hingegen findet einen Kompromiss zwischen Konfigurationsdaten und leistungsstarken allgemeinen Sprachmerkmalen. Zum Beispiel verfügt KCL nicht über prozedurale for-Schleifen wie Pkl, und während es Sprachmerkmale mit einem teilweisen Schwerpunkt auf der objektorientierten Programmierung bietet, führt es keine komplexen Vererbungsketten und Polymorphismen wie Pkl ein. Darüber hinaus übernimmt KCL einige funktionale Sprachmerkmale, und seine Funktionen sind darauf ausgelegt, "rein" ohne zusätzliche Seiteneffekte zu sein. Dies ermöglicht es KCL, sich mit höherstufigen Geschäftssystemen für eine umfangreichere Automatisierung zu integrieren, während die komplexesten Extremszenarien vermieden werden und viele integrierte Bibliotheksfunktionen für häufige Szenarien angeboten werden.

### Sprachmerkmale

Insgesamt unterstützen KCL und Pkl sowohl die Definition von Variablen, Referenzen und Typen als auch Unterschiede im Grad der Unterstützung und der Syntaxsemantik. Beide unterstützen gängige Programmiersprachenfunktionen wie arithmetische, logische Operationen, Listenverständnis, Bedingungen, Funktionen, Standardbibliotheken und das Importieren von Drittanbietermodulen. Die Methoden und Syntax zur Unterstützung sind jedoch unterschiedlich, aber sie haben sich von allgemeinen Programmiersprachen inspirieren lassen. Beide unterstützen teilweise oder gemischt benutzerdefinierte Typen und objektorientierte Funktionen. In Bezug auf die Integration von Datendateien können KCL und Pkl direkt JSON/YAML-Datentypen, JSON-Schema, Kubernetes-CRD und andere Typdefinitionen importieren.

Darüber hinaus verfügen KCL und Pkl beide über viele integrierte Sprachfunktionen für Konfigurationsoperationen, Datenvalidierung und Sicherheitskonformität, um den Anforderungen von Konfigurationsszenarien gerecht zu werden. Beispielsweise unterstützen sie Konfigurationsautomergung, Feldbereichsprüfungen, Typüberprüfungen, reguläre Ausdrücke und mehr. Der Unterschied besteht darin, dass KCL teilweise objektorientierte Funktionen übernimmt, indem es Datentypüberprüfungen und Einschränkungsprüfungen trennt und somit KCL ermöglicht, mehr statische Analysefähigkeiten bereitzustellen, um den Anforderungen von IDEs oder anderen Toolchains gerecht zu werden. Im Gegensatz dazu müssen bei Pkl Einschränkungsdefinitionen zusammen mit Typdefinitionen geschrieben werden, und Typüberprüfungen und Einschränkungsvalidierung werden einheitlich zur Laufzeit durchgeführt.

### Entwicklertools

In Bezug auf die Entwicklertools priorisieren sowohl Pkl als auch KCL die Entwicklerproduktivität und bieten eine breite Palette von Sprachwerkzeugen und IDE-Plugin-Unterstützung. Neben den grundlegenden Sprachwerkzeugen bietet die Pkl-Website hauptsächlich Unterstützung für drei IDE-Plugins: IntelliJ, NeoVim und VS Code. Interessanterweise bietet KCL derzeit auch Unterstützung für diese drei IDE-Plugins, obwohl ihre Funktionen und Schwerpunkte leicht unterschiedlich sind.

Da Pkl in Java und Kotlin entwickelt wurde, kann es leicht an das JetBrains-IDE-Plugin-Ökosystem angepasst werden. Daher ist die Unterstützung des IntelliJ-Plugins für Pkl am umfassendsten. Da Pkl selbst jedoch keinen Language Server bereitstellt, basieren die NeoVim- und VS-Code-Plugins auf dem Tree-Sitter-Parsergenerator und bieten nur grundlegende Hervorhebung und Code-Faltung ohne weiter fortgeschrittene Funktionen wie Definitionsnavigation, Code-Refactoring und automatische Vervollständigung. Obwohl Pkl ein Apple-Projekt ist, wird es nicht in Swift entwickelt, und es gibt keine IDE-Plugins für XCode.

Im Gegensatz dazu bietet KCL, das in Rust entwickelt wurde, Unterstützung für Language Server und ermöglicht eine einfache Integration mit IDE-Plugins neben VS Code, einschließlich NeoVim und einigen aufkommenden LSP-unterstützenden IDEs oder Editoren. Der Language Server von KCL bietet vollständige Funktionen wie Code-Hervorhebung, automatische Vervollständigung, Navigation, Refactoring und Schnellkorrekturen. Da IntelliJ nur in seiner Professional-Version eine begrenzte LSP-Unterstützung bietet, haben wir das IntelliJ-Plugin mit entsprechender Java-Implementierungsunterstützung ergänzt. Im Vergleich zum VS-Code-KCL-Plugin hat die Funktionalität des IntelliJ-Plugins derzeit jedoch Verbesserungs- und Erweiterungsbedarf.

Zusammenfassend haben sowohl Pkl als auch KCL Verbesserungspotenzial in Bezug auf die Entwicklerproduktivität. Aufgrund von Unterschieden in der Implementierung von IDE-Plugins besteht Bedarf an weiteren Verbesserungen des IDE-Erlebnisses und des Workflows durch eine engere Zusammenarbeit mit der Open-Source-Community.

### Mehrere Sprachbindungen

Um die Konfigurationssprache besser in Benutzeranwendungen zu integrieren, bietet Pkl Bindungen für vier verschiedene Sprachen: Java, Kotlin, Swift und Go. Interessanterweise bietet KCL auch vier SDKs, darunter Go, Python, Java und Rust.

Vor der Open-Source-Veröffentlichung von Pkl war KCL eines der wenigen, wenn nicht das einzige, Projekt, das offiziell mehrere Sprachbindungen und IDE-Plugins bereitstellte. Dieser Erfolg war das Ergebnis gemeinsamer Anstrengungen und der angesammelten Beiträge von Community-Benutzern und -Entwicklern. Obwohl Pkl nur wenige Kernbetreuer hat, bot es nach seiner Open-Source-Veröffentlichung viele Funktionen, die mit denen von KCL verglichen werden können. Dies legt nahe, dass erhebliche Anstrengungen und Zeit in seine Entwicklung geflossen sind, was lobenswert ist.

### Summary

Die Vergleichstabelle unten umfasst die Funktionen von Pkl und KCL zur Referenz zusammen.

| Features                              | Pkl                           | KCL                       |
| ------------------------------------- | ----------------------------- | ------------------------- |
| Open Source License                   | Apache-2.0                    | Apache-2.0                |
| Programmiersprache                    | Java, Kotlin                  | Rust                      |
| Sprachstil                            | Similar to Swift, Kotlin      | Similar to Python, Go     |
| Sprachfunktionalität                  | Strong                        | Medium                    |
| Kompilationsausführungsmethode        | JIT                           | AOT                       |
| Laufzeitperformance                   | Medium                        | Medium                    |
| Inkrementelle Kompilierung            | ✅                            | ✅                        |
| Standardbibliothek                    | ✅                            | ✅                        |
| Paketverwaltungs tool                 | ✅                            | ✅                        |
| Formatierungs tool                    | ❌                            | ✅                        |
| Dokumentations tool                   | ❌                            | ✅                        |
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
