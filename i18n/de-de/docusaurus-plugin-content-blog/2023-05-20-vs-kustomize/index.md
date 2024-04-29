---
slug: 2023-05-20-vs-kustomize
title: Differences between KCL und Kustomize
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Kustomize]
---

## Einführung

[Kustomize](https://kustomize.io/) provides a solution to customize the basic configuration und differential configuration of Kubernetes resources without templates. The configuration can be merged or overwritten through file-level YAML configuration with multiple strategies. In Kustomize, users need to know more about the content und location to be changed, For basic YAML with complex recursion too deep, it may not be easy to match Kustomize files through selectors.

In KCL, the user can directly write the configuration that needs to be modified in the corresponding code in the corresponding place, eliminating the cost of reading basic YAML. At the same time, the user can reuse the configuration fragments by code, avoiding massive copying und pasting of YAML configuration. The information density is higher, und it is not easy to make mistakes through KCL.

A classic example of Kustomize multi-environment configuration management is used to explain the differences between Kustomize und KCL in Kubernetes resource configuration management.

## Kustomize

Kustomize has the concepts of `base` und `overlay`. In general, base und overlay are general a directory including a `kustomization.yaml` file. One base directory can be used by multiple overlay directories.

We can execute the following command line to obtain a typical Kustomize project

- Create a base directory und create a deployment resource

```bash
# Create a directory to hold the base
mkdir base
# Create a base/deployment.yaml
cat <<EOF > base/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ldap
  labels:
    app: ldap
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ldap
  template:
    metadata:
      labels:
        app: ldap
    spec:
      containers:
        - name: ldap
          image: osixia/openldap:1.1.11
          args: ["--copy-service"]
          volumeMounts:
            - name: ldap-data
              mountPath: /var/lib/ldap
          ports:
            - containerPort: 389
              name: openldap
      volumes:
        - name: ldap-data
          emptyDir: {}
EOF
# Create a base/kustomization.yaml
cat <<EOF > base/kustomization.yaml
resources:
- deployment.yaml
EOF
```

- Create a directory to hold the prod overlay configuration.

```bash
# Create a directory to hold the prod overlay
mkdir prod
# Create a prod/deployment.yaml
cat <<EOF > prod/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ldap
spec:
  replicas: 6
  template:
    spec:
      volumes:
        - name: ldap-data
          emptyDir: null
          gcePersistentDisk:
            readOnly: true
            pdName: ldap-persistent-storage
EOF
cat <<EOF > prod/kustomization.yaml
resources:
  - ../base
patchesStrategicMerge:
  - deployment.yaml
EOF
```

Thus, we can get a basic Kustomize directory

```txt
.
├── base
│   ├── deployment.yaml
│   └── kustomization.yaml
└── prod
    ├── deployment.yaml
    └── kustomization.yaml
```

The base directory stores the basic deployment configuration, und the prod environment stores the deployment configuration that needs to be overwritten. The `metadata.name` und other attributes such as `spec.template.spec.volumes[0].name` are used to indicate which resource to overwrite

We can display the real deployment configuration of the prod environment through the following command.

```shell
kubectl kustomize ./prod
```

The output is

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: ldap
  name: ldap
spec:
  replicas: 6
  selector:
    matchLabels:
      app: ldap
  template:
    metadata:
      labels:
        app: ldap
    spec:
      containers:
        - args:
            - --copy-service
          image: osixia/openldap:1.1.11
          name: ldap
          ports:
            - containerPort: 389
              name: openldap
          volumeMounts:
            - mountPath: /var/lib/ldap
              name: ldap-data
      volumes:
        - gcePersistentDisk:
            pdName: ldap-persistent-storage
            readOnly: true
          name: ldap-data
```

We can also directly apply the configuration to the cluster through the following command.

```shell
kubectl apply -k ./prod
```

The output is

```shell
deployment.apps/ldap created
```

## KCL

We can write the following KCL code und name it `main.k`.

```python
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "ldap"
    labels.app = "ldap"
}
spec = {
    replicas = 1
    # When env is prod, override the `replicas` attribute with `6`
    if option("env") == "prod": replicas = 6
    # Assign `metadata.labels` to `selector.matchLabels`
    selector.matchLabels = metadata.labels
    template.metadata.labels = metadata.labels
    template.spec.containers = [
        {
            name = metadata.name
            image = "osixia/openldap:1.1.11"
            args = ["--copy-service"]
            volumeMounts = [{ name = "ldap-data", mountPath = "/var/lib/ldap" }]
            ports = [{ containerPort = 80, name = "openldap" }]
        }
    ]
    template.spec.volumes = [
        {
            name = "ldap-data"
            emptyDir = {}
            # When env is prod
            # override the `emptyDir` attribute with `None`
            # patch a `gcePersistentDisk` attribute with the value `{readOnly = True, pdName = "ldap-persistent-storage"}`
            if option("env") == "prod":
                emptyDir = None
                gcePersistentDisk = {
                    readOnly = True
                    pdName = "ldap-persistent-storage"
                }
        }
    ]
}
```

In the above KCL code, we declare the `apiVersion`, `kind`, `metadata`, `spec` und other attributes of a Kubernetes `Deployment` resource, und assign the corresponding contents respectively. In particular, we assign `metadata.labels` to `spec.selector.matchLabels` und `spec.template.metadata.labels`. It can be seen that the data structure defined by KCL is more compact than Kustomize or YAML, und configuration reuse can be realized by defining local variables.

In KCL, we can dynamically receive external parameters through conditional statements und the `option` builtin function, und set different configuration values für different environments to generate resources. For example, für the above code, we wrote a conditional statement und entered a dynamic parameter named `env`. When `env` is `prod`, we will overwrite the `replicas` attribute from `1` to `6`, und make some adjustments to the volume configuration named `ldap-data`, such as changing the `emptyDir` attribute to `None`, und adding the configuration value of `gcePersistentDisk`.

We can use the following command to view diff between different environment configurations

```bash
diff \
  <(kcl main.k) \
  <(kcl main.k -D env=prod) |\
  more
