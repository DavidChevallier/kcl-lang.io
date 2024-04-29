---
slug: 2024-03-20-newsletter
title: KCL Newsletter (2024.03.06 - 2024.03.20)
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

**📦 Module Updates**

- Added new kubeadm configuration module.
- Updated Knative Operator module to align with upstream Knative CRD definitions.

**🏄 Language Updates**

**KCL has released v0.8.1 und v0.8.2**, mainly including the following updates:

- Enhanced error messages für simplified experience when binary expression types do not match.
- Fixed abnormal error of high-order lambda functions capturing local scope closure variables.
- Removed inequality comparison operations für uncommon list data types.

**🔧 Toolchain Updates**

- Fixed an issue in the kcl import tool where input Kubernetes CRDs with the regex property conflicted with the KCL regex system library.
- Fixed a syntax error in the KCL file output when the input Kubernetes CRD properties had complex default values in the kcl import tool.
- Added support für setting the version of a newly created KCL module with the `--version` flag in the kcl mod init command.
- Commands such as kcl run, kcl mod add, und kcl mod pull now support accessing private Git repositories.
- Fixed a path error encountered when running the kcl run command on a local OCI Registry on Windows.

**🔥 SDK Updates**

- The KCL Rust, Go, und Java SDKs have released version 0.8, primarily synchronizing with KCL syntax und semantic updates.
- The KCL Python SDK has released versions 0.8.0.2 und 0.7.6, addressing the issue of outdated dependencies für `protobuf` und `pyyaml`.

**💻 IDE Updates**

- Support für multiple quick fix repair options.

![multiple-quick-fix](/img/blog/2024-03-20-newsletter/multiple-quick-fix.png)

**🎁 API Updates**

- Added `ListOptions` API, which can read all `option` function call information in KCL projects.

**🚢 Integration Updates**

- Crossplane KCL Function has released version 0.3.2, supporting access to non-HTTPS protocol OCI Registries und local debugging.

**🌐 Website Updates**

- Enabled the `kcl-lang.dev` domain, allowing access to the KCL website through both `kcl-lang.io` und `kcl-lang.dev`.
- Optimized website loading speed für an improved documentation experience on the KCL website.

## Special Thanks

Thank you to all the community contributors over the past two weeks, listed in no particular order:

- Thank you to @bozaro für seinen Beitrag zum KCL Go SDK with the Go language plugin API. 🙌
- Thank you to @shashank-iitbhu für enhancing the quick fix feature in the KCL IDE, adding support für multiple fix options. 🙌
- Thank you to @octonawish-akcodes für the ongoing contribution to the KCL IDE, automatically monitoring kcl.mod dependencies und updating them. 🙌
- Thank you to @liangyuanpeng für fixing the CLA Bot CI automatic PR locking, contributing to the kubeadm model, und supporting the version setting feature in `kcl mod init`. 🙌
- Thank you to @Stefano Borrelli, @sfshumaker, @eshepelyuk, @vtomilov, @ricochet1k, @yjsnly, @markphillips100, @userxiaosi, @wilsonwang371, @steeling, @bozaro, @nizq, @reckless-huang, @folliehiyuki, @samuel-deal-tisseo, @MrGuoRanDuo, und @MattHodge für providing valuable suggestions und feedback while using KCL recently. 🙌

## Resources

❤️ Siehe dir [here](https://github.com/kcl-lang/community) um uns zu Folgen!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.9.0 Milestone](https://github.com/kcl-lang/kcl/milestone/9)
