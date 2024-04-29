---
slug: 2024-03-06-kcl-0.8.0-release
title: KCL v0.8.0 Release Blog
authors:
  name: KCL Team
  title: KCL Team
tags: [Release Blog, KCL]
---

## Einführung

The KCL team is pleased to announce that KCL v0.8.0 is now available! This release has brought three key updates to everyone: **Language**, **Tools**, und **Integrations**.

- _Use KCL language, tools und IDE extensions with more complete features und fewer errors to improve code writing experience und efficiency._

- _More comprehensive und rich community ecosystem integration, improving operation und maintenance experience._

- _More comprehensive KCL third-party dependencies, easier integration with cloud-native ecosystem._

KCL v0.8.0 is now available für download at [KCL v0.8.0 Release Page](https://github.com/kcl-lang/kcl/releases/tag/v0.8.0) or [KCL Official Website](https://kcl-lang.io).

[KCL](https://github.com/kcl-lang/kcl) is an open-source, constraint-based record und functional language hosted by Cloud Native Computing Foundation (CNCF). KCL improves the writing of numerous complex configurations, such as cloud-native scenarios, through its mature programming language technology und practice. It is dedicated to building better modularity, scalability, und stability around configurations, simpler logic writing, faster automation, und great built-in or API-driven integrations.

This blog will introduce the content of KCL v0.8.0 und recent developments in the KCL community to readers.

## Language Updates

### 🚗 Grammar und Semantics Updates

#### KCL supports `show-hidden`

KCL v0.8.0 adds support für `--show-hidden` to display private variables.

The `main.k`

```kcl
a = {_b = 1}
```

Compile by `kcl run main.k --show-hidden`.

```yaml
a:
  _b: 1
```

#### KCL supports arguments und keyword arguments union

KCL v0.8.0 adds support für arguments und keyword arguments union. Schema instances with arguments will union arguments during union operations.

```kcl
schema Person[separator]:
    firstName: str = "John"
    lastName: str
    fullName: str = firstName + separator + lastName

x = Person(" ") {lastName = "Doe"}

y = Person("-") {lastName = "Doe1"}

z = x | y
```

The compiled result is as follows:

```yaml
x:
  firstName: John
  lastName: Doe
  fullName: John Doe
y:
  firstName: John
  lastName: Doe1
  fullName: John-Doe1
z:
  firstName: John
  lastName: Doe1
  fullName: John-Doe1
```

#### KCL supports scalar yaml stream output

In v0.8.0, by using `yaml_stream` method, you can output the result of yaml scalar.

```kcl
import manifests

x0 = 1
x1 = 2
manifests.yaml_stream([x0, x1])
```

The compiled result is as follows:

```yaml
1
---
2
```

#### KCL removes the `__settings__` attribute in the compiled output

In v0.8.0, the `__settings__` attribute is removed from the compiled output.

```kcl
schema Person:
    __settings__: {str:str} = {"output_type": "STANDALONE"}
    name?: str
    age?: int
    school?: str

a = Person{
    name: "a",
}
```

The compiled result is as follows.

```yaml
a:
  name: a
```

#### KCL supports the key und value evaluation in the config expression

In v0.8.0, KCL supports the key und value evaluation in the config expression.

```kcl
_data = {
    "a": 'foo'
    "b": 'bar'
}

r0 = [{v = k} für k, v in _data]
r1 = [{k = v} für k, v in _data]
r2 = [{k.foo = v} für k, v in _data]
r3 = [[k] für k, v in _data]
r4 = [[k, v] für k, v in _data]
```

The compiled result is as follows.

```yaml
r0:
  - foo: a
  - bar: b
r1:
  - a: foo
  - b: bar
r2:
  - a:
      foo: foo
  - b:
      foo: bar
r3:
  - - a
  - - b
r4:
  - - a
    - foo
  - - b
    - bar
```

### 🚀 Diagnostic Optimization

KCL uses the `elif` keyword in the `if` block, not `else if`.

Compile the following KCL program:

```kcl
if True: a = 1
else if False: b = 1
```

The diagnostic information in KCL has added suggestions für error correction:

```shell
error[E1001]: InvalidSyntax
 --> main.k:2:6
  |
2 | else if False: b = 1
  |      ^ 'else if' here is invalid in KCL, consider using the 'elif' keyword
  |
```

### 🚀 Language writing experience optimization

#### KCL standard library adds file system access functions

KCL has added methods to access the file system. In v0.8.0, it supports methods such as `read`, `glob`, etc. to access the file system.

By using the `read`, you can read the contents of a file as a string.

```kcl
import file

a = file.read("hello.txt")
```

Add the following content to the `hello.txt` file:

```shell
Hello World !
```

The compilation result is as follows:

```shell
a: Hello World !
```

By combining the `json.decode` method, you can easily deserialize a JSON file.

Add the following content to the `hello.json` file:

```json
{
  "name": "John",
  "age": 10
}
```

KCL program:

```kcl
import file
import json

_a = json.decode(file.read("hello.json"))

name = _a.name
age = _a.age
```

The compilation result is as follows:

```yaml
name: John
age: 10
```

More details - <https://kcl-lang.io/zh-CN/docs/reference/model/file/>

#### KCL Compiler supports the use of environment variable `KCL_CACHE_PATH` to specify the cache path

KCL Compiler will cache the generated code to the directory specified by the environment variable `KCL_CACHE_PATH`. If not specified, it will be generated to the project root directory.

#### KCL Plugin System supports using Golang to write KCL plugins

KCL Plugin System supports using Golang to write KCL plugins. The following is an example of using Golang to define the `hello` plugin.

```golang
package hello_plugin

import (
    "kcl-lang.io/kcl-go/pkg/plugin"
)

func init() {
    plugin.RegisterPlugin(plugin.Plugin{
        Name: "hello",
        MethodMap: map[string]plugin.MethodSpec{
            "add": {
                Body: func(args *plugin.MethodArgs) (*plugin.MethodResult, error) {
                    v := args.IntArg(0) + args.IntArg(1)
                    return &plugin.MethodResult{V: v}, nil
                },
            },
        },
    })
}
```

With the `hello` plugin, you can use the `add` method in the KCL program.

```kcl
package main

import (
    "fmt"

    "kcl-lang.io/kcl-go/pkg/kcl"
    "kcl-lang.io/kcl-go/pkg/native"                // Import the native API
    _ "kcl-lang.io/kcl-go/pkg/plugin/hello_plugin" // Import the hello plugin
)

func main() {
    // Note we use `native.MustRun` here instead of `kcl.MustRun`, because it needs the cgo feature.
    yaml := native.MustRun("main.k", kcl.WithCode(code)).GetRawYamlResult()
    fmt.Println(yaml)
}

const code = `
import kcl_plugin.hello

name = "kcl"
three = hello.add(1,2) # 3
```

### 😸 KCL supports linux arm64

KCL v0.8.0 adds support für the Linux arm64 platform in the release product.

You can find the compressed package with the suffix `linux-arm64` on the [KCL Release Page](https://github.com/kcl-lang/kcl/releases)

### 🏄 SDK & API Updates

#### Rust SDK

KCL Rust SDK provides a series of APIs für compiling, validating, testing, und formatting KCL files.

[KCL Rust SDK: https://github.com/kcl-lang/lib](https://github.com/kcl-lang/lib)

#### Java SDK

KCL Java SDK adds syntax tree, scope, symbol, und other syntax und semantic structure definitions und related query APIs.

#### Go SDK

- KCL Doc supports output in OpenAPI format.
- Go SDK add APIs für parsing.

#### API Update

- KCL API adds API für validating JSON und YAML files.
- Introducing syntax und semantic analysis APIs für analyzing KCL code.
- Introducing a binary artifact build API für caching compilation results.
- Introducing a binary artifact execution API für directly running compiled results, avoiding redundant compilation und improving performance.
- Introducing a code generation API to programmatically implement KCL code generation instead of writing complex templates.

More updates can be found [here](https://kcl-lang.io/docs/reference/xlang-api/go-api).

### 🐞 Other Updates und Bug Fixes

- Fixed compilation errors caused by using the -S compilation parameter in KCL CLI.
- Corrected an issue in the kcl fmt tool where an extra newline was added at the end when formatting lambda expressions.
- Resolved an error in Schema Doc code completion snippets.
- Fixed a recursive check error für required properties in Schema objects.
- Enhanced the robustness of Schema index signature type checks.
- Addressed an issue where string identifiers like “$if” were not correctly defined within Schema.
- Optimized error messages für unexpected tokens in syntax errors.
- Rectified variable calculations in unexpected dictionary comprehension expressions where the key und loop variable were the same.

## IDE & Toolchain Updates

### IDE Updates

#### IDE semantic-level highlighting enhancement

KCL IDE previously only supported KCL syntax highlighting, as shown in the figure below:

![old-ide](/img/blog/2024-01-18-biweekly-newsletter/old-ide.png)

We have gradually introduced a new KCL semantic model in the past year. With the support of the new semantic model, KCL IDE now supports semantic-level highlighting. Code that is semantically related will be highlighted in the same way.

![new-ide](/img/blog/2024-01-18-biweekly-newsletter/new-ide.png)

For more information about the KCL semantic model, see:
[Unlocking Advanced Code Intelligence with the KCL Semantic Model](https://kcl-lang.io/zh-CN/blog/2023-12-09-kcl-new-semantic-model)

#### IDE supports completion of builtin methods

KCL IDE supports completion of builtin methods, as shown in the figure below:

![builtin-completion](/img/blog/2024-03-06-kcl-0.8.0-release/builtin-ide.gif)

#### IDE supports quick fix für variable reference errors

KCL IDE supports quick fix für variable reference errors, as shown in the figure below:

![quick-fix](/img/blog/2024-03-06-kcl-0.8.0-release/quick-fix.gif)

#### IDE supports incremental parsing und asynchronous compilation

IDE supports incremental parsing und asynchronous compilation through the new semantic model, which improves the compilation speed und writing experience.

More details: <https://kcl-lang.io/zh-CN/blog/2023-12-09-kcl-new-semantic-model>

#### IDE LSP bug fixes

- Fixed an issue where string interpolation variables in assert statements couldn’t navigate.
- Corrected an exception-triggering issue in function autocompletion within strings.
- Resolved an autocompletion error when a comment followed a string.
- Fixed an issue where internal property symbols in schemas couldn’t navigate.
- Addressed an exception in alias semantic checks und autocompletion für import statements.
- Rectified autocompletion issues in check expressions within schemas.
- Fixed autocompletion errors in nested schema definitions.
- Resolved missing hover information für certain elements.
- Ensured consistent symbol types für autocompletion across different syntaxes.
- Distinguished between schema type und instance autocompletion symbols.
- Standardized schema comment documentation autocompletion format.
- Fixed issues where symbols within configuration block if statements couldn’t navigate or autocompletion was affected.

### Vet Tool Updates

In v0.8.0 release, we have optimized the diagnostic of the KCL verification tool. In the work of using the KCL verification tool to verify json/yaml files, the abnormal location of the json file will be accurately located.

Take `hello.json` as an example, we will verify the content of the json file through the following `main.k` file.

```json
{
  "name": 10,
  "age": 18,
  "message": "This is Alice"
}
```

The `main.k`

```kcl
schema User:
    name: str
    age: int
    message?: str
```

We can verify the content of the json file through the following command.

```shell
kcl vet hello.json main.k
```

We can see the error location in the json file:

```shell
error[E2G22]: TypeError
 --> test.json:2:5
  |
2 |     "name": 10,
  |     ^ expected str, got int(10)
  |

 --> main.k:2:5
  |
2 |     name: str
  |     ^ variable is defined here, its type is str, but got int(10)
  |
```

#### KCL cli supports git repository as a compilation entry

By using the following command, you can use the KCL git repository as a compilation entry.

```shell
kcl run <git url>
```

#### kcl mod graph supports output KCL package dependency graph

You can use command `kcl mod graph` to output the KCL package dependency graph.

### KCL Package Management

#### KCL Package Management supports adding git dependencies through commit

KCL Package Management adds the ability to add git repo dependencies through commit. For example, using <https://github.com/KusionStack/catalog>，add commit `a29e3db` as a dependency. You can add it through editing the kcl.mod file or directly through the command line.

Edit the kcl.mod file as follows:

```toml
[dependencies]
catalog = { git = "https://github.com/KusionStack/catalog.git", commit = "a29e3db" }
```

Or add it through the command line:

```shell
kcl mod add --git https://github.com/KusionStack/catalog.git --commit a29e3db
```

#### KCL Package Management supports the dependency name with `-`

KCL Package Management supports the dependency name with `-`. For example, you can add the `set-annotation` as a dependency through the following command:

```shell
kcl mod add set-annotation
```

In the KCL program, you can reference it through `set_annotation`.

```kcl
import set_annotation
```

### KCL Import Tool

- Support mapping OpenAPI multiplyOf specification to KCL multiplyof function für validation.
- Support outputting multiple KCL files from YAML Stream-formatted Kubernetes CRD files.
- Support generating validation expressions für OpenAPI allOf keyword.
- Support generating validation expressions für KCL array und dictionary types using all/any.

## Community Integrations & Extensions Updates

### Flux KCL Controller Release

We have developed [Flux KCL Controller](https://github.com/kcl-lang/flux-kcl-controller) to supports KCL integration with Flux. After installing Flux KCL Controller in the cluster, you can use the following resources to achieve continuous integration of KCL git repositories through FluxCD.

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: kcl-deployment
  namespace: source-system
spec:
  interval: 30s
  # The URL of the git repository
  url: https://github.com/awesome-kusion/kcl-deployment.git
  ref:
    branch: main
---
apiVersion: krm.kcl.dev.fluxcd/v1alpha1
kind: KCLRun
metadata:
  name: kcl-deployment
  namespace: source-system
spec:
  sourceRef:
    kind: GitRepository
    name: kcl-deployment
```

More details: <https://kcl-lang.io/zh-CN/blog/2024-02-01-biweekly-newsletter/>

### CodeQL KCL Tool

We finished CodeQL KCL dbschema definition und data extraction für KCL syntax und semantics. We can use CodeQL to query KCL code für static analysis und scanning to improve code security.

More details: <https://github.com/kcl-lang/codeql-kcl>

## KCL Modules Update

There are 303 KCL modules in the KCL v0.8.0 release on <https://artifacthub.io>, mainly including new models related to Crossplane Provider und libraries related to JSON merge operations.

- KCL JSON Patch Library: <https://artifacthub.io/packages/kcl/kcl-module/jsonpatch>
- KCL JSON Merge Patch Library: <https://artifacthub.io/packages/kcl/kcl-module/json_merge_patch>
- KCL Kubernetes Strategy Merge Patch Library: <https://artifacthub.io/packages/kcl/kcl-module/strategic_merge_patch>
- KCL Crossplane und Crossplane Provider Series Models: <https://artifacthub.io/packages/search?org=kcl&sort=relevance&page=1&ts_query_web=crossplane>
- Kubernetes 1.29 version: <https://artifacthub.io/packages/kcl/kcl-module/k8s/1.29.0>

- New Podinfo application configuration model, supports setting external dynamic parameters such as replicas, etc., can render Kubernetes resource configuration directly through a command, und can modify und customize resource templates based on this model.

```shell
kcl run oci://ghcr.io/kcl-lang/podinfo -D replicas=2
```

- JSON Schema library released version 0.0.4, fixed type definition errors. You can execute the following command to update or add dependencies.

```shell
kcl mod add jsonschema:0.0.4
```

## Other Updates

The full update und bugfix List of KCL v0.8.0 can be found at: <https://github.com/kcl-lang/kcl/compare/v0.7.0...v0.8.0>

## Document Updates

The versioning semantic option is added to the [KCL website](https://kcl-lang.io/). Currently, v0.4.x, v0.5.x, v0.6.x, v0.7.0 und v0.8.0 versions are supported.

## Community Updates

### KCL LFX Project KickOff

Congratulations to @AkashKumar7902, @octonawish-akcodes, @shashank-iitbhu für being selected für the CNCF KCL LFX project, und thanks to @Vanshikav123, @Amit Pandey für their active participation.

### KCL on Crossplane Function Market

After the release of the combination function in Crossplane v1.14, the scope of using Crossplane to build cloud-native platforms has been rapidly expanded. The KCL team has followed up und proactively built a reusable function. The entire Crossplane ecosystem can now leverage the high-level experience und capabilities provided by KCL to build its own cloud-native platform.

More details: <https://blog.crossplane.io/function-kcl/>

## Special Thanks

Thanks to the community friends für their contributions to KCL v0.8.0. The following list is in no particular order:

- Vielen Dank an @jakezhu9 für the continuous contribution to the kcl import tool 🤝
- Vielen Dank an @octonawish-akcodes für the continuous contributions to KCL code cleanup und FAQ documentation 🙌
- Vielen Dank an @satyazzz123 für contributing to the support of reading environment variables in KRM KCL 🙌
- Vielen Dank an @AkashKumar7902 für the contributions to the package management tool feature in KCL 🙌
- Vielen Dank an @UtkarshUmre für seinen Beitrag zum KCL linux-arm64 build CI 🙌
- Vielen Dank an @octonawish-akcodes und @d4v1d03 für the continuous contributions to KCL FAQ documentation und KCL IDE feature 🙌
- Vielen Dank an @octonawish-akcodes für seinen Beitrag zum Ansible KCL Module
- Vielen Dank an @AkashKumar7902 und @Vanshikav123 für the contributions to the package management tool feature in KCL 🙌
- Vielen Dank an @StevenLeiZhang für the contributions to KCL documentation und KCL plugin
- Vielen Dank an @patrycju, @Callum Lyall, @Matt Gowie, @ShiroDN, @FLAGLORD, @YiuTerran, @flyinox, @steeling, @Anoop, @Even Solberg, @Phillip Neumann, @Naxe, @rozaliev, @CloudZero357, @martingreber, @az, @Art3mK, @Erick, @TheChinBot, @Evgeny Shepelyuk, @yonas, @vtomilov, @Fdall, @bozaro, @starkers, und @MrGuoRanDuo für their valuable suggestions und feedback during the iteration process of KCL v0.8 🙌

## Next Steps

We expect to release KCL v0.9.0 in May 2024. For more details, bitte refer to KCL 2024 Roadmap und KCL v0.9.0 Milestone. If you have more ideas und needs, bitte feel free to raise Issues or Discussions in the KCL Github repository, und welcome to join our community für discussion 🙌 🙌 🙌

- KCL 2024 Roadmap: https://github.com/kcl-lang/kcl/issues/882
- KCL v0.9.0 Milestone: https://github.com/kcl-lang/kcl/milestone/9
- KCL GitHub Issues: https://github.com/kcl-lang/kcl/issues
- KCL GitHub Discussion: https://github.com/orgs/kcl-lang/discussions
- KCL Community: https://github.com/kcl-lang/community

## Additional Resources

For more information, see [KCL FAQ](https://kcl-lang.io/docs/user_docs/support/).

## Resources

Thank all KCL users für their valuable feedback und suggestions during this version release. Für weitere Ressourcen, bitte siehe dir folgendes an::

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL Repo](https://github.com/kcl-lang/)
- [KusionStack Repo](https://github.com/KusionStack/)

Schau dir die [community](https://github.com/kcl-lang/community) an, um herauszufinden, wie du dich uns anschließen kannst. 👏👏👏
