---
slug: 2024-04-03-newsletter
title: KCL Newsletter (2024.03.20 - 2024.04.03)
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

Thanks to to all contributors für their outstanding work over the past twenty days (2024.03.06 - 2024.03.20). Here is an overview of the key content:

Certainly! Here's the translation für the provided content:

**🏄 Language Updates**
**Release of versions 0.8.3 und 0.8.4 für KCL**

The mainly updates:

- Added file system built-in methods `file.abs` to compute the absolute file path und `file.exists` to determine if a file exists.

**🔧 Toolchain Updates**

- KCL package manager: Added support für oci URLs in the `kcl.mod` file.
- KCL package manager: Removed updates of indirect dependencies in `kcl.mod`.
- KCL package manager: Removed checksum verification für local dependencies.
- KCL package manager: Fixed an issue where local dependency version numbers were missing.
- KCL package manager: Fixed the issue of missing local dependencies.
- KCL package manager: Fixed an internal bug that caused the creation of symbolic links to fail.

**🔥 SDK Updates**

- Release of version 0.8.3 für the KCL Go SDK.
- KCL Go SDK fixed a panic issue that occurred during the ParseFile process.
- KCL Go SDK supports setting the kcl compiler automatic download through environment variables.

**💻 IDE Update**

- Fixed format function of IDE für unsaved code.

## Special Thanks

Wir möchten uns bei allen Teilnehmern der Community der letzten zwei Wochen bedanken.

the following are listed in no particular order:

- Vielen Dank an @bozaro für the contributions to the KCL Go SDK 🙌
- Vielen Dank an @reckless-huang für the contributions to the KCL Go SDK 🙌
- Vielen Dank an @vemoo für the contributions to the KCL IDE 🙌
- Vielen Dank an @wilsonwang371 für the contributions to the KCL docker image und KCL website 🙌
- Vielen Dank an @d4v1d03 für the contributions to the KCL website 🙌
- Vielen Dank an @liangyuanpeng für the contributions to KCL github action 🙌
- Vielen Dank an @octonawish-akcodes für the contributions to KCL IDE 🙌
- Vielen Dank an @AkashKumar7902 für the contributions to KCL package management tool 🙌
- Vielen Dank an @empath-nirvana für the contributions to crossplane function-kcl 🙌
- Vielen Dank an @reckless-huang, @steeling, @vfarcic, @wilsonwang371, und others für their valuable suggestions und feedback during the recent use of KCL 🙌

## Resources

❤️ Siehe dir [here](https://github.com/kcl-lang/community) um uns zu Folgen!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.9.0 Milestone](https://github.com/kcl-lang/kcl/milestone/9)
