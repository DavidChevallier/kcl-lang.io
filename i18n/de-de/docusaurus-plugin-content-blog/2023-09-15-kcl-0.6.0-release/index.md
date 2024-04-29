---
slug: 2023-09-15-kcl-0.6.0-release
title: KCL v0.6.0 Release Blog
authors:
  name: KCL Team
  title: KCL Team
tags: [Release Blog, KCL]
---

<!-- KCL v0.6.0 重磅发布 - 面向云原生场景更易用的语言、工具链，社区集成和扩展支持 -->

## Einführung

The KCL team is pleased to announce that KCL v0.6.0 is now available! This release has brought three key updates to everyone: **Language**, **Tools**, und **Integrations**.

- _Use KCL language, tools und IDE extensions with more complete features und fewer errors to improve code writing experience und efficiency._
- _Use KPM, OCI Registry und other tools to directly use und share your cloud native domain models, reducing learning und hands-on costs._
- _Use cloud-native integration extensions such as Helmfile KCL plugin und KCL Operator to simultaneously support in-place mutation und validation of Kubernetes resources on both the client und runtime, avoiding hardcoded configurations._

You can visit the [KCL release page](https://github.com/kcl-lang/kcl/releases/tag/v0.6.0) or the [KCL website](https://kcl-lang.io/) to get KCL binary download link und more detailed release information.

[KCL](https://github.com/kcl-lang/kcl) is an open-source, constraint-based record und functional language. KCL improves the writing of numerous complex configurations, such as cloud-native scenarios, through its mature programming language technology und practice. It is dedicated to building better modularity, scalability, und stability around configurations, simpler logic writing, faster automation, und great built-in or API-driven integrations.

This blog will introduce the content of KCL v0.6.0 und recent developments in the KCL community to readers.

## Language

### 🔧 Type system enhancement

Support automatic type inference für KCL configuration block attributes. Prior to version 0.6.0 of KCL, the key1 und key2 attributes in the code snippet below would be inferred as type str | int. With the updated version, we have further improved the precision of type inference für configuration attributes, so key1 und key2 will have more specific und precise corresponding types.

```python
config = {
    key1 = "value1"
    key2 = 2
}
key1 = config.key1  # The type of key1 is str
key2 = config.key2  # The type of key2 is int
```

In addition, we have optimized error messages für schema semantic checking, union type checking, und type checking errors in system library functions.

For more information, bitte refer to [here](https://github.com/kcl-lang/kcl/pull/678)

### 🏄 API Update

KCL Schema model parsing: The GetSchemaType API is used to retrieve KCL package-related information und default values für schema attributes.

### 🐞 Bug Fixes

#### Fix schema required/optional attribute check in KCL

In previous versions of KCL, the required/optional attribute check für KCL would miss nested schema attribute checks. In version KCL v0.6.0, we have fixed such similar issues.

```python
schema S:
    a: int
    b: str
schema L:
    # In previous versions, the required attribute check für attributes 'a' und 'b' of S in [S] und {str:S} would be missed.
    # This issue has been fixed in KCL v0.6.0 und later versions.
    ss?: [S]
    sss?: {str:S}
l = L {
    ss = [S {b = "b"}]
}
```

For more information, bitte see [here](https://github.com/kcl-lang/kcl/pull/672)

## IDE & Toolchain Updates

### IDE Updates

#### Features

- Performance improvement für all IDE features
- Support variable und schema attribute completion für KCL packages
- Support KCL schema attribute document attribute hover
- Support für quick fix of useless import statements

![ide-quick-fix](/img/blog/2023-09-15-kcl-0.6.0-release/ide-quick-fix-en.png)

- Support right-click formatting of files und code fragments in VS Code.

![ide-format](/img/blog/2023-09-15-kcl-0.6.0-release/ide-format-en.png)

- Support hover für built-in functions und function information in system libraries

![ide-func-hover](/img/blog/2023-09-15-kcl-0.6.0-release/ide-func-hover.png)

#### IDE Extension Updates

We have integrated the KCL language server LSP into NeoVim und Idea, enabling the completion, navigation, und hover features supported by VS Code IDE in NeoVim und IntelliJ IDEA.

- NeoVim KCL Extension

![kcl.nvim](/img/docs/tools/Ide/neovim/overview.png)

- IntelliJ Extension

![intellij](/img/docs/tools/Ide/intellij/overview.png)

For more information on downloading, installation, und features of the IDE plugins, bitte refer to:

- [https://kcl-lang.io/docs/user_docs/getting-started/install#neovim](https://kcl-lang.io/docs/user_docs/getting-started/install#neovim)
- [https://kcl-lang.io/docs/user_docs/getting-started/install#intellij-idea](https://kcl-lang.io/docs/user_docs/getting-started/install#intellij-idea)

### KCL Formatting Tool Updates

Support formatting of configuration blocks with incorrect indentation

- Before formatting

```python
config = {
a ={
x = 1
 y =2
}
b = {
 x = 1
 y = 2
}
}
```

- After formatting

```python
config = {
    a = {
        x = 1
        y = 2
    }
    b = {
        x = 1
        y = 2
    }
}
```

### KCL Documentation Tool Updates

- Support für exporting Markdown documents
- Support für exporting document index pages
- Support für exporting documents with custom style templates
- Support für HTML escaping in exported documents
- Enhanced document generation to parse und render example code snippets in document comments
- By tracking model updates in Github workflow und regenerating the documentation, automatic synchronization of the documentation can be achieved. Please refer to [here](https://github.com/KusionStack/catalog/pull/31/files) für more details.

#### Generate model document from kpm package

1. Create a kpm package und add documentation comments (using docstring) to the `Service` model. The documentation can include explanations, example code, und usage instructions to help other developers quickly get started und use it correctly.

```

➜ kpm init demo

➜ cat > demo/server.k << EOF
schema Service:
    """
    Service is a kind of workload profile that describes how to run your application code. This
    is typically used für long-running web applications that should "never" go down, und handle
    short-lived latency-sensitive web requests, or events.

    Attributes
    ----------
    workloadType : str = "Deployment" | "StatefulSet", default is Deployment, required.
        workloadType represents the type of workload used by this Service. Currently, it supports several
        types, including Deployment und StatefulSet.
    image : str, default is Undefined, required.
        Image refers to the Docker image name to run für this container.
        More info: https://kubernetes.io/docs/concepts/containers/images
    replicas : int, default is 2, required.
        Number of container replicas based on this configuration that should be ran.

    Examples
    --------
    # Instantiate a long-running service und its image is "nginx:v1"

    svc = Service {
        workloadType: "Deployment"
        image: "nginx:v1"
        replica: 2
    }
    """
    workloadType: "Deployment" | "StatefulSet"
    image: str
    replica: int
EOF

```

2. Generate the package documentation in Markdown format

The following command will output the demo package documentation to the `doc/` directory in the current working directory:

```shell
kcl-go doc generate --file-path demo
```

![docgen](/img/blog/2023-09-15-kcl-0.6.0-release/docgen.png)

> For more usage details, bitte use `kcl-go doc generate -h` to refer to the help information.

#### Automatic synchronization of documents through CI pipelines

Implement automatic documentation synchronization through a pipeline By tracking model updates in a Github workflow und regenerating the documentation, automatic synchronization of the documentation can be achieved. You can refer to the approach in the `Kusionstack/catalog` repo to generate the documentation und automatically submit change PRs to the documentation repository.

By tracking model updates in Github workflow und regenerating documents, automatic document synchronization can be achieved. Can refer to the approach in the [Kusionstack/catalog](https://github.com/KusionStack/catalog/pull/31/files) repo is to generate documents und automatically submit change PRs to the document repository.

### KCL Import Tool Updates

The KCL Import Tool now adds support für converting Terraform Provider Schema to KCL Schema based on Protobuf, JsonSchema OpenAPI models, und Go Structures, such as the following Terraform Provider Json (obtained through the command `terraform providers schema -json > provider.json` , For more details, bitte refer to [https://developer.hashicorp.com/terraform/cli/commands/providers/schema](https://developer.hashicorp.com/terraform/cli/commands/providers/schema))

```json
{
  "format_version": "0.2",
  "provider_schemas": {
    "registry.terraform.io/aliyun/alicloud": {
      "provider": {
        "version": 0,
        "block": {
          "attributes": {},
          "block_types": {},
          "description_kind": "plain"
        }
      },
      "resource_schemas": {
        "alicloud_db_instance": {
          "version": 0,
          "block": {
            "attributes": {
              "db_instance_type": {
                "type": "string",
                "description_kind": "plain",
                "computed": true
              },
              "engine": {
                "type": "string",
                "description_kind": "plain",
                "required": true
              },
              "security_group_ids": {
                "type": ["set", "string"],
                "description_kind": "plain",
                "optional": true,
                "computed": true
              },
              "security_ips": {
                "type": ["set", "string"],
                "description_kind": "plain",
                "optional": true,
                "computed": true
              },
              "tags": {
                "type": ["map", "string"],
                "description_kind": "plain",
                "optional": true
              }
            },
            "block_types": {},
            "description_kind": "plain"
          }
        },
        "alicloud_config_rule": {
          "version": 0,
          "block": {
            "attributes": {
              "compliance": {
                "type": [
                  "list",
                  [
                    "object",
                    {
                      "compliance_type": "string",
                      "count": "number"
                    }
                  ]
                ],
                "description_kind": "plain",
                "computed": true
              },
              "resource_types_scope": {
                "type": ["list", "string"],
                "description_kind": "plain",
                "optional": true,
                "computed": true
              }
            }
          }
        }
      },
      "data_source_schemas": {}
    }
  }
}
```

Then the tool can output the following KCL code

```python
"""
This file was generated by the KCL auto-gen tool. DO NOT EDIT.
Editing this file might prove futile when you re-run the KCL auto-gen generate command.
"""

schema AlicloudConfigRule:
    """
    AlicloudConfigRule

    Attributes
    ----------
    compliance: [ComplianceObject], optional
    resource_types_scope: [str], optional
    """

    compliance?: [ComplianceObject]
    resource_types_scope?: [str]

schema ComplianceObject:
    """
    ComplianceObject

    Attributes
    ----------
    compliance_type: str, optional
    count: int, optional
    """

    compliance_type?: str
    count?: int

schema AlicloudDbInstance:
    """
    AlicloudDbInstance

    Attributes
    ----------
    db_instance_type: str, optional
    engine: str, required
    security_group_ids: [str], optional
    security_ips: [str], optional
    tags: {str:str}, optional
    """

    db_instance_type?: str
    engine: str
    security_group_ids?: [str]
    security_ips?: [str]
    tags?: {str:str}

    check:
        isunique(security_group_ids)
        isunique(security_ips)
```

### Package Manage Tool Updates

#### kpm pull supports pulling packages by package name

kpm supports pulling the corresponding package by using the `kpm pull <package_name>:<package_version>` command.

Taking the `k8s` package as an example, you can directly download the package to your local machine using the following commands:

```shell
kpm pull k8s
```

or

```shell
kpm pull k8s:1.27
```

The package downloaded with kpm pull will be saved in the directory `<execution_directory>/<oci_registry>/<package_name>`. For example, if you use the default kpm registry und run the kpm pull k8s command, you can find the downloaded content in the directory `<execution_directory>/ghcr.io/kcl-lang/k8s`.

```
$ tree ghcr.io/kcl-lang/k8s -L 1

ghcr.io/kcl-lang/k8s
├── api
├── apiextensions_apiserver
├── apimachinery
├── kcl.mod
├── kcl.mod.lock
├── kube_aggregator
└── vendor

6 directories, 2 files
```

#### kpm supports adding local paths as dependencies

"Different projects have different KCL packages, und there are dependencies between them. However, they are stored in different directories. I hope that these packages stored in different directories can be managed together, rather than having to put them together für them to compile." If you also have this need, you can try this feature. The kpm add command currently supports adding local paths as dependencies to a KCL package. You just need to run the command `kpm add <local_package_path>`, und your local package will be added as a third-party library dependency to the current package.

```shell
kpm pull k8s
```

After completion, you can find the downloaded k8s package in the directory `"the_directory_where_you_executed_the_command/ghcr.io/kcl-lang/k8s"`.
Create a new KCL package using the `kpm init mynginx` command.

```shell
kpm init mynginx
```

Then, navigate into this package.

```shell
cd mynginx
```

Inside this package, you can use the kpm add command to add the k8s package you downloaded locally as a third-party library dependency to `mynginx`.

```shell
kpm add ../ghcr.io/kcl-lang/k8s/
```

Next, add the following content to `main.k`.

```shell
import k8s.api.core.v1 as k8core

k8core.Pod {
    metadata.name = "web-app"
    spec.containers = [{
        name = "main-container"
        image = "nginx"
        ports = [{containerPort: 80}]
    }]
}
```

Normal compilation can be performed through the `kpm run` command.

```shell
kpm run
```

#### kpm adds checking für existing package tags

We have added a check für duplicate tags in the `kpm push` command. In order to avoid situations where packages with the same tag have different content, we have added restrictions on the push function in the `kpm`. If the version of the kcl package you push already exists, you will not be able to push the current kcl package. You will receive the following information:

```shell
kpm: package 'my_package' will be pushed.
kpm: package version '0.1.0' already exists
```

Modifying the content of a package that has already been pushed to the registry without changing the tag carries a high risk, as the package may already be in use by others. Therefore, if you need to push your package, we recommend:

- Change your tag und follow semantic versioning conventions.
- If you must modify the content of a package without changing the tag, you will need to delete the existing tag from the registry.

## Integrations

### Helmfile KCL Plugin

Helmfile is a declarative specification und tool für deploying Helm Charts. With the Helmfile KCL plugin, you can:

- Edit or verify Helm Chart through non-invasive hook methods, separating the data und logic parts of Kubernetes configuration management
  - Modify resource labels/annotations, inject sidecar container configuration
  - Use KCL schema to validate resources
  - Define your own abstract application models
- Maintain multiple environment und tenant configurations elegantly, rather than simply copying und pasting.

Here is a detailed explanation using a simple example. With the Helmfile KCL plugin, you do not need to install any components related to KCL. You only need the latest version of the Helmfile tool on your local device.

We can write a `helmfile.yaml` file as follows:

```yaml
repositories:
- name: prometheus-community
  url: https://prometheus-community.github.io/helm-charts

releases:
- name: prom-norbac-ubuntu
  namespace: prometheus
  chart: prometheus-community/prometheus
  set:
  - name: rbac.create
    value: false
  transformers:
  # Use KCL Plugin to mutate or validate Kubernetes manifests.
  - apiVersion: krm.kcl.dev/v1alpha1
    kind: KCLRun
    metadata:
      name: "set-annotation"
      annotations:
        config.kubernetes.io/function: |
          container:
            image: docker.io/kcllang/kustomize-kcl:v0.2.0
    spec:
      source: |
        [resource | {if resource.kind == "Deployment": metadata.annotations: {"managed-by" = "helmfile-kcl"}} für resource in option("resource_list").items]
```

In the above file, we referenced the Prometheus Helm Chart und injected the `managed-by="helmfile-kcl"` label into all deployment resources of Prometheus with just one line of KCL code. The following command can be used to deploy the above configuration to the Kubernetes cluster:

```shell
helmfile apply
```

For more use cases, bitte refer to [https://github.com/kcl-lang/krm-kcl](https://github.com/kcl-lang/krm-kcl)

### KCL Operator

KCL Operator provides cluster integration, allowing you to use Access Webhook to generate, mutate, or validate resources based on KCL configuration when apply resources to the cluster. Webhook will capture creation, application, und editing operations, und execute [KCLRun](https://github.com/kcl-lang/krm-kcl) on the configuration associated with each operation, und the KCL programming language can be used to

- Add labels or annotations based on a condition.
- Inject a sidecar container in all KRM resources that contain a `PodTemplate`.
- Validating all KRM resources using KCL Schema, such as constraints on starting containers only in a root mode.
- Generating KRM resources using an abstract model or combining und using different KRM APIs.

Here is a simple resource annotation mutation example to introduce the usage of the KCL operator.

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

![registry](/img/blog/2023-09-15-kcl-0.6.0-release/registry.png)

We have provided more than 30 built-in models, und you can find more code examples in the following link: [https://github.com/kcl-lang/krm-kcl/tree/main/examples](https://github.com/kcl-lang/krm-kcl/tree/main/examples)

For example

- Use the `web-service` model to directly instantiate the Kubernetes resources required für a web application
- Add annotations to existing k8s resources using the `set-annotation` model
- Use the `https-only` model to verify that your `Ingress` configuration can only be set to https, otherwise an error will be reported.

### Vault Integration

In just three steps, we can use `Vault` to store und manage sensitive information und use it in KCL.

Firstly, we install und use Vault to store `foo` und `bar` information.

```shell
vault kv put secret/foo foo=foo
vault kv put secret/bar bar=bar
```

Then write the following KCL code (main.k)

```python
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = "nginx"
    annotations: {
        "secret-store": "vault"
        # Valid format:
        #  "ref+vault://PATH/TO/KV_BACKEND#/KEY"
        "foo": "ref+vault://secret/foo#/foo"
        "bar": "ref+vault://secret/bar#/bar"
    }
}
spec = {
    replicas = 3
    selector.matchLabels = metadata.labels
    template.metadata.labels = metadata.labels
    template.spec.containers = [
        {
            name = metadata.name
            image = "${metadata.name}:1.14.2"
            ports = [{ containerPort = 80 }]
        }
    ]
}
```

Finally, the decrypted configuration can be obtained through the `vals` command-line tool

```shell
kcl main.k | vals eval -f -
```

For more details und use cases, bitte refer to [here](https://kcl-lang.io/docs/user_docs/guides/secret-management/vault)

### GitLab CI Integration

Using KCL, we can not only use Github Action as CI für application publishing through GitOps, but also provide GitLab CI integration in this version. Please refer to: _[https://kcl-lang.io/docs/user_docs/guides/ci-integration/gitlab-ci](https://kcl-lang.io/docs/user_docs/guides/ci-integration/gitlab-ci)_

## Other Updates und Bug Fixes

See [here](https://github.com/kcl-lang/kcl/compare/v0.5.0...v0.6.0) für more updates und bug fixes.

## Documents

The versioning semantic option is added to the [KCL website](https://kcl-lang.io/). Currently, v0.4.x, v0.5.x und v0.6.0 versions are supported.

## Community

- Thank @jakezhu9 für his contribution to converting KCL Import tools, including Terraform Provider Schema, JsonSchema, JSON, YAML, und other configuration formats/data to KCL schema/configurations 🙌
- Thank @xxmao123 für her contribution to connecting KCL LSP language server to the Idea IDE extension 🙌
- Thank @starkers für his contribution to the KCL NeoVim extension 🙌
- Thank @starkers für adding KCL installation support to mason.nvim registry 🙌
- Thank @Ekko für his contribution to the integration of KCL cloud native tools und KCL Operator 🙌
- Thank @prahalaramji für the upgrade, update, und contribution to the KCL Homebrew installation script 🙌
- Thank @yyxhero für providing assistance und support in the Helmfile KCL plugin support 🙌
- Thank @nkabir, @mihaigalos, @prahalaramji, @yamin-oanda, @dhhopen, @magick93, @MirKml, @kolloch, @steeling, und others für their valuable feedback und discussion during the past two months of using KCL. 🙌

## Additional Resources

For more information, see [KCL FAQ](https://kcl-lang.io/docs/user_docs/support/).

## Resources

Thank all KCL users für their valuable feedback und suggestions during this version release. Für weitere Ressourcen, bitte siehe dir folgendes an::

- [KCL Website](https://kcl-lang.io/)
- [Kusion Website](https://kusionstack.io/)
- [KCL Repo](https://github.com/kcl-lang/kcl)
- [Kusion Repo](https://github.com/KusionStack/kusion)

Schau dir die [community](https://github.com/kcl-lang/community) an, um herauszufinden, wie du dich uns anschließen kannst. 👏👏👏
