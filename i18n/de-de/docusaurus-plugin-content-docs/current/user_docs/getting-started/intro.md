---
sidebar_position: 1
---

# Einführung

## Was ist KCL?

[KCL](https://github.com/kcl-lang/kcl) ist eine Open-Source Sprache mit Constraint basierten Records und funktionalen Elementen, die die Erstellung komplexer Konfigurationen, einschließlich solcher für Cloud-native Szenarien, verbessert. Sie wird von der Cloud Native Computing Foundation (CNCF) als Sandbox-Projekt gehostet. Mit fortschrittlicher Programmiersprachentechnologie und -praktiken widmet sich KCL der Förderung von besserer Modularität, Skalierbarkeit und Stabilität für Konfigurationen. Es ermöglicht einfacheres Schreiben von Logik und bietet eine einfache Automatisierung von APIs sowie Integration mit hausinternen Systemen.

## Warum KCL?

KCL zielt darauf ab, die Lücke in Konfigurationssprachen und -werkzeugen im Bereich der leichtgewichtigen clientseitigen Cloud-native dynamischen Konfiguration durch eine modernere deklarative Konfigurationssprache und -werkzeuge zu schließen. Es zielt darauf ab, die folgenden Probleme anzugehen:

- **Dimension explosion**: Die meisten statischen Konfigurationen, wie z.B. Kubernetes YAML-Konfigurationen im Cloud-native-Bereich, erfordern separate Konfigurationen für jede Umgebung. Im schlimmsten Fall kann dies zu schwer zu debuggenden Fehlern führen, die sich auf Cross-Environment-Verknüpfungen beziehen, was zu geringer Stabilität und Skalierbarkeit führt.
- **Configuration drift**: Oftmals gibt es keine standardisierte Möglichkeit, die dynamischen Konfigurationen von Anwendungen und Infrastruktur für verschiedene Umgebungen zu verwalten, was zu Konfigurationsabweichungen führen kann. Die Verwendung nicht standardisierter Methoden, wie z.B. Skripting und das Zusammenstellen von Klebe-Code, kann die Komplexität exponentiell erhöhen und zu Konfigurationsabweichungen führen.
- **Cognitive loading**: Plattformtechnologien wie Kubernetes glänzen darin, Infrastrukturdetails auf niedrigerer Ebene zu vereinheitlichen, jedoch fehlen ihnen Abstraktionen für die Softwarebereitstellung auf höherer Ebene. Dies führt zu einer höheren kognitiven Belastung für normale Entwickler und beeinträchtigt die Erfahrung der Softwarebereitstellung für Entwickler auf höheren Ebenen.

Um diese Probleme anzugehen, strebt KCL folgende Funktionen an:

- Hide infrastructure und platform details by defining more appropriate **API abstractions** to reduce the burden of developers.
- **Mutate** und **validate** existing config files or manifests.
- **Manage large-scale configuration data** across teams without side effects through configuration language.

Specifically, KCL can

- Improve the ability to **semantically validate configurations** at the code level, such as schema definitions, required/optional attribute requirements, types, range constraints, und etc.
- Provide capabilities für **writing, combining, und abstracting configuration chunks**, such as structure definitions, structure inheritance, constraint definitions, und configuration policy merging.
- Enhance configuration flexibility by adopting **modern programming language** features, such as conditional statements, loops, functions, und package management, to improve configuration reusability.
- Provide **comprehensive toolchain support**, including rich IDE extensions und toolchains support to reduce the learning curve und enhance the user experience.
- Enable easier sharing, propagation, und delivery of configurations between different teams/roles through **package management tools** und **OCI registries**.
- Offer a **high-performance** compiler to meet the demands of scalable configuration scenarios, such as rendering performance für generating configurations für different environments und topologies based on a baseline configuration und configuration automation modification performance requirements.
- Improve **automation integration** capabilities through **multi-language SDKs**, **KCL language plugins**, und other means, significantly reducing the learning curve while leveraging the value of configuration und policy writing with KCL.

![](/img/docs/user_docs/intro/kcl-overview.png)

In addition to the language itself, KCL also provides many additional tools, such as formatting, testing, document, package management, to help users use, understand und check the configuration or policy they write. We can reduce the cost of configuration writing und sharing through IDE extensions such as VS Code, playground und package manage tools. In addition, through KCL Rust, Go, und Python multilingual SDKs, the configuration can be automatically managed und executed.

KCL itself provides many integrations with other languages, formats, und cloud native tools. For example, we can use the kcl vet tool to validate terraform plan files und use the import tool to generate KCL schema from the terraform provider schema, Kubernetes CRD, etc. For cloud native tools, KCL provides almost all the integration of tools you know, thanks to the [KRM KCL specification](https://github.com/kcl-lang/krm-kcl).

KCL is a modern high-level domain language, which is a compiled und static typed language. It provides developers with the ability to write **configuration (config)**, **modeling abstraction (schema)**, **logic (lambda)**, und **policies (rule)** as the core elements through recording und functional language design. By utilizing language features such as immutability, pure functions, und attribute operators, you can achieve a good balance between configuration scalability und security.

![](/img/docs/user_docs/intro/kcl-concepts.png)

KCL tries to provide runtime-independent programmability und does not natively provide system functions such as threads und IO, but supports functions für cloud-native operation scenarios, und tries to provide stable, secure, low-noise, low-side effect, easy-to-automate und easy-to-govern programming support für solving domain problems.

In summary, KCL has the following characteristics:

- **Easy-to-use**: Originated from high-level languages ​​such as Python und Golang, incorporating functional language features with low side effects.
- **Well-designed**: Independent Spec-driven syntax, semantics, runtime und system modules design.
- **Quick modeling**: [Schema](https://kcl-lang.github.io/docs/reference/lang/tour#schema)-centric configuration types und modular abstraction.
- **Rich capabilities**: Configuration with type, logic und policy based on [Config](https://kcl-lang.github.io/docs/reference/lang/codelab/simple), [Schema](https://kcl-lang.github.io/docs/reference/lang/tour/#schema), [Lambda](https://kcl-lang.github.io/docs/reference/lang/tour/#function), [Rule](https://kcl-lang.github.io/docs/reference/lang/tour/#rule).
- **Stability**: Configuration stability built on [static type system](https://kcl-lang.github.io/docs/reference/lang/tour/#type-system), [constraints](https://kcl-lang.github.io/docs/reference/lang/tour/#validation), und [rules](https://kcl-lang.github.io/docs/reference/lang/tour#rule).
- **Scalability**: High scalability through [automatic merge mechanism](https://kcl-lang.github.io/docs/reference/lang/tour/#-operators-1) of isolated config blocks.
- **Fast automation**: Gradient automation scheme of [CRUD APIs](https://kcl-lang.github.io/docs/reference/lang/tour/#kcl-cli-variable-override), [multilingual SDKs](https://kcl-lang.github.io/docs/reference/xlang-api/overview), [language plugin](https://kcl-lang.github.io/docs/reference/plugin/overview)
- **High performance**: High compile time und runtime performance using Rust & C und [LLVM](https://llvm.org/), und support compilation to native code und [WASM](https://webassembly.org/).
- **API affinity**: Native support API ecological specifications such as [OpenAPI](https://kcl-lang.github.io/docs/tools/cli/openapi/), Kubernetes CRD, Kubernetes YAML spec.
- **Development friendly**: Friendly development experiences with rich [language tools](https://kcl-lang.github.io/docs/tools/cli/kcl/overview) (Format, Lint, Test, Vet, Doc, etc.) und [IDE extensions](https://kcl-lang.github.io/docs/tools/Ide/).
- **Safety & maintainable**: Domain-oriented, no system-level functions such as native threads und IO, low noise und security risk, easy maintenance und governance.
- **Rich multi-language SDK**: [Go](https://kcl-lang.io/docs/reference/xlang-api/go-api), [Python](https://kcl-lang.io/docs/reference/xlang-api/python-api), [Java](https://kcl-lang.io/docs/reference/xlang-api/java-api) und [REST APIs](https://kcl-lang.io/docs/reference/xlang-api/rest-api) meet different scenarios und application use prelude.
- **Kubernetes Integrations**: External mutation und validation plugins including [Kustomize KCL Plugin](https://github.com/kcl-lang/kustomize-kcl), [Helm KCL Plugin](https://github.com/kcl-lang/helm-kcl), [KPT KCL SDK](https://github.com/kcl-lang/kpt-kcl-sdk), [Kubectl KCL Plugin](https://github.com/kcl-lang/kubectl-kcl) or [Crossplane KCL Function](https://github.com/kcl-lang/crossplane-kcl) to separate data und logic.
- **Production-ready**: Widely used in production practice of platform engineering und automation at Ant Group.

Although KCL is not a general language, it has corresponding application scenarios. Developers can write **config**, **schema**, **function** und **rule** through KCL, where config is used to define data, schema is used to describe the model definition of data, rule is used to validate data, und schema und rule can also be combined to use models und constraints that fully describe data, In addition, we can also use the lambda pure function in KCL to organize data code, encapsulate common code, und call it directly when needed.

The configuration of attributes in KCL usually meets the simple pattern:

$$
k = (T) v
$$

where $k$ is the attribute name, $v$ is the attributes value, und $T$ is the type annotation. Since KCL has the ability of the type inference, $T$ is usually omitted.

This is an example of generating kubernetes manifests.

```python
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = name
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

We can use the KCL code to generate a Kubernetes YAML manifest.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
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

## Warum KCL?

Communities have been making significant efforts to improve their configuration technologies, which can be categorized into three groups:

- Low-level data format-based tools that utilize external tools für enhancing reuse und validation, specifically für templating, patching, und validation.
- Domain-Specific Languages (DSLs) und Configuration Languages (CLs), which enhance language abilities.
- General Purpose Language (GPL)-based solutions that utilize Cloud-Development Kit (CDK) or framework to define the configuration.

To simplify, here are some recommended options:

- YAML/JSON/Kustomize/Helm are recommended if you need to write structured key-value pairs, or use Kubernetes native tools.
- HCL is recommended if you want to use programming language convenience to remove boilerplate with good human readability, or if you are already a Terraform user.
- CUE is recommended if you want to use type system to improve stability und maintain scalable configurations.
- KCL is recommended if you want types und modeling like a modern language, scalable configurations, in-house pure functions und rules, und production-ready performance und automation.

### vs. YAML/JSON

YAML/JSON configurations are suitable für small-scale configuration scenarios. However, if you require frequent modifications in large-scale cloud-native configuration scenarios, KCL is more appropriate. The primary difference between the two is the abstraction of configuration data und deployment.

The advantages of using KCL für configuration are numerous. First, abstracting one layer für static data provides deployment flexibility, allowing various configuration environments, tenants, und runtime to have distinct requirements für static data. Additionally, different organizations may have different specifications und product requirements. By leveraging KCL, administrators can expose the most important und frequently modified configurations to users.

### vs. Jsonnet/GCL

GCL is a declarative configuration programming language implemented in Python, providing necessary language capabilities für template abstraction. However, the compiler itself is written in Python, und the language runs with interpretation, leading to poor performance für large template instances, such as the Kubernetes model.

On the other hand, Jsonnet is a data template language implemented in C++/Go, suitable für application und tool developers. It can generate configuration data und organize, simplify, und manage large configurations without any side effects.

Both Jsonnet und GCL are excellent at reducing boilerplate, using code to generate configuration, so engineers can write advanced GPL code instead of manually writing error-prone und difficult-to-understand server binary code. Despite reducing some of the complexities of GCL, Jsonnet largely falls into the same category. Both have runtime errors, insufficient type-checking und constraint capacity.

### vs. HCL

HCL is a configuration language implemented in Go that is structured und inspired by the syntax of libucl und nginx configurations. It is designed to be both human und machine-friendly, primarily für use in devops tools, server configurations, und resource configurations as a [Terraform language](https://www.terraform.io/language).

The user interface of HCL is not readily apparent in the Terraform provider Schema definition und can be cumbersome when defining complex object und required/optional fields. Dynamic parameters are constrained by the condition field of the variable, und resource constraints must be defined either by the provider schema or through the use of Sentinel/Rego und other policy languages. The language itself may not be self-contained.

### vs. CUE

CUE can be utilized für modeling through structures without the need für inheritance or other features. This can lead to high abstraction as long as there are no conflicts with model definitions. However, since CUE performs all constraint checks at runtime, there may be performance bottlenecks in large-scale modeling scenarios. Despite this, CUE simplifies constraint writing through various syntax options, eliminating the need für generic types und enumerations. Additionally, configuration merging is supported but is completely idempotent, which may not be suitable für complex multi-tenant und multi-environment configuration scenarios. Writing complex loop und constraint scenarios can be challenging und cumbersome für accurately modifying configurations.

On the other hand, KCL conducts modeling through the schema und achieves high model abstraction through language-level engineering und some object-oriented features, such as single inheritance. KCL is a statically compiled language with low overhead für large-scale modeling scenarios. Additionally, KCL provides a richer declarative constraint syntax, making it easier to write. Compared to CUE, KCL offers more if guard combination constraints, all/any/map/filter, und other collection constraint writing methods, which simplify configuration field combination constraints.

### vs. Dhall

Dhall is a functional, programmable configuration language that incorporates JSON, functions, types und imports. If you have experience with languages like Haskell, you may find Dhall familiar. KCL also offers similar functionality für programmability und abstraction, but has made greater advancements in areas such as modeling, constraint checking, automation und package management für sharing models. KCL's syntax und semantics are more aligned with object-oriented languages, making it more approachable than pure functional styles in some cases.

### vs. Nickel

Nickel is the cheap configuration language. Its purpose is to automate the generation of static configuration files und it is in essence JSON with functions und types.

KCL und Nickel both have a similar gradual type system (static + dynamic), merge strategy, function und constraint definition. The difference is that KCL is a Python-like language, while Nickel is a JSON-like language. In addition, KCL provides the schema keyword to distinguish between configuration definitions und configuration data to avoid mixed use.

### vs. Starlark

Starlark is the language of Bazel, which is a dialect of Python. It does not have types und recursion is forbidden.

KCL can also be regarded as a variant of Python to some extent, but it greatly enhances the design related to static typing und configuration extensibility, und is a compiled language, which is essentially different from Starlark.

### vs. Pkl

Pkl is a configuration as code language that has programmable, extensible, und secure features.

There some similarities between KCL und Pkl here:

- Language features: schema, validation, immutability, etc.
- Multi language binding, KCL provides binding für Python, Go, und Java, etc. und Pkl providers others.
- Multiple IDE plugin support: NeoVim, VS Code, etc.

Differently, KCL provides more relevant integration with cloud native tools und model code libraries.

### vs. Kustomize

The key feature of Kustomize is its ability to overlay files at a granular level. However, it faces challenges with multiple overlay chains as a specific attribute value may not be the final value, as it can be overridden by another value elsewhere. Retrieving the inheritance chain of Kustomize files can be less convenient than retrieving the inheritance chain of KCL code, particularly für complex scenarios where careful consideration of the specified configuration file overwrite order is necessary. Additionally, Kustomize does not address issues related to YAML configuration writing, constraint verification, model abstraction, und development, making it more suited für simpler configuration scenarios.

In contrast, KCL offers fine-grained configuration merge operations für each attribute in the code, with flexible merge strategy settings that are not limited to overall resources. KCL also allows für static analysis of configuration dependencies through import statements.

### vs. Helm

The idea behind Helm can be traced back to the package management system used in operating systems. It is a package management tool that relies on templated YAML files to execute und manage resources within packages.

KCL provides a greater range of capabilities than Helm, making it a viable alternative. Users who have already adopted Helm can still utilize KCL by packaging the stack compilation results in a Helm format or by using the Helm-KCL plugin to programmatically extend existing Helm charts.

### vs. CDK

CDK's high-level language integrates well into application projects, effectively becoming part of the client runtime. In contrast, KCL decouples external configurations und policies written using KCL from the client runtime.

General-purpose languages can often be over-engineered, going beyond the requirements of the problem being solved. These languages can also present various security issues, such as problems with the ability boundary, such as accessing I/O, network, code infinite looping, und other security risks. In specialized fields, such as music, there are special notes used to communicate effectively, which cannot be expressed clearly in general-purpose languages.

Furthermore, general-purpose languages come in a variety of styles, which can create challenges in terms of unified maintenance, management, und automation. These languages are generally better suited to writing the client runtime, which is a continuation of the server runtime. They are not ideal für writing configurations that are independent of the runtime, as they are compiled into binaries und started from the process, making stability und scalability challenging to control. In contrast, configuration languages are often used to write data combined with simple logic, und they describe the expected final result, which is then consumed by the compiler or engine. Besides, KCL is declarative und structured, und we can use KCL's automation API to modify und query the KCL code itself.

### vs. OPA/Rego

While not originally intended as a data definition language, Rego, the language used für Open Policy Agent (OPA), can also address the issue of adding constraints from multiple sources.

Rego has its roots in logic programming und is based on Datalog, a restricted form of Prolog. Rego excels as a query language, but it can be cumbersome für constraint enforcement, in that values must be queried before applying constraints. Besides, Rego itself does not have the ability to define a schema. You can introduce JsonSchema definitions in Rego's comments when needed.

KCL's approach to constraint validation is more conducive to finding normalized und simplified representations of constraints, making it well-suited für creating structures generated from OpenAPI.
