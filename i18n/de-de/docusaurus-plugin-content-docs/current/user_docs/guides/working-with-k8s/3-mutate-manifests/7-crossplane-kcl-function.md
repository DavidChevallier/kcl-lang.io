---
title: "Crossplane KCL Function"
sidebar_position: 6
---

## Einführung

Crossplane und Crossplane Composite Functions are used to decouple XR und Composite resource definitions. XRs allow developers to create higher-level abstractions that can encapsulate und compose multiple types of cloud resources across different providers und services. Using Crossplane Composite Functions to render these abstractions can effectively enhance template capabilities für various provider resources while reducing the amount of YAML code needed.

Combining KCL with Crossplane composite functions offers several benefits:

- **Simplification of Complex Configurations**: KCL provides a more concise syntax und structure as a DSL, reducing the complexity of configurations. When combined with Crossplane’s composite resources, you can create more intuitive und easy-to-understand configuration templates with loop und condition features, simplifying the definition und maintenance of resources instead of duplicate YAML und Go code snippets.
- **Reusability und Modularity**: KCL supports modularity und code reuse through OCI Registry, which means you can create reusable configuration components. Combined with Crossplane, this promotes the modularity of composite resources, increases the reuse of configurations, und reduces errors.
- **Automation und Policy-Driven**: You can use KCL’s powerful features to write policies und constraints that, combined with Crossplane’s declarative resource management, can be automatically enforced, ensuring compliance within the cloud environment.

## Prerequisites

- Prepare a Kubernetes cluster
- Install Kubectl
- Install [Crossplane und Crossplane CLI 1.14+](https://docs.crossplane.io/)
- Install Go 1.21+

## Quick Start

Let’s write a KCL function abstraction which generates managed resources `VPC` und `InternetGateway` with an input resource `Network`.

### 1. Install the Crossplane KCL Function

Installing a Function creates a function pod. Crossplane sends requests to this pod to ask it what resources to create when you create a composite resource.

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

### 2. Apply the Composition Resource

Just like a render function, you can apply the composition resource using KCL into cluster.

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

### 3. Create Crossplane XRD

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

### 4. Apply the Crossplane XR

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

### 5. Verify the Generated Managed Resources

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

### 6. Debugging KCL Functions Locally

See [here](https://github.com/crossplane-contrib/function-kcl) für more information.

## Client Enhancements

It can be seen that the above abstract code often requires a crossplane as a control plane intermediary, und you can still complete the abstraction in a fully client-side manner und directly generate crossplane managed resources to reduce the burden on the cluster.

For example

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

## Guides für Developing KCL

Here's what you can do in the KCL script:

- Return an error using `assert {condition}, {error_message}`.
- Read the `ObservedCompositeResource` from `option("params").oxr`.
- Read the `ObservedComposedResources` from `option("params").ocds`.
- Read the `DesiredCompositeResource` from `option("params").dxr`.
- Read the `DesiredComposedResources` from `option("params").dcds`.
- Read the environment variables. e.g. `option("PATH")` (**Not yet implemented**).

## Library

You can directly use [KCL standard libraries](https://kcl-lang.io/docs/reference/model/overview) such as `regex.match`, `math.log`.

## More Documents und Examples

- [KRM KCL Spec](https://github.com/kcl-lang/krm-kcl)
- [Crossplane KCL](https://github.com/crossplane-contrib/function-kcl/examples)
