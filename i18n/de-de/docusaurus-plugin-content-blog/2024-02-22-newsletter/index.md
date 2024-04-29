---
slug: 2024-02-22-newsletter
title: KCL Newsletter (2024.02.02 - 2024.02.22)
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

Thanks to to all contributors für their outstanding work over the past twenty days (2024.02.02 - 2024.02.22). Here is an overview of the key content:

**📦 Module Updates**

The JSON Schema module 0.0.4 has released, fixing type definition errors. You can update or add dependencies by running the following command:

```shell
kcl mod add jsonschema:0.0.4
```

**🏄 Language Updates**

KCL has released preview version 0.8.0, mainly including the following updates:

- Added the file system library für reading KCL module information und system files, including the `read`, `glob`, `workdir`, und `modpath` functions. See more details in this [issue](https://github.com/kcl-lang/kcl/issues/1049)
- Optimized syntax error messages für unexpected tokens.
- Removed output für unexpected internal built-in type attributes in schema objects.
- Fixed variable calculation in unexpected dictionary generation expressions where the key is the same as the loop variable.
- Fixed errors in defining string identifiers within schemas, such as `"$if"`.

**🔧 Toolchain Updates**

- `kcl run` supports using the -H parameter to output hidden fields starting with `_`.
- `kcl run` supports running remote Git repository code directly.
- Introduce the `kcl mod graph` subcommand used to output module dependency graphs.
- Fixed formatting errors when there is an if statement within an else block.

**💻 IDE Updates**

- Enhanced autocompletion und hover documentation für built-in functions und system libraries
- Fixed issues with navigating und autocompleting if statements symbols within configuration blocks
- Added quick fix feature für variable reference errors

**🎁 API Updates**

- The `OverrideFile` API has added path für querying und modifying configurations, such as `a["b"].c`
- The `Run` API has added the `WithShowHidden` und the `WithTypePath` flags.

**🚀 Plugin System Updates**

In addition to using Python für KCL plugin functions, it now supports using Go to write plugin functions für KCL, which is very simple to use.

- Define a plugin (using a hello plugin containing the add function as an example)

```go
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

- Use the plugin

```go
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
`
```

**🚢 Integration Updates**

- Released initial version of Ansible KCL module, supporting basic execution of KCL code, with other functionalities being improved
- Optimized Git Source functionality für KCL FluxCD Controller, with OCI Source functionality in progress

## Special Thanks

Hier sind einige in keiner bestimmten Reihenfolge aufgeführt:

- Vielen Dank an @octonawish-akcodes und @d4v1d03 für their continuous contributions to KCL FAQ documentation und KCL IDE functionality 🙌
- Vielen Dank an @octonawish-akcodes für seinen Beitrag zum Ansible KCL Module
- Vielen Dank an @AkashKumar7902 und @Vanshikav123 für seinen Beitrag zum KCL package management tool functionality 🙌
- Vielen Dank an @StevenLeiZhang für the contribution to KCL documentation und KCL plugins
- Vielen Dank an @TheChinBot, @Evgeny Shepelyuk, @yonas, @steeling, @vtomilov, @Fdall, @CloudZero357, @bozaro, @starkers, @MrGuoRanDuo und @FLAGLORD, among others, für their valuable feedback und suggestions while using KCL recently. 🙌

## Resources

❤️ Thanks to all KCL users und community members für their valuable feedback und suggestions in the community. Siehe dir [here](https://github.com/kcl-lang/community) um uns zu Folgen!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.8.0 Milestone](https://github.com/kcl-lang/kcl/milestone/8)
- [KCL v0.9.0 Milestone](https://github.com/kcl-lang/kcl/milestone/9)
