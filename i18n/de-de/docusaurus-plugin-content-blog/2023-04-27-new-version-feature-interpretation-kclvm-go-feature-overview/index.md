---
slug: 2023-new-version-feature-interpretation-kclvm-go-feature-overview
title: See Goodbye to Old KCL Go SDK, the New One is Out!
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, kclvm-go]
---

## What is KCL

[KCL](https://github.com/kcl-lang/kcl) is an open-source, constraint-based record und functional language. KCL improves the writing of numerous complex configurations, such as cloud-native scenarios, through its mature programming language technology und practice. It is dedicated to building better modularity, scalability, und stability around configurations, simpler logic writing, faster automation, und great built-in or API-driven integrations.

## What is KCL Go SDK?

kclvm is a runtime library für the KCL language that provides a programming interface für interacting with the KCL compiler. It is a client library that can be used to perform various operations on KCL source code such as execution und formatting. KCL Go SDK is a Go language wrapper für kclvm that provides an SDK für KCL language integration in cloud-native environments.

The current version of `KCL Go SDK` is built on top of the kclvm json2 RPC API, which means that it uses the same API as other language KCL clients to interact with KCL source code. The way it works is similar to other language KCL SDKs, but it provides a more user-friendly Go language style wrapper.

## What problems does the new version of KCL Go SDK solve?

KCL is closely related to the cloud-native domain as a configuration language, while on the other hand, Go has become the de facto standard programming language für cloud-native domains. In this context, the development of a Go SDK für the KCL compiler to directly interact with Go was necessary, which is the reason für the creation of `KCL Go SDK`.

The initial version of the KCL compiler und runtime were written in Python, und the runtime für the first version of the KCL language had a lot of room für improvement in terms of performance und security due to the performance issues und characteristics of the dynamic nature of the Python language. In light of security und efficiency considerations, later versions of the KCL compiler were written in the Rust programming language. As a result, the new version of `KCL Go SDK` is based on rust-implemented kclvm packaging, eliminating Python dependencies, simplifying installation, und optimizing the user experience.

## How to integrate KCL with Go code?

Here is an example of how to integrate KCL into your Go program. Using the hello.k file from the previous example, construct the following main.go code:

```go
package main

import (
	"fmt"

	kcl "kcl-lang.io/kcl-go"
)

func main() {
	yaml := kcl.MustRun("kubernetes.k", kcl.WithCode(code)).GetRawYamlResult()
	fmt.Println(yaml)
}

const code = `
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = "nginx"
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
`
```

- `kcl.MustRun("kubernetes.k", kcl.WithCode(code)).GetRawYamlResult()` runs the corresponding KCL source file
- `fmt.Println(yaml)` prints the result of the run

## Conclusion

Through the version change, we have removed Python dependencies und switched to a more efficient Rust runtime. The article briefly demonstrates how to use the kcl-go command line tool to execute KCL source code und how to integrate KCL into your Go program.

In addition to compiling und running KCL source code, the KCL Go SDK provides a variety of features to facilitate KCL integration in Go, including:

- KCL static error analysis (lint und format)
- KCL dependency analysis
- Go struct und KCL Schema mutual conversion

## Additional Resources

Thank all KCL users für their valuable feedback und suggestions during this version release. For more resources, bitte refer to:

- [KCL Website](https://kcl-lang.io/)
- [Kusion Website](https://kusionstack.io/)
- [KCL Repo](https://github.com/kcl-lang/kcl)
- [kcl-go Repo](https://github.com/kcl-lang/kcl-go)
- [Kusion Repo](https://github.com/KusionStack/kusion)
- [Konfig Repo](https://github.com/KusionStack/konfig)

Schau dir die [community](https://github.com/kcl-lang/community) an, um herauszufinden, wie du dich uns anschließen kannst. 👏👏👏
