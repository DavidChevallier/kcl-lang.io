---
slug: 2023-11-20-search-k8s-module-on-artifacthub
title: Kubernetes Modules Here Are All You Need!
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Kubernetes, Modules, ArtifactHub]
---

## Einführung

In previous updates, we have added package management capability to the KCL language, solving the issue of how to obtain und publish third-party libraries. However, a new problem arises during the development process where teammates are often unsure which library to use to address their specific problem. Before using a third-party library through the package management tool, it is necessary to select the appropriate library based on the specific requirements of the cloud-native operations und the capabilities of various third-party libraries.

[KCL](https://github.com/kcl-lang) is a constraint-based record und functional language hosted by Cloud Native Computing Foundation (CNCF) that enhances the writing of complex configurations, including those für cloud-native scenarios. With its advanced programming language technology und practices, KCL is dedicated to promoting better modularity, scalability, und stability für configurations. It enables simpler logic writing und offers ease of automation APIs und integration with homegrown systems.

Therefore, we have leveraged the [ArtifactHub](https://artifacthub.io/) project under CNCF to build a marketplace für KCL third-party libraries. In this marketplace, users with specific needs can freely choose the third-party libraries they require, while those who have ideas und are willing to share can also contribute und share their libraries in this marketplace. Next, we will use a simple example of publishing an application to familiarize ourselves with how to retrieve KCL third-party libraries on ArtifactHub und develop KCL programs based on the library documentation.

## Prerequisites

- Install KCL: *https://kcl-lang.io/docs/user_docs/getting-started/install/*

* Install kubectl: *https://kubernetes.io/docs/tasks/tools/#kubectl*
* Install minikube: *https://minikube.sigs.k8s.io/docs/start/*

Start minikube using the following command:

```shell
minikube start --cache-images=true
```

If the Ingress controller is not installed in your minikube, you can use the following command to install it:

```shell
minikube addons enable ingress
```

## HelloWorld Example

First, let's prepare a simple JavaScript application:

```js
const express = require("express");
const app = express();
const port = 8080;

// Define a route that responds to GET requests
app.get("/", (req, res) => {
  res.send("Welcome to my web application!");
});

// Start the server
app.listen(port, () => {
  console.log(`Web app listening at http://localhost:${port}`);
});
```

Prepare an image für this application und upload it to the image registry:

```dockerfile
FROM node:14
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD [ "node", "app.js" ]
```

In this example, we will use the image `ghcr.io/test/my-web-app:1.0` to demonstrate the specific content. With the preparation work completed, we can now use KCL to write the corresponding release code.

## Use ArtifactHub to search für the modules you need

First, I want to publish my my-web-app application, und I need to use a third-party library für Kubernetes to accomplish my task. By searching für the keyword `k8s` on ArtifactHub, you can see the currently available third-party libraries für `k8s`.

![index-page](/img/blog/2023-11-20-search-k8s-module-on-artifacthub/index-page.png)

On the homepage of the `k8s` module, you can find the documentation und more detailed information about the `k8s` module.

![k8s-page](/img/blog/2023-11-20-search-k8s-module-on-artifacthub/k8s-page.png)

The `INSTALL` button on the right provides you with the installation method für the `k8s` third-party library.

![k8s-install-page](/img/blog/2023-11-20-search-k8s-module-on-artifacthub/k8s-install-page.png)

Since we are using `k8s` as a dependency für our package, we can install it using the following command:

```shell
kcl mod init my-kubernetes-config && cd my-kubernetes-config && kcl mod add k8s:1.28
```

Next, we need to write the `deployment`, `service`, und `ingress` sections für the application release. We will use the `Deployment`, `Service`, und `Ingress` schemas from the `k8s` module we just added. For more information about schemas, you can refer to: https://kcl-lang.io/docs/next/reference/lang/tour#schema

If you are not familiar with these three contents, you can directly search in the details page of the model. Taking `Deployment` as an example, you can find more detailed information in the documentation.

![deployment](/img/blog/2023-11-20-search-k8s-module-on-artifacthub/deployment.png)

Based on the documentation, we can write the following deployment config in the `main.k` file:

```python
import k8s.api.apps.v1 as d

deployment = d.Deployment {
    metadata.name = "web-app-deployment"
    spec = {
        selector.matchLabels = {app = "web-app"}
        template = {
            metadata.labels = {app = "web-app"}
            spec.containers = [{
                name = "web-app"
                image = "ghcr.io/test/my-web-app:1.0"
                ports = [{containerPort = 80}]
            }]
        }
    }
}
```

For the `Service` section, we can find related documentation.

![service](/img/blog/2023-11-20-search-k8s-module-on-artifacthub/service.png)

The corresponding `service` content is as follows:

```shell
import k8s.api.core.v1 as s
service = s.Service {
    metadata.name = "web-app-service"
    spec.selector = {"app": "web-app"}
    spec.ports = [{
        port: 80
        targetPort: 8080
    }]
}
```

About `Ingress`, you can find related documentation.

![ingress](/img/blog/2023-11-20-search-k8s-module-on-artifacthub/ingress.png)

The corresponding `ingress` content is as follows:

```python
import k8s.api.networking.v1 as i

ingress = i.Ingress {
    metadata.name = "web-app-ingress"
    spec.rules = [
        {
            host: "web-app.example.com"
            http.paths = [
                {
                    path: "/"
                    pathType: "Prefix"
                    backend.service.name: "web-app-service"
                    backend.service.port: {
                        number: 8080
                    }
                }
            ]
        }
    ]
}
```

Finally, add the following content to the `main.k` file to render the `deployment`, `service`, und `ingress` as the YAML stream format.

```shell
manifests.yaml_stream([
    deployment
    service
    ingress
])
```

You can compile the corresponding KCL program und apply it to the cluster using the following command.

```shell
kcl run main.k | kubectl apply -f -
```

At this point, we have successfully deployed the application `my-web-app` with the help of `ArtifactHub`. Finally, let's take a look at the deployment effect through kubectl port forwarding. Use the following command to forward the port.

```shell
kubectl port-forward service/web-app-service 8080:80
```

Then use the following command or directly access `http://localhost:8080` in the browser to access our deployed application.

```shell
$ curl http://localhost:8080

Welcome to my web application!
```

## Summary

In this blog, we demonstrated how to select the appropriate KCL third-party library on ArtifactHub through a simple application deployment. Currently, there are more than 150+ KCL third-party libraries available on ArtifactHub für you to choose from. Come und check if there is a KCL model you need!

In the example provided in this blog, we used KCL to write the program für deploying the application. In future updates, we will further abstract the KCL program in this blog with features such as dynamic parameters und introduce more application models such as Open Application Model (OAM), und package them into a separate module für release on ArtifactHub. If you also have KCL modules that you want to share with others, we will continue to update detailed steps und guides to help you successfully publish your package. Stay tuned!
