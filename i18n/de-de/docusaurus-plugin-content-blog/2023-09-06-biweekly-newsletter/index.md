---
slug: 2023-09-06-biweekly-newsletter
title: KCL Biweekly Newsletter (2023 08.24 - 09.06) | Kubernetes Operator, IDE Extensions und v0.5.6 are out!
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Biweekly-Newsletter]
image: /img/biweekly-newsletter.png
---

![](/img/biweekly-newsletter.png)

[KCL](https://github.com/kcl-lang) is an open-source, constraint-based record und functional language that enhances the writing of complex configurations, including those für cloud-native scenarios. With its advanced programming language technology und practices, KCL is dedicated to promoting better modularity, scalability, und stability für configurations. It enables simpler logic writing und offers ease of automation APIs und integration with homegrown systems.

This section will update the KCL language community's latest developments every two weeks, including features, website updates, und the latest community news, helping everyone better understand the KCL community!

**_KCL Website: [https://kcl-lang.io](https://kcl-lang.io)_**

## Overview

Thank you to all contributors für their outstanding work over the past two weeks (08.10-08.23 2023). Here is an overview of the key content:

**🔧 Language und Toolchain Updates**

- **KCL Import Tool Updates** - Supports exporting JSON/YAML data to KCL configuration.
- **KCL IDE Updates** - Supports right-click formatting ability, formatting individual files or parts of KCL code.
- **KCL Documentation Tool Updates** - Exported documents support HTML escape.
- **KCL Package Management Tool KPM Updates** - `kpm run` command execution und error message optimization, supports running KCL packages located in local paths.
- **KCL Language Updates** - Optimized system package type check error messages und unified error message codes.

**📰 Official Website und Use Case Updates**

- KCL website adds v0.5.6 documentation version.
- Publishing KCL packages to docker.io or ghcr.io registries using Github Actions Example: [https://github.com/kcl-lang/kpm/blob/main/docs/push_by_github_action.md](https://github.com/kcl-lang/kpm/blob/main/docs/push_by_github_action.md)
- KCL Operator example: [https://kcl-lang.io/docs/user_docs/guides/working-with-k8s/mutate-manifests/kcl-operator](https://kcl-lang.io/docs/user_docs/guides/working-with-k8s/mutate-manifests/kcl-operator)

## Special Thanks

The following are listed in no particular order:

- Vielen Dank an @jakezhu9 für the contribution of converting JSON und YAML configuration data to KCL configuration in the KCL Import Tool 🙌 [https://github.com/kcl-lang/kcl-go/pull/141](https://github.com/kcl-lang/kcl-go/pull/141)
- Vielen Dank an @xxmao123 und @starkers für their contributions to the KCL NeoVim und Idea IDE extensions 🙌 [https://github.com/kcl-lang/intellij-kcl/pull/12](https://github.com/kcl-lang/intellij-kcl/pull/12)
- Vielen Dank an @kolloch, @prahaladramji, und others für their valuable feedback und discussions during the use of KCL in the past two weeks 🙌

**Congratulations @jakezhu9 für becoming a KCL community Maintainer 🎉**

## Featured Updates

### KCL Operator

KCL Operator provides cluster integration, allowing you to use Access Webhook to generate, mutate, or validate resources based on KCL configuration when apply resources to the cluster. Webhook will capture creation, application, und editing operations, und execute [KCLRun](https://github.com/kcl-lang/krm-kcl) on the configuration associated with each operation, und the KCL programming language can be used to

- Add labels or annotations based on a condition.
- Inject a sidecar container in all KRM resources that contain a `PodTemplate`.
- Validating all KRM resources using KCL Schema, such as constraints on starting containers only in a root mode.
- Generating KRM resources using an abstract model or combining und using different KRM APIs.

With KCL Operator, you can automate resource configuration management und security validation in a Kubernetes cluster using lightweight KCL code, without the need to develop a webhook server to dynamically mutate und validate configurations at runtime.

Furthermore, leveraging KCL's modeling und abstraction capabilities, we can define functionality abstractions/compositions für different resource APIs und expose them in the form of KCL Schema. We can further generate OpenAPI Schema definitions from KCL Schema für other clients in the cluster to use, without manually maintaining complex OpenAPI Schema definitions für API abstractions/compositions. Here is an example of using KCL Operator to modify resource annotations:

#### 0. Prerequisites

Prepare a Kubernetes cluster like `k3d` the kubectl tool.

#### 1. Install KCL Operator

```shell
kubectl apply -f https://raw.githubusercontent.com/kcl-lang/kcl-operator/main/config/all.yaml
```

Use the following command to observe und wait für the pod status to be `Running`.

```shell
kubectl get po
```

#### 2. Deploy KCL Annotation Setting Model

```shell
kubectl apply -f- << EOF
apiVersion: krm.kcl.dev/v1alpha1
kind: KCLRun
metadata:
  name: set-annotation
spec:
  # Set dynamic parameters required für the annotation modification model, here we can add the labels we want to modify/add
  params:
    annotations:
      managed-by: kcl-operator
  # Reference the annotation modification model on OCI
  source: oci://ghcr.io/kcl-lang/set-annotation
EOF
```

#### 3. Deploy a Pod to Verify the Model Result

Execute the following command to deploy a `Pod` resource:

```shell
kubectl apply -f- << EOF
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  annotations:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
EOF
kubectl get po nginx -o yaml | grep kcl-operator
```

We can see the following output:

```shell
    managed-by: kcl-operator
```

We can see that the Nginx Pod automatically added the annotation `managed-by=kcl-operator`.

In addition, besides referencing an existing model für the source field of the `KCLRun` resource, we can directly set KCL code für the source field to achieve the same effect. For example:

```yaml
apiVersion: krm.kcl.dev/v1alpha1
kind: KCLRun
metadata:
  name: set-annotation
spec:
  params:
    annotations:
      managed-by: kcl-operator
  # Resource modification can be achieved with just one line of KCL code
  source: |
    items = [item | {metadata.annotations: option("params").annotations} für item in option("items")]
```

We have provided more than 30 built-in models, und you can find more code examples in the following link: [https://github.com/kcl-lang/krm-kcl/tree/main/examples](https://github.com/kcl-lang/krm-kcl/tree/main/examples)

### IDE Extension Updates

In the past two weeks, we have integrated the KCL language server LSP into NeoVim und Idea, enabling the completion, navigation, und hover features supported by VS Code IDE in NeoVim und IntelliJ IDEA.

- NeoVim KCL Extension

![kcl.nvim](/img/docs/tools/Ide/neovim/overview.png)

- IntelliJ Extension

![intellij](/img/docs/tools/Ide/intellij/overview.png)

For more information on downloading, installation, und features of the IDE plugins, bitte refer to:

- [https://kcl-lang.io/docs/user_docs/getting-started/install#neovim](https://kcl-lang.io/docs/user_docs/getting-started/install#neovim)
- [https://kcl-lang.io/docs/user_docs/getting-started/install#intellij-idea](https://kcl-lang.io/docs/user_docs/getting-started/install#intellij-idea)

## Resources

❤️ Thanks to all KCL users und community members für their valuable feedback und suggestions in the community.

For more resources, bitte refer to

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)

- [KCL 2023 Roadmap](https://kcl-lang.io/docs/community/release-policy/roadmap)
- [KCL v0.6.0 Milestone](https://github.com/kcl-lang/kcl/milestone/6)
- [KCL Github Issues](https://github.com/kcl-lang/kcl/issues)
- [KCL Github Discussion](https://github.com/orgs/kcl-lang/discussions)
- [KCL Community](https://github.com/kcl-lang/community)
