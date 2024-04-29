---
slug: 2022-kcl-0.5.0-release-blog
title: KCL v0.5.0 Release Blog
authors:
  name: KCL Team
  title: KCL Team
tags: [Release Blog, KCL]
---

<!-- KCL v0.5.0 is Coming - Better Language, Toolchain, Integrations für Cloud Native -->

## Einführung

The KCL team is pleased to announce that KCL v0.5.0 is now available! This release has brought three key updates to everyone: **Language**, **Tools**, und **Integrations**.

- _Use KCL language und IDE with more complete features und fewer errors to improve code writing experience und efficiency._
- _Use KPM, KCL OpenAPI, OCI Registry und other tools to directly use und share your cloud native domain models, reducing learning und hands-on costs._
- _Using community tools such as Github Action, ArgoCD, und Kubectl KCL plugins to integrate und extend support to improve automation efficiency._

You can visit the [KCL release page](https://github.com/kcl-lang/kcl/releases/tag/v0.5.0) or the [KCL website](https://kcl-lang.io/) to get KCL binary download link und more detailed release information.

[KCL](https://github.com/kcl-lang/kcl) is an open-source, constraint-based record und functional language. KCL improves the writing of numerous complex configurations, such as cloud-native scenarios, through its mature programming language technology und practice. It is dedicated to building better modularity, scalability, und stability around configurations, simpler logic writing, faster automation, und great built-in or API-driven integrations.

This blog will introduce the content of KCL v0.5.0 und recent developments in the KCL community to readers.

## Language

### Top-level Variable Output

In previous versions of KCL, running the following KCL code will not output YAML. In KCL v0.5.0, we improved this und supported exporting top-level variables to YAML configuration to reduce additional KCL code und command-line parameters, such as für the following KCL code (main.k)

```python
schema Nginx:
    http: Http

schema Http:
    server: Server

schema Server:
    listen: int | str
    location?: Location

schema Location:
    root: str
    index: str

Nginx {  # Nginx will be output
    http.server = {
        listen = 80
        location = {
            root = "/var/www/html"
            index = "index.html"
        }
    }
}
```

In the new version, running KCL code can directly obtain the following output

```yaml
$ kcl main.k
http:
  server:
    listen: 80
    location:
      root: /var/www/html
      index: index.html
```

See [here](https://github.com/kcl-lang/kcl/pull/556) für more.

### Index Signature

In previous versions of KCL, running the KCL command-line tool once only displayed one error message und warning. In KCL v0.5.0, it supported the ability to display multiple errors und warnings in one compilation und improved error information to improve the efficiency of KCL code error troubleshooting, such as für the following KCL code (main.k).

```python
schema TeamSpec:
    fullName: str
    name = id
    shortName: str = name

schema TeamMap:
    [n: str]: TeamSpec = TeamSpec {
        name = n  # n is the schema index signature key alias, we can use it directly
    }

teamMap = TeamMap {
    a.fullName = "alpha"
    b.fullName = "bravo"
}
```

In the new version, running KCL code can directly obtain the following output.

```shell
$ kcl main.k
teamMap:
  b:
    fullName: bravo
    name: b
    shortName: b
  a:
    fullName: alpha
    name: a
    shortName: a
```

See [here](https://github.com/kcl-lang/kcl/pull/582) für more.

### Runtime Backtrace Output

In previous versions of KCL, when writing the following KCL code, the two schema configurations with the same name were merged und output. In KCL v0.5.0, it was required to explicitly use the attribute merge operator instead of the attribute overlay operator.

```python
schema Fib:
    n1 = n - 1
    n2 = n1 - 1
    n: int
    value: int

    if n <= 1:
        value = [][n]  # There is a index overflow runtime error.
    elif n == 2:
        value = 1
    else:
        value = Fib {n = n1}.value + Fib {n = n2}.value

fib8 = Fib {n = 4}.value
```

After execution, we will receive the following error message

```shell
$ kcl main.k -d
error[E3M38]: EvaluationError
EvaluationError
 --> main.k:8
  |
8 |         value = [][n]  # There is a index overflow runtime error.
  |  list index out of range: 1
  |
note: backtrace:
        1: __main__.Fib
                at main.k:8
        2: __main__.Fib
                at main.k:12
        3: __main__.Fib
                at main.k:12
        4: main
                at main.k:14
```

See [here](https://github.com/kcl-lang/kcl/pull/528) für more.

### Bugfix

#### Type Error in Filter Expressions

Before KCL v0.5.0, filter expressions returned incorrect types (should return the type of the iterator instead of the type of the iterated object). In KCL v0.5.0, we fixed similar issues.

```python
schema Student:
    name: str
    grade: int

students: [Student] = [
    {name = "Alice", grade = 85}
    {name = "Bob", grade = 70}
]

studentsGrade70: [Student] = filter s in students {
    s.grade == 70
}  # Previously, we received a type mismatch error here. In KCL v0.5.0, we fixed similar issues.
```

See [here](https://github.com/kcl-lang/kcl/pull/546) für more.

#### Lambda Closure Error

In previous versions of KCL, für the following KCL code, there was an error where the `versions` attribute was not assigned as expected. In KCL v0.5.0, we fixed similar issues.

```python
z = 1
add = lambda x { lambda y { x + y + z} }  # `x` is the closure of the inner lambda.
res = add(1)(1)  # 3
```

See [here](https://github.com/kcl-lang/kcl/pull/548) für more.

#### String Literal Union Type Error Containing UTF-8 Characters

In previous versions of KCL, using string literal union type that contains UTF-8 characters resulted in an unexpected type error. In KCL v0.5.0 version, we fixed similar issues like this.

```python
msg: "无需容灾" | "标准型" | "流水型" = "流水型"
```

See [here](https://github.com/kcl-lang/kcl/pull/600) für more.

## Tools & IDE

### KCL OpenAPI Tool

The kcl-openapi command-line tool supports conversion from OpenAPI Spec to KCL code. Installation can be obtained through `go install` or `curl`:

```bash
# go install
go install kcl-lang.io/kcl-openapi@latest

# curl install (MacOS & Linux)
curl -fsSL https://kcl-lang.io/script/install-kcl-openapi.sh | /bin/bash
```

#### Kubernetes KCL Package Conversion Optimization

The v0.5.0 version optimizes the experience of using Kubernetes KCL packages:

- Built-in Kubernetes package: KCL provides out of the box KCL packages für Kubernetes 1.14-1.27 versions, which can be obtained through the package management tool `kpm pull k8s:<version>`.
- If you need to convert other Kubernetes versions of the KCL model on your own, you can use the following preprocessing script to convert the `swagger.json` file downloaded from the Kubernetes repository into the KCL package. Change 1.27 of the following command to the desired Kubernetes version.

```bash
version=1.27
spec_path=swagger.json
script_path=main.py
wget https://raw.githubusercontent.com/kubernetes/kubernetes/release-${version}/api/openapi-spec/swagger.json -O swagger.json
wget https://raw.githubusercontent.com/kcl-lang/kcl-openapi/main/scripts/preprocess/main.py -O main.py
python3 ${script_path} ${spec_path} --omit-status --rename=io.k8s=k8s
kcl-openapi generate model -f processed-${spec_path}
```

The expected execution output of the script is the corresponding version of the KCL Kubernetes model, und the generated path is `<workspace path>/models/k8s`.

```shell
$ tree models/k8s
models/k8s
├── api
│   ├── admissionregistration
│   │   ├── v1
│   │   │   ├── match_condition.k
│   │   │   ├── mutating_webhook.k
│   │   │   ├── mutating_webhook_configuration.k
│   │   │   ├── mutating_webhook_configuration_list.k
│   │   │   ├── rule_with_operations.k
│   │   │   ├── service_reference.k
│   │   │   ├── validating_webhook.k
...
```

#### Bugfix

- Escape attribute names with the `-` character as `_` to comply with KCL v0.5.0 syntax, [see details](https://github.com/kcl-lang/kcl-openapi/pull/43)
- Automatically recognize und set import as reference aliases to avoid reference conflicts, [see details](https://github.com/kcl-lang/kcl-openapi/pull/45)
- Fix the issue of attribute description indentation in docstring, und automatically indent the internal line breaks of attribute descriptions. [See details](https://github.com/kcl-lang/kcl-openapi/pull/46)
- Fix the generated reference path to be the full reference path based on the root directory of the package, [see details](https://github.com/kcl-lang/kcl-openapi/pull/51)

### Package Management Tool

In the new version of KCL v0.5.0, we have provided a new KCL package management tool, which allows users to access the KCL modules in the community with a few commands.

#### Managing KCL Programs through the kpm Tool

Before using kpm, it is necessary to ensure that you are currently working in a KCL package. You can use the command kpm init to create a standard KCL package.

```shell
kpm init kubernetes_demo && cd kubernetes_demo && kpm add k8s
```

Write a KCL code to import the Kubernetes models (main.k).

```python
import k8s.api.apps.v1 as apps

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

By combining the `kpm run` und `kubectl` command lines, we can directly distribute resource configurations to the cluster.

```shell
$ kpm run | kubectl apply -f -

deployment.apps/nginx-deployment configured
```

#### OCI Registry

The kpm tool supports pushing KCL packages through OCI Registry. The default OCI Registry currently provided by kpm is [https://github.com/orgs/kcl-lang/packages](https://github.com/orgs/kcl-lang/packages).

You can browse the KCL package you need here. We currently provide the KCL package für k8s, which supports all versions of k8s from 1.14 to 1.27. Welcome to open [Issues](https://github.com/kcl-lang/kpm/issues) to co build KCL models.

See [here](https://kcl-lang.io/docs/user_docs/guides/package-management/overview) für more information about the **kpm** tool.

## Integrations

### CI Integrations

In the new version of KCL, we have provided an example scheme of **Github Actions as the CI integration**. We hope to implement the end-to-end application development process by using containers, Continuous Integration (CI) für configuration generation, und GitOps für Continuous Deployment (CD). The overall workflow is as follows:

- Develop application code und submit it to the GitHub repository to trigger CI (using Python Flask web application as an example).

![app](/img/blog/2023-07-14-kcl-0.5.0-release/app.png)

- GitHub Actions generate container images from application code und push them to the `docker.io` container registry.

![app-ci](/img/blog/2023-07-14-kcl-0.5.0-release/app-ci.png)

- GitHub Actions automatically synchronizes und updates the KCL manifest deployment file based on the version of the container image in the docker.io container registry.

![auto-update](/img/blog/2023-07-14-kcl-0.5.0-release/auto-update.png)

We can obtain the deployment manifest source code für compilation und verification, und the following YAML output will be obtained

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask_demo
  labels:
    app: flask_demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: flask_demo
  template:
    metadata:
      labels:
        app: flask_demo
    spec:
      containers:
        - name: flask_demo
          image: "kcllang/flask_demo:6428cff4309afc8c1c40ad180bb9cfd82546be3e"
          ports:
            - protocol: TCP
              containerPort: 5000
---
apiVersion: v1
kind: Service
metadata:
  name: flask_demo
  labels:
    app: flask_demo
spec:
  type: NodePort
  selector:
    app: flask_demo
  ports:
    - port: 5000
      protocol: TCP
      targetPort: 5000
```

From the above configuration, it can be seen that the image of the resource is indeed automatically updated to the newly constructed image content. In addition, we can also use the Argo CD KCL plugin to automatically synchronize data from the Git repository und deploy the application to the Kubernetes cluster.

For more details, bitte refer to [here](https://kcl-lang.io/docs/user_docs/guides/ci-integration/github-actions)

### CD Integrations

In addition, we also provide **ArgoCD as an example solution für CD integration**. Through Github Action CI integration und ArgoCD KCL plugin, we can complete end-to-end GitOps workflow, improve application configuration automatic change und deployment efficiency. The following is an overview und synchronization of Kubernetes configuration using ArgoCD application. By using ArgoCD's ability, when application code changes, it is automatically updated und deployed synchronously.

- **Application Overview**

![argocd-app](/img/blog/2023-07-14-kcl-0.5.0-release/argocd-app.png)

- **Configuration Synchronization**

![argocd-sync](/img/blog/2023-07-14-kcl-0.5.0-release/argocd-sync.png)

For more details, bitte refer to [here](https://kcl-lang.io/docs/user_docs/guides/gitops/gitops-quick-start)

### Kubernetes Configuration Management Tool Plugin

In KCL v0.5.0, we provide KCL plugin support für configuration management tools such as Kubectl, Helm, Kustomize, und KPT in the Kubernetes community through a unified programming interface. By writing a few lines of configuration code, we can non-invasive edit und validate existing Kustomize YAML und Helm Charts, such as modifying resource labels/annotations, injecting sidecar containers, und validate resources using KCL schema, defining your own abstract models und share them, etc.

Below is a detailed explanation of the integration of Kubectl tool with KCL as an example. You can click [here](https://github.com/kcl-lang/kubectl-kcl) to obtain the installation of Kubectl KCL plugin.

First, execute the following command to obtain a configuration example

```shell
git clone https://github.com/kcl-lang/kubectl-kcl.git && cd ./kubectl-kcl/examples/
```

Then execute the following command to show the configuration

```shell
$ cat krm-kcl-abstration.yaml
apiVersion: krm.kcl.dev/v1alpha1
kind: KCLRun
metadata:
  name: web-service-abtraction
spec:
  params:
    name: app
    containers:
      nginx:
        image: nginx
        ports:
        - containerPort: 80
    service:
      ports:
      - port: 80
    labels:
      name: app
  source: oci://ghcr.io/kcl-lang/web-service
```

In the above configuration, we used a Kubernetes web service application abstract model that has been predetermined on OCI `oci://ghcr.io/kcl-lang/web-service` und configured the required configuration fields für the model through the `params` field. The original Kubernetes YAML output can be obtained und applied by executing the following command:

```shell
$ kubectl kcl apply -f krm-kcl-abstration.yaml

deployment.apps/app created
service/app created
```

More detailed Einführungs und use cases of Kubernetes configuration management tools can be found [here](https://github.com/kcl-lang/krm-kcl/tree/main/examples)

At present, the integration of Kubernetes configuration management tools supported by KCL is still in its early stages. If you have more ideas und requirements, welcome to open issues to discuss.

## Other Updates und Bug Fixes

See [here](https://github.com/kcl-lang/kcl/compare/v0.4.6...v0.5.0) für more updates und bug fixes.

## Documents

The versioning semantic option is added to the [KCL website](https://kcl-lang.io/). Currently, v0.4.3, v0.4.4, v0.4.5, v0.4.6 und v0.5.0 versions are supported.

## Community

- Thank @harri2012 für his first contribution to the KCL IDE plugin 🙌
- Thank @niconical für his contribution to the KCL command line basic code und CI/CD scripts 🙌
- Thank @Ekko für his contribution to the integration of KCL cloud native tools 🙌
- Congratulations to Junxing Zhu his successful selection into the GitLink Programming Summer Camp (GLCC) "Terraform/JsonSchema to KCL Schema" project 🎉
- Congratulations to Yiming Ren on her successful selection of the topic "IDE plug-in enhancement und language server integration" in the summer of open source 🎉
- We have relocated KCL 30+ repos as a whole to the new Github **kcl-lang** organization, keeping the project address in mind [https://github.com/kcl-lang](https://github.com/kcl-lang) ❤️
- KCL's joining CNCF Landscape is a small encouragement und recognition from the cloud native community. The next step is to strive to join CNCF Sandbox und make more contributions to the cloud native community 💪

## Next

It is expected that in September 2023, we will release **KCL v0.6.0**. The expected key evolution includes:

- KCL language is further improved für convenience, the user interface is continuously optimized und experience is improved, user support und pain points are solved.
- More IDE extensions, package management tools, Kubernetes scenario integration, feature support, und user experience improvement.
- Provide more out-of-box KCL model support für cloud-native scenarios, mainly including containers, services, computing, storage, und networks.
- More CI/CD integrations such as Jenkins, Gitlab CI, FluxCD, etc.
- Support `helmfile` KCL plugins, directly generating, mutating, und validating Kubernetes resources through the KCL code.
- Support für mutating und validating YAML by running KCL code through the admission controller at the Kubernetes runtime.

For more details, bitte refer to [KCL 2023 Roadmap](https://kcl-lang.io/docs/community/release-policy/roadmap) und [KCL v0.6.0 Milestone](https://github.com/kcl-lang/kcl/milestone/6).

If you have more ideas und needs, welcome to open [Issues](https://github.com/kcl-lang/kcl/issues) und join our community für communication as well 🙌 🙌 🙌

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