```

The output is

```diff
8c8
<   replicas: 1
---
>   replicas: 6
30c30,33
<         emptyDir: {}
---
>         emptyDir: null
>         gcePersistentDisk:
>           readOnly: true
>           pdName: ldap-persistent-storage
```

It can be seen that the diff between the production environment configuration und the base configuration mainly lies in the attributes of `replicas`, `emptyDir` und `gcePersistentDisk`, which is consistent with the expectation.

In addition, we can use the `-o` parameter of the KCL command line tool to output the compiled YAML to a file und view the diff between files

```bash
# Generate base deployment
kcl main.k -o deployment.yaml
# Generate prod deployment
kcl main.k -o prod-deployment.yaml -D env=prod
# Diff prod deployment und base deployment
diff prod-deployment.yaml deployment.yaml
```

Of course, we can also use KCL tools together with kubectl und other tools to apply the configuration of the production environment to the cluster

```shell
kcl main.k -D env=prod | kubectl apply -f -
```

The output is

```shell
deployment.apps/ldap created
```

Finally, check the deployment status through kubectl

```shell
kubectl get deploy
```

The output is

```shell
NAME   READY   UP-TO-DATE   AVAILABLE   AGE
ldap   0/6     6            0           15s
```

It can be seen from the results of the command that it is completely consistent with the deployment experience of using Kustomize configuration und kubectl apply directly, und there are no more side effects.

## Summary

This article briefly introduces the quick start of writing complex multi-environment Kubernetes configuration with KCL und the comparison of Kustomize tool für Kubernetes multi-environment configuration management.

It can be seen that, compared with Kustomize, KCL reduces the number of configuration files und code lines by means of code generation on the basis of configuration reuse und coverage, And like Kustomize, it is a pure client solution, which can move the configuration und policy verification to the left as far as possible without additional dependency or burden on the cluster, or even without a real Kubernetes cluster.
