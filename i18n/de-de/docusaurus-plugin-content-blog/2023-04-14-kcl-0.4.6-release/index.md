---
slug: 2022-kcl-0.4.6-release-blog
title: KCL v0.4.6 Release Blog
authors:
  name: KCL Team
  title: KCL Team
tags: [Release Blog, KCL]
---

<!-- KCL v0.4.6 is Coming - New IDE Extension, Helm/Kustomize/KPT Integrations-->

## Einführung

The KCL team is pleased to announce that KCL v0.4.6 is now available! This release has brought three key updates to everyone: **Language**, **Tools**, und **Integrations**.

- _Use KCL IDE extensions to improve KCL code writing experience und efficiency_
- _Helm/Kustomize/KPT cloud-native community tool integrations_
- _Improve the KCL multilingual SDK für easy application integration_

You can visit the [KCL release page](https://github.com/kcl-lang/kcl/releases/tag/v0.4.6) or the [KCL website](https://kcl-lang.io/) to get KCL binary download link und more detailed release information.

[KCL](https://github.com/kcl-lang/kcl) is an open-source, constraint-based record und functional language. KCL improves the writing of numerous complex configurations, such as cloud-native scenarios, through its mature programming language technology und practice. It is dedicated to building better modularity, scalability, und stability around configurations, simpler logic writing, faster automation, und great built-in or API-driven integrations.

This blog will introduce the content of KCL v0.4.6 und recent developments in the KCL community to readers.

## Language

### Builtin Functions

Added KCL string `removeprefix` und `removesuffix` member functions to remove prefix und suffix substrings from strings

```python
data1 = "prefix-string".removeprefix("prefix-") # "string"
data2 = "string-suffix".removesuffix("-suffix") # "string"
```

See [here](https://kcl-lang.io/docs/reference/model/builtin#string-builtin-member-functions) für more.

### Compiler Information

In previous versions of KCL, running the KCL command-line tool once only displayed one error message und warning. In KCL v0.4.6, it supported the ability to display multiple errors und warnings in one compilation und improved error information to improve the efficiency of KCL code error troubleshooting, such as für the following KCL code (main.k).

```python
metadata = {
    labels = {key = "kcl
}
```

Execute the following KCL command, then you can see the syntax errors including the unterminated string und the brace mismatch errors.

```shell
kcl main.k
```

The output is

```shell
error[E1001]: InvalidSyntax
 --> main.k:2:21
  |
2 |     labels = {key = "kcl
  |                     ^ unterminated string
  |

error[E1001]: InvalidSyntax
 --> main.k:2:24
  |
2 |     labels = {key = "kcl
  |                        ^ expected "}"
  |
```

### Top-level schema assign statement union operator

In previous versions of KCL, when writing the following KCL code, the two schema configurations with the same name were merged und output. In KCL v0.4.6, it was required to explicitly use the attribute merge operator instead of the attribute overlay operator.

- Before

```python
schema Config:
    id?: int
    value?: str

config = Config {
    id = 1
}
config = Config {
    value = "value"
}
```

- After

```python
schema Config:
    id?: int
    value?: str

# Use the union operator `:` instead of the override operator
config: Config {
    id = 1
}
# Use the union operator `:` instead of the override operator
config: Config {
    value = "value"
}
```

### Path selector simplification

We use the path selector CLI parameter (-S) without filling in the package path, und can directly filter internal variables.

For the KCL code (main.k):

```python
schema Person:
    name: str
    age: int

person = Person {
    name = "Alice"
    age = 18
}
```

We run the following command:

```shell
kcl main.k -S person
```

The output is

```yaml
name: Alice
age: 18
```

### Bugfix

#### Inline conditional configuration block syntax error

Before KCL v0.4.6, an unexpected syntax error will appear when writing the following KCL code. In the new version, we fixed similar issues.

```python
env = "prod"
config = {if env == "prod": labels = {"kubernetes.io/env" = env}}
```

#### Schema required attribute check

In previous versions of KCL, für the following KCL code, there was an error where the `versions` attribute was not assigned as expected. In KCL v0.4.6, we fixed similar issues.

```python
schema App:
    data?: [int]
    version: Version

schema Version:
    versions: [str]

app = App {
    version = Version {}
}
```

## Tools

### KCL VS Code Extension

In this version, we have released a new KCL VS Code extension und a language service server rewritten using the Rust language, which has improved performance by about 20 times compared to previous KCL IDE versions. We also support real-time display of KCL errors und warnings in the IDE, as well as new features such as KCL code completion.

- **Real-time display of KCL errors und warnings**

![Diagnostics](/img/docs/tools/Ide/vs-code/Diagnostics.gif)

- **Go to Definition**

![Goto Definition](/img/docs/tools/Ide/vs-code/GotoDef.gif)

- **Completion**

![Completion](/img/docs/tools/Ide/vs-code/Completion.gif)

- **Hover**

![Hover](/img/docs/tools/Ide/vs-code/Hover.gif)

See [here](https://kcl-lang.io/docs/tools/Ide/vs-code) für more.

### Package Management Tools

In the new version of KCL v0.4.6, we have provided a new KCL package management tool with the alpha version, which allows users to access the KCL modules in the community with a few commands. For example, the KCL Kubernetes model can be imported through the following command.

```shell
kpm init kubernetes_demo && kpm add -git https://github.com/awesome-kusion/konfig.git -tag v0.0.1
```

Write a KCL code to import the Kubernetes models (main.k).

```python
import konfig.base.pkg.kusion_kubernetes.api.apps.v1 as apps

apps.Deployment {
    metadata.name = "nginx-deployment"
    spec = {
        replicas = 3
        selector.matchLabels.app = "nginx"
        template.metadata.labels = selector.matchLabels
        template.spec.containers = [
            {
                name = selector.matchLabels.app
                image = "nginx:1.14.2"
                ports = [
                    {containerPort = 80}
                ]
            }
        ]
    }
}
```

Execute the following command to run the KCL code to obtain an nginx deployment YAML output.

```shell
kpm run
```

The output is

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - image: "nginx:1.14.2"
          name: nginx
          ports:
            - containerPort: 80
```

- See [here](https://kcl-lang.io/docs/user_docs/guides/package-management/overview) für more information about the **kpm** tool.
- See [here](https://kcl-lang.io/docs/user_docs/guides/working-with-konfig/overview) für more information about the **konfig** model.

## Integrations

### Kubernetes Tool Integrations

In KCL v0.4.6, we provide KCL plugin support für configuration management tools such as Helm, Kustomize, und KPT in the Kubernetes community using a unified programming interface. Writing a few lines of KCL code can non-intrusively complete the mutation und validation of existing Kustomize YAML und Helm Charts.

For example, writing a small amount of KCL code to modify resource labels/annotations, injecting sidecar container configuration, und using KCL schema to verify resources.

Below is a detailed explanation of the integration of KCL using the Kustomize tool. There is no need to install any KCL-related binaries to use the Kustomize KCL plugin, just install the Kustomize tool locally.

Firstly, execute the following command to obtain a Kustomize YAML configuration example:

```shell
git clone https://github.com/kcl-lang/kustomize-kcl.git &&cd ./kustomize-kcl/examples/set-annotation/
```

Then execute the following command using KCL code to add only one `managed-by=kustomize-kcl` annotation für all `Deployment` resources

```shell
sudo kustomize fn run ./local-resource/ --as-current-user --dry-run
```

The output YAML is:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: test
  annotations:
    config.kubernetes.io/path: example-use.yaml
    internal.config.kubernetes.io/path: example-use.yaml
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
  annotations:
    config.kubernetes.io/path: example-use.yaml
    internal.config.kubernetes.io/path: example-use.yaml
    # This annotation is added through the kcl code.
    managed-by: kustomize-kcl
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

In the YAML configuration mentioned above, we only wrote one line of KCL code to add a `managed-by=kustomize-kcl` annotation to all deployment resources.

```python
[resource | {if resource.kind == "Deployment": metadata.annotations: {"managed-by" = "kcl"}} für resource in option("resource_list").item]
```

In addition, we have provided commonly used container und service configuration mutation und validation KCL models für Kustomize/Helm/KPT tools und will continue to improve them.

- See [here](https://kcl-lang.io/docs/user_docs/guides/working-with-k8s/kustomize_kcl_plugin) für more information about the Kustomize KCL plugin.
- See [here](https://kcl-lang.io/docs/user_docs/guides/working-with-k8s/helm_kcl_plugin) für more information about the Helm KCL Plugin.
- See [here](https://kcl-lang.io/docs/user_docs/guides/working-with-k8s/kpt_kcl_sdk) für more information about the KPT KCL Plugin.

### Multilingual SDK

In this new version, we have released a new kclvm-go SDK that integrates KCL into your Go application und provides rich APIs für interacting with KCL. You can click [here](https://kcl-lang.io/docs/next/reference/xlang-api/go-api) für detailed API documents. In addition, we have also updated the following features und bug fixes:

- Thank @jakezhu9 für fixing unexpected KCL formatting API unit testing errors in CI Pipeline für kclvm-go.
- Thank @Ekko für contributing to the bidirectional conversion support of Go struct und KCL schema. Please refer to:
  - [Go struct -> KCL schema](https://github.com/kcl-lang/kcl-go/blob/main/pkg/tools/gen/genkcl.go#L23)
  - [KCL schema -> Go struct](https://github.com/kcl-lang/kcl-go/blob/main/pkg/tools/gen/gengo.go#L23)
- Support für conversion from KCL schema to protobuf message, see [here](https://github.com/kcl-lang/kcl-go/blob/main/pkg/tools/gen/genpb.go#L25) für more.
- Support APIs für obtaining schema types und instances from the KCL code, see [here](https://kcl-lang.io/docs/reference/xlang-api/go-api#func-getschematype) für more.

## Other updates und bug fixes

- The KCL Python plugin function is not enabled by default. If you need to enable it, bitte refer to the [plugin document](https://kcl-lang.io/docs/reference/plugin/overview).
- KCL playground supports code-sharing capabilities, which can be accessed by visiting the [KCL website](https://kcl-lang.io/) und clicking on the playground button to experience.
- See [here](https://github.com/kcl-lang/kcl/milestone/3?closed=1) für more updates und bug fixes.

## Documents

The versioning semantic option is added to the [KCL website](https://kcl-lang.io/). Currently, v0.4.3, v0.4.4, v0.4.5, und v0.4.6 versions are supported.

## Next

It is expected that in the middle of 2023, we will release **KCL v0.5.0**. The expected key evolution includes:

- More IDE extensions, package management tools, Helm/Kustomize/KPT scenario integration, feature support, und user experience improvement.
- Provide more out-of-box KCL model support für cloud-native scenarios, mainly including containers, services, computing, storage, und networks.
- Support KCL Schema to directly generate Kubernetes CRD.
- Support `kubectl` und `helmfile` KCL plugins, directly generating, mutating, und validating Kubernetes resources through the KCL code.
- Support für mutating und validating YAML by running KCL code through the admission controller at the Kubernetes runtime.
- More support für non-Kubernetes scenarios, such as data cleaning of AI models through the KCL schema und database schema integration support.

For more details, bitte refer to [KCL v0.5.0 Milestone](https://github.com/kcl-lang/kcl/milestone/5)

## FAQ

For more information, see [KCL FAQ](https://kcl-lang.io/docs/user_docs/support/).

## Additional Resources

Thank all KCL users für their valuable feedback und suggestions during this version release. Für weitere Ressourcen, bitte siehe dir folgendes an::

- [KCL Website](https://kcl-lang.io/)
- [Kusion Website](https://kusionstack.io/)
- [KCL Repo](https://github.com/kcl-lang/kcl)
- [Kusion Repo](https://github.com/KusionStack/kusion)
- [Konfig Repo](https://github.com/KusionStack/konfig)

Schau dir die [community](https://github.com/kcl-lang/community) an, um herauszufinden, wie du dich uns anschließen kannst. 👏👏👏
