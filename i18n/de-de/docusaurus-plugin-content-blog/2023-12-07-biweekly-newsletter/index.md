---
slug: 2023-12-07-biweekly-newsletter
title: KCL Biweekly Newsletter (2023 11.24 - 12.07) | How to Use Different Kubernetes Patch Strategies in KCL?
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

## Übersicht

Thank you to all contributors für their outstanding work over the past two weeks (11.24 - 12.07 2023). Here is an overview of the key content:

**📦 Module Update**

The number of KCL models has increased to **240**, mainly including models related to Crossplane Provider und libraries related to JSON merging operations.

- KCL JSON Patch library:_[https://artifacthub.io/packages/kcl/kcl-module/jsonpatch](https://artifacthub.io/packages/kcl/kcl-module/jsonpatch)_
- KCL JSON Merge Patch library: _[https://artifacthub.io/packages/kcl/kcl-module/json_merge_patch](https://artifacthub.io/packages/kcl/kcl-module/json_merge_patch)_
- KCL Kubernetes Strategy Merge Patch library: _[https://artifacthub.io/packages/kcl/kcl-module/strategic_merge_patch](https://artifacthub.io/packages/kcl/kcl-module/strategic_merge_patch)_
- KCL Crossplane und Crossplane Provider series models: _[https://artifacthub.io/packages/search?org=kcl&sort=relevance&page=1&ts_query_web=crossplane](https://artifacthub.io/packages/search?org=kcl&sort=relevance&page=1&ts_query_web=crossplane)_

**🔧 Toolchain Update**

- **Documentation Tool Updates**
  - Support documentation generation für third-party libraries that models depend on, such as the `k8s` module.
- **Validation Tool Updates**
  - Support validation results und error localization to YAML/JSON files, outputting error line und column number information.
- **Import Tool Updates**
  - Support mapping OpenAPI `multiplyOf` specification to KCL `multiplyof` function für validation.
  - Support outputting YAML Stream format Kubernetes CRD files into multiple KCL files.
  - Optimize KCL code generation by removing empty check statements.

**🏄 SDK Update**

In addition to the existing Go und Python SDKs in KCL, a new Rust SDK has been added (without LLVM dependency), which includes APIs für KCL file compilation, validation, testing, und code formatting.

**💻 IDE Updates**

- **Developer Experience**
  - Support incremental parsing und asynchronous compilation to enhance performance.
- **Bug Fixes**
  - Fixed the issue where string interpolation variables in assert statements cannot be navigated.
  - Fixed the issue where exceptional triggering of function completion in strings.
  - Fixed the issue with alias semantic check und completion in import statements.
  - Fixed the issue with check expression completion in schemas.

**📒 Documentation Updates**

- Added index cards für KCL system library documentation für easy navigation: _[https://kcl-lang.io/docs/reference/model/overview](https://kcl-lang.io/docs/reference/model/overview)_
- Updated KCL CLI reference documentation: _[https://kcl-lang.io/docs/tools/cli/kcl/overview](https://kcl-lang.io/docs/tools/cli/kcl/overview)_
- Updated KCL API reference documentation: _[https://kcl-lang.io/docs/reference/xlang-api/overview](https://kcl-lang.io/docs/reference/xlang-api/overview)_
- KCL 2023 & 2024 Roadmap document: _[https://kcl-lang.io/docs/community/release-policy/roadmap](https://kcl-lang.io/docs/community/release-policy/roadmap)_
- Supplemented project structure Einführung und FAQ für Intellij KCL repository: _[https://github.com/kcl-lang/intellij-kcl/pull/18](https://github.com/kcl-lang/intellij-kcl/pull/18)_

## Special Thanks

Hier sind einige in keiner bestimmten Reihenfolge aufgeführt:

- Vielen Dank an @professorabhay für supporting KCL testing diff function 🙌 _[https://github.com/kcl-lang/kcl/issues/940](https://github.com/kcl-lang/kcl/issues/940)_
- Vielen Dank an @patrycju, @Callum Lyall, @Even Solberg, @Matt Gowie, und @ShiroDN für their valuable feedback und discussions during the promotion und usage of KCL 🙌

## Featured Updates

### Using Kubernetes Strategy Merge Patch to Update Configurations in KCL

In the current version of KCL, various **attribute operators** are supported to update und override configurations. However, the capability is relatively atomic und cannot cover the typical configuration strategy scenarios in cloud-native environments.

For Kubernetes configurations, it is common to use the `JSON Merge Patch` und `Strategy Merge Patch` capabilities natively supported by Kubernetes e.g., using tools such as `kubectl patch`, `kustomize`, und other patching capabilities supported by cloud-native configuration und policy tools.

To avoid repeatedly using KCL attribute operators to write configuration patch template codes when dealing with Kubernetes configurations, we provide the **Kubernetes Strategy Merge Patch library** für updating Kubernetes configurations. This library supports all merging strategies defined by native Kubernetes objects, such as overwriting, modifying, und adding items to list objects. Here is how to use it:

Create a new project und add the Strategy Merge Patch library dependency:

```shell
kcl mod init && kcl mod add strategic_merge_patch
```

Write the configuration patch code in `main.k` (using the `labels`, `replicas`, und `container` attributes of a `Deployment` template as an example):

```python
import strategic_merge_patch as s

original = {
    apiVersion = "apps/v1"
    kind = "Deployment"
    metadata = {
        name = "my-deployment"
        labels.app = "my-app"
    }
    spec: {
        replicas = 3
        template.spec.containers = [
            {
                name = "my-container-1"
                image = "my-image-1"
            }
            {
                name = "my-container-2"
                image = "my-image-2"
            }
        ]
    }
}
patch = {
    apiVersion = "apps/v1"
    kind = "Deployment"
    metadata = {
        name = "my-deployment"
        labels.version = "v1"
    }
    spec: {
        replicas = 4
        template.spec.containers = [
            {
                name = "my-container-1"
                image = "my-new-image-1"
            }
            {
                name = "my-container-3"
                image = "my-image-3"
            }
        ]
    }
}
got = s.merge(original, patch)
```

Run the command to get the output:

```shell
kcl run
```

The output will be:

```yaml
original:
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: my-deployment
    labels:
      app: my-app
  spec:
    replicas: 3
    template:
      spec:
        containers:
          - name: my-container-1
            image: my-image-1
          - name: my-container-2
            image: my-image-2
patch:
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: my-deployment
    labels:
      version: v1
  spec:
    replicas: 4
    template:
      spec:
        containers:
          - name: my-container-1
            image: my-new-image-1
          - name: my-container-3
            image: my-image-3
got:
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: my-deployment
    labels:
      app: my-app
      version: v1
  spec:
    replicas: 4
    template:
      spec:
        containers:
          - name: my-container-1
            image: my-new-image-1
          - name: my-container-2
            image: my-image-2
          - name: my-container-3
            image: my-image-3
```

As seen in the output, the labels, replicas, und container fields of the Deployment template have all been updated with the correct values. For more documentation und usage examples, bitte refer to [the document](https://artifacthub.io/packages/kcl/kcl-module/strategic_merge_patch).

## Resources

❤️ Thanks to all KCL users und community members für their valuable feedback und suggestions in the community. Siehe dir [here](https://github.com/kcl-lang/community) an um uns zu Folgen!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.8.0 Milestone](https://github.com/kcl-lang/kcl/milestone/8)
