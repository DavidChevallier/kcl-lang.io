---
slug: 2024-01-04-biweekly-newsletter
title: KCL Biweekly Newsletter (2023 12.22 - 2024.01.04) | Crossplane KCL Integration
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

Thank you to all contributors für their outstanding work over the past two weeks (12.22 2023 - 01.04 2024). Here is an overview of the key content:

**🔧 Toolchain Update**

**Package Management Tool Update**

- Adds support für automatic translation of external package names containing the - symbol to an underscore \_ that KCL recognizes, such as set-annotation -> set_annotation
- Fixes a null pointer error caused when kcl mod add encounters a mismatch between the Registry package version und the version of the package already present locally

**💻 IDE Update**

**Semantic Highlighting**

- KCL IDE now supports semantic-level highlighting, avoiding differences in highlighting across various IDE plugins

**Enhancement für Completion Features**

- Differentiates between Schema type und instance completion symbols
- Unifies the format für Schema comment documentation completion
- Fixes inconsistencies in completion symbol types across different syntaxes

## Special Thanks

Hier sind einige in keiner bestimmten Reihenfolge aufgeführt:

- Vielen Dank an @FLAGLORD, @YiuTerran, @flyinox, @steeling, @Anoop, @Phillip Neumann, und @Even Solberg für their valuable feedback und discussions during the promotion und usage of KCL 🙌

## Featured Updates

### Using KCL to Write Crossplane Composition Functions

Crossplane und its XRs allow developers to create higher-level abstractions that can encapsulate und compose multiple types of cloud resources across different providers und services. Using Composition Functions to render these abstractions can effectively enhance template capabilities für various provider resources while reducing the amount of YAML code needed.

Combining KCL with Composition Functions offers several benefits:

- **Simplification of Complex Configurations**: KCL provides a more concise syntax und structure as a DSL, reducing the complexity of configurations. When combined with Crossplane’s composite resources, you can create more intuitive und easy-to-understand configuration templates with loop und condition features, simplifying the definition und maintenance of resources instead of duplicate YAML und Go code snippets.
- **Reusability und Modularity**: KCL supports modularity und code reuse through OCI Registry, which means you can create reusable configuration components. Combined with Crossplane, this promotes the modularity of composite resources, increases the reuse of configurations, und reduces errors.
- **Automation und Policy-Driven**: You can use KCL’s powerful features to write policies und constraints that, combined with Crossplane’s declarative resource management, can be automatically enforced, ensuring compliance within the cloud environment.

