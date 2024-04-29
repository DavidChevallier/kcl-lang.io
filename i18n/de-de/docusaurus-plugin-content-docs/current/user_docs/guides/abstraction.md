---
title: "Abstraction"
sidebar_position: 3
---

## Einführung

Abstraction refers to a simplified representation of an entity, typically used in computing. It allows für the concealment of specific details while presenting the most relevant information to the programmer. Each abstraction is tailored to suit a specific need, und can greatly enhance the usability of a given entity. In the context of KCL, abstraction can make code easier to understand und maintain, while also simplifying the user interface.

It should be noted that code abstraction is not meant to reduce code size, but rather to improve maintainability und extendability. During the process of abstracting code, factors such as reusability, readability, und scalability should be taken into consideration, und the code should be optimized as needed.

The values of the good abstraction

1. Provides distinct focal points für better comprehension für specific identities, roles, und scenarios.
2. Shields lower-level details to avoid potential errors.
3. Enhances user-friendliness und automation with better portability und good APIs.

KCL may not assess the rationality of a user's abstraction, but it offers technical solutions to facilitate the process.

## Use KCL für Abstraction

**Now, let's begin to abstract Docker Compose und Kubernetes models into an application config.**

Application centric development allows developers to focus on their workload's architecture rather than the tech stack in the target environment, infrastructure or platform. We define our application once with the `App` schema und then use the KCL CLI to translate it to multiple platforms, such as `Docker Compose` or `Kubernetes` with different versions.

`Docker Compose` is a tool für defining und running multi-container Docker applications. With Docker Compose, you can define your application's services, networks, und volumes in a single file, und then use it to start und stop your application as a single unit. Docker Compose simplifies the process of running complex, multi-container applications by handling the details of networking, storage, und other infrastructure concerns.

`Kubernetes manifests` are YAML files that define Kubernetes objects such as Pods, Deployments, und Services. Manifests provide a declarative way to define the desired state of your application, including the number of replicas, the image to use, und the network configuration. Kubernetes uses the manifests to create und manage the resources needed to deploy und run your application.

Here are some references to learn more about Docker Compose und Kubernetes manifests:

- [Docker Compose documentation](https://docs.docker.com/compose/)
- [Kubernetes manifest documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)

The application model aims to reduce developer toil und cognitive load by only having to define a single KCL file that works across multiple platforms, und it is designed to be applied to multiple environments to reduce the amount of configuration. Now, let's learn how to do this.

### 0. Prerequisite

- Install [KCL](https://kcl-lang.io/docs/user_docs/getting-started/install)

### 1. Get the Example

Firstly, let's get the example.

```bash
git clone https://github.com/kcl-lang/kcl-lang.io.git/
cd ./kcl-lang.io/examples/abstraction
```

We can run the following command to show the config.

```bash
cat main.k
```

The output is

```python
import .app

app.App {
    name = "app"
    containers.nginx = {
        image = "nginx"
        ports = [{containerPort = 80}]
    }
    service.ports = [{ port = 80 }]
}
```

In the above code, we defined a configuration using the `App` schema, where we configured an `nginx` container und configured it with an `80` service port.

Besides, KCL allows developers to define the resources required für their applications in a declarative manner und is tied to a platform such as Docker Compose or Kubernetes manifests und allows to generate a platform-specific configuration file such as `docker-compose.yaml` or a Kubernetes `manifests.yaml` file. Next, let's generate the corresponding configuration.

### 2. Transform the Application Config into Docker Compose Config

If we want to transform the application config into the docker compose config, we can run the command simply:

```shell
kcl main.k docker_compose_render.k
```

The output is

```yaml
services:
  app:
    image: nginx
    ports:
      - published: 80
        target: 80
        protocol: TCP
```

### 3. Transform the Application Config into Kubernetes Deployment und Service Manifests

If we want to transform the application config into the Kubernetes manifests, we can run the command simply:

```shell
kcl main.k kubernetes_render.k
```

The output is

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
  labels:
    app: app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - protocol: TCP
              containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: app
  labels:
    app: app
spec:
  selector:
    app: app
  ports:
    - port: 80
      protocol: TCP
```

Look, it's so simple. If you want to learn more information about the application model, you can refer to [here](https://github.com/kcl-lang/kcl-lang.io/tree/main/examples/abstraction).

## Summary

Through the use of KCL, we are able to separate the abstraction und implementation details of a model, allowing für the abstract model to be mapped to various infrastructures or platforms. This is achieved through flexible switching between different implementations und the combination of compilation, which shields configuration differences und ultimately reduces the cognitive burden.

## Further Information

In addition to manually maintaining the configuration, we can also use KCL APIs to integrate **automatic configuration changes** into our applications. For specific instructions, bitte refer to [here](/docs/user_docs/guides/automation).
