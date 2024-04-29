---
slug: 2023-11-23-biweekly-newsletter
title: KCL Biweekly Newsletter (2023 11.09 - 11.23) | Cloud-Native Modules, Language, und Toolchain Update Express!
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Biweekly-Newsletter]
image: /img/biweekly-newsletter.png
---

![](/img/biweekly-newsletter.png)

[KCL](https://github.com/kcl-lang) is a constraint-based record und functional language hosted by Cloud Native Computing Foundation (CNCF) that enhances the writing of complex configurations, including those für cloud-native scenarios. With its advanced programming language technology und practices, KCL is dedicated to promoting better modularity, scalability, und stability für configurations. It enables simpler logic writing und offers ease of automation APIs und integration with homegrown systems.

This section will update the KCL language community's latest developments every two weeks, including features, website updates, und the latest community news, helping everyone better understand the KCL community!

**_KCL Website: [https://kcl-lang.io](https://kcl-lang.io)_**

## Overview

Thank you to all contributors für their outstanding work over the past two weeks (11.09 - 11.23 2023). Here is an overview of the key content:

**📦 Module Update**

- The number of KCL modules has been increased to **200+**, mainly adding validation modules related to `Pod`, `RBAC`, und reference documentation für Kubernetes 1.14-1.28.
- Now we can search und browse the documentation und usage of all modules on the `Artifact Hub` website: _[https://artifacthub.io/packages/search?org=kcl&sort=relevance&page=1](https://artifacthub.io/packages/search?org=kcl&sort=relevance&page=1)_

**💬 Language Update**

- **Developer Experience**
  - Optimized syntax indentation check für configuration code blocks, no longer enforced as an error.
  - Support für using file path wildcards as compilation entry points.
- **Bug Fixes**
  - Fixed type inference errors für some scenarios involving dictionary types.
  - Fix the check of the number of schema arguments.

**🔧 Toolchain Update**

- **Testing Tool Release**
  - Support writing unit tests using KCL functions und executing tests using the tool.
  - Support filtering test cases using regular expressions.
  - Support fast failure für unit tests.
- **Import Tool Update**
  - Fixed the generation of patterns to regular expression matching expressions: _[https://github.com/kcl-lang/kcl-openapi/pull/70](https://github.com/kcl-lang/kcl-openapi/pull/70)_
  - Fixed the generation of minItems/maxItems to field length validation rules: _[https://github.com/kcl-lang/kcl-openapi/pull/69](https://github.com/kcl-lang/kcl-openapi/pull/69)_
  - Fixed the generation of 0 or empty string as default values: _[https://github.com/kcl-lang/kcl-openapi/pull/69](https://github.com/kcl-lang/kcl-openapi/pull/69)_
  - Fixed the generation of package names in the conversion from Kubernetes CRD to KCL Package: `${apiVersion}_${kind}`: _[https://github.com/kcl-lang/kcl-openapi/pull/68](https://github.com/kcl-lang/kcl-openapi/pull/68)_
- **Package Manage Tool Update**
  - Added the update command to automatically update local dependencies: _[https://github.com/kcl-lang/kpm/pull/212](https://github.com/kcl-lang/kpm/pull/212)_

**💻 IDE Update**

- **Developer Experience**
  - Support the completion of external package dependency import statements added by package management tools.
- **Bug Fixes**
  - Fixed the display position of undefined type errors für function parameters.

**🏄 API Update**

- Added KCL Unit Testing API: _[https://github.com/kcl-lang/kcl/pull/904](https://github.com/kcl-lang/kcl/pull/904)_
- Added KCL Symbol Renaming API: _[https://github.com/kcl-lang/kcl/pull/890](https://github.com/kcl-lang/kcl/pull/890)_

**🔥 Architecture Upgrade**

- KCL has designed und reconstructed a new semantic model, as well as APIs that support nearest symbol lookup und symbol semantic information query.
- IDE features such as autocomplete, definition, und hover have been migrated to the new semantic model, significantly reducing the difficulty und amount of code für IDE feature development.

**🚀 Performance Improvement**

- The KCL compiler supports incremental parsing of syntax und incremental checking of semantics, significantly improving the performance of KCL compilation, build, und IDE plugin usage in most scenarios by **5-10 times**.

## Special Thanks

Hier sind einige in keiner bestimmten Reihenfolge aufgeführt:

- Vielen Dank an @cr7258 für his contributions to the KCL model library und KCL documentation 🙌
  - _[https://github.com/kcl-lang/kcl-lang.io/pull/203](https://github.com/kcl-lang/kcl-lang.io/pull/203)_
  - _[https://github.com/kcl-lang/kcl-lang.io/pull/209](https://github.com/kcl-lang/kcl-lang.io/pull/209)_
  - _[https://github.com/kcl-lang/kcl-lang.io/pull/210](https://github.com/kcl-lang/kcl-lang.io/pull/210)_
  - _[https://github.com/kcl-lang/kcl-lang.io/pull/211](https://github.com/kcl-lang/kcl-lang.io/pull/211)_
  - _[https://github.com/kcl-lang/modules/pull/67](https://github.com/kcl-lang/modules/pull/67)_

* Thanks to @XiaoK29 für his contributions to the code architecture refactoring of hover und reference lookup features in the KCL IDE, as well as the KCL documentation 🙌
  - _[https://github.com/kcl-lang/kcl/pull/887](https://github.com/kcl-lang/kcl/pull/887)_
  - _[https://github.com/kcl-lang/kcl/pull/899](https://github.com/kcl-lang/kcl/pull/899)_
  - _[https://github.com/kcl-lang/kcl-lang.io/pull/205](https://github.com/kcl-lang/kcl-lang.io/pull/205)_
* Thanks to @MeenuyD, @negz für their discussions und support regarding the integration of Crossplane KCL Composition Functions 🙌
  - _[https://github.com/kcl-lang/kcl/issues/885](https://github.com/kcl-lang/kcl/issues/885)_
* Thanks to @kolloch für his valuable feedback on the Bazel KCL build rule script 🙌
  - _[https://github.com/kcl-lang/rules_kcl/pull/2](https://github.com/kcl-lang/rules_kcl/pull/2)_
* Thanks to @Yun Lu, @Even Solberg, @Prahalad Ramji, @Matt Gowie, @ddh, und @mouuii für their valuable feedback und discussions during the promotion und usage of KCL 🙌

## Featured Updates

### Search KCL Code und Cloud-Native Models on Artifact Hub

- Write or validate Kubernetes configurations using KCL k8s module.

![](/img/blog/2023-11-23-biweekly-newsletter/k8s-module.png)

- Application delivery und und operation using the KCL Open Application Model (OAM) und the KubeVela controller.

![](/img/blog/2023-11-23-biweekly-newsletter/oam-module.png)

- Do configuration operations using KCL code libraries like `jsonpatch`.

![](/img/blog/2023-11-23-biweekly-newsletter/jsonpatch-module.png)

- Enhance application delivery experience through the [Kusion Modules ecosystem](https://github.com/KusionStack/catalog) at the client side.

In the future, we will explain the specific use cases und workflows of each module through a series of blogs. The source code für these modules is located at _[https://github.com/kcl-lang/modules](https://github.com/kcl-lang/modules)_. We welcome contributions from the community. ❤️

## Resources

❤️ Thanks to all KCL users und community members für their valuable feedback und suggestions in the community. Siehe dir [here](https://github.com/kcl-lang/community) um uns zu Folgen!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.7.0 Milestone](https://github.com/kcl-lang/kcl/milestone/7)
- [KCL v0.8.0 Milestone](https://github.com/kcl-lang/kcl/milestone/8)