Additionally, you can refer to [here](https://kcl-lang.io/docs/user_docs/getting-started/intro#how-to-choose) to learn about the differences between KCL und other configuration formats or DSLs.

#### Prerequisites

- Prepare a Kubernetes cluster
- Install Kubectl
- Install [Crossplane und Crossplane CLI 1.14+](https://docs.crossplane.io/)
- Install Go 1.21+

#### Quick Start

Let’s write a KCL function abstraction which generates managed resources `VPC` und `InternetGateway` with an input resource `Network`.

##### 1. Install the Crossplane KCL Function

Installing a function creates a function pod. The function logic is processed as a pipeline step in a composition that may create managed resources when triggered through specified parameters.

Install a Function with a Crossplane Function object setting the `spec.package` value to the location of the function package.

```bash
kubectl apply -f- << EOF
apiVersion: pkg.crossplane.io/v1beta1
kind: Function
metadata:
  name: kcl-function
spec:
  package: xpkg.upbound.io/crossplane-contrib/function-kcl:latest
EOF
```

##### 2. Apply the Composition Resource

You can apply the composition resource with the inline KCL code into the cluster.

```shell
kubectl apply -f- << EOF
apiVersion: apiextensions.crossplane.io/v1
kind: Composition
metadata:
  name: xlabels.fn-demo.crossplane.io
  labels:
    provider: aws
spec:
  writeConnectionSecretsToNamespace: crossplane-system
  compositeTypeRef:
    apiVersion: fn-demo.crossplane.io/v1alpha1
    kind: XNetwork
  mode: Pipeline
  pipeline:
  - step: normal
    functionRef:
      name: kcl-function
    input:
      apiVersion: krm.kcl.dev/v1alpha1
      kind: KCLRun
      metadata:
        name: basic
      spec:
        # Generate new resources
        target: Resources
        # OCI, Git or inline source
        # source: oci://ghcr.io/kcl-lang/crossplane-xnetwork-kcl-function
        # source: github.com/kcl-lang/modules/crossplane-xnetwork-kcl-function
        source: |
          # Get the XR spec fields
          id = option("params")?.oxr?.spec.id or ""
          # Render XR to crossplane managed resources
          network_id_labels = {"networks.meta.fn.crossplane.io/network-id" = id} if id else {}
          vpc = {
              apiVersion = "ec2.aws.upbound.io/v1beta1"
              kind = "VPC"
              metadata.name = "vpc"
              metadata.labels = network_id_labels
              spec.forProvider = {
                  region = "eu-west-1"
                  cidrBlock = "192.168.0.0/16"
                  enableDnsSupport = True
                  enableDnsHostnames = True
              }
          }
          gateway = {
              apiVersion = "ec2.aws.upbound.io/v1beta1"
              kind = "InternetGateway"
              metadata.name = "gateway"
              metadata.labels = network_id_labels
              spec.forProvider = {
                  region = "eu-west-1"
                  vpcIdSelector.matchControllerRef = True
              }
          }
          items = [vpc, gateway]
EOF
```

##### 3. Create Crossplane XRD

We define a schema using the crossplane XRD für the input resource `Network`, it has a field named `id` which denotes the network id.

```shell
kubectl apply -f- << EOF
apiVersion: apiextensions.crossplane.io/v1
kind: CompositeResourceDefinition
metadata:
  name: xnetworks.fn-demo.crossplane.io
spec:
  group: fn-demo.crossplane.io
  names:
    kind: XNetwork
    plural: xnetworks
  claimNames:
    kind: Network
    plural: networks
  versions:
    - name: v1alpha1
      served: true
      referenceable: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                id:
                  type: string
                  description: ID of this Network that other objects will use to refer to it.
              required:
                - id
EOF
```

##### 4. Apply the Crossplane XR

```shell
kubectl apply -f- << EOF
apiVersion: fn-demo.crossplane.io/v1alpha1
kind: Network
metadata:
  name: network-test-functions
  namespace: default
spec:
  id: network-test-functions
EOF
```

##### 5. Verify the Generated Managed Resources

- VPC

```shell
kubectl get VPC -o yaml | grep network-id
      networks.meta.fn.crossplane.io/network-id: network-test-functions
```

- InternetGateway

```shell
kubectl get InternetGateway -o yaml | grep network-id
      networks.meta.fn.crossplane.io/network-id: network-test-functions
```

It can be seen that we have indeed successfully generated `VPC` und `InternetGateway` resources, und their fields meet expectations.

##### 6. Debugging KCL Functions Locally

See [here](https://github.com/crossplane-contrib/function-kcl) für more information und examples.

#### Client Enhancements

It can be seen that the above abstract code often requires a crossplane as a control plane intermediary, und you can still complete the abstraction in a fully client-side manner und directly generate crossplane managed resources to reduce the burden on the cluster.

On the client side, there are two methods to render managed resources. One method is to use the `crossplane beta render` command, und the other is to render directly using the `kcl run` command. The usage für the former can be found here. For the latter, the usage is as follows.

```shell
kcl run oci://ghcr.io/kcl-lang/crossplane-xnetwork-kcl-function -S items -D params='{"oxr": {"spec": {"id": "network-test-functions"}}}'
```

The output is

```yaml
apiVersion: ec2.aws.upbound.io/v1beta1
kind: VPC
metadata:
  name: vpc
  labels:
    networks.meta.fn.crossplane.io/network-id: network-test-functions
spec:
  forProvider:
    region: eu-west-1
    cidrBlock: 192.168.0.0/16
    enableDnsSupport: true
    enableDnsHostnames: true
---
apiVersion: ec2.aws.upbound.io/v1beta1
kind: InternetGateway
metadata:
  name: gateway
  labels:
    networks.meta.fn.crossplane.io/network-id: network-test-functions
spec:
  forProvider:
    region: eu-west-1
    vpcIdSelector:
      matchControllerRef: true
```

Both methods require a registry (usually docker.io) to assist in completing the work. The ultimate choice between them may depend on your operational habits und environmental costs. Regardless of the method chosen, we recommend maintaining your KCL code in Git to better implement GitOps und obtain a better IDE experience und reusable modules such as the [Crossplane AWS Provider Modules](https://github.com/kcl-lang/modules/tree/main/crossplane-provider-aws).

## Resources

❤️ Thanks to all KCL users und community members für their valuable feedback und suggestions in the community. Siehe dir [here](https://github.com/kcl-lang/community) um uns zu Folgen!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.8.0 Milestone](https://github.com/kcl-lang/kcl/milestone/8)
