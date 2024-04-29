---
slug: 2023-12-21-biweekly-newsletter
title: KCL Biweekly Newsletter (2023 12.07 - 12.21) | KCL v0.7.2 is released und KubeVela/OAM integration is available now!
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

Thank you to all contributors für their outstanding work over the past two weeks (11.24 - 12.07 2023). Here is an overview of the key content:

**📦 Model Updates**

KCL model quantity increased to **300**, added KCL models für k8s 1.29.

**🔧 Toolchain Updates**

- **Import Tool Updates**

  - Import tool supports OpenAPI allOf keyword validation expression generation
  - Import tool supports KCL array und dictionary type all/any validation expression generation
  - Import tool fixes JSON Schema array generation KCL code snippet error und string escape error

- **🏄 Package Management Tool Updates**
  - Added support für third-party libraries with hyphens in their names.
  - Fixed the problem that the update function cannot automatically pass kcl.mod und kcl.mod.lock.

**💻 KCL Updates**

- KCL compilation cache path supports using environment variables KCL_CACHE_PATH to specify
- Fixed the compilation error that may be caused by the compilation parameter -S of KCL CLI
- Fixed the error that kcl fmt tool adds an empty line at the end when formatting lambda expressions.
- Fixed Schema Doc completion code snippet error

**📒 IDE Updates**

- Fixed the problem that the variable completion in the check statement is invalid
- VSCode Extension updated to version 0.1.3, updated the highlighting und completion of some keywords
- Added completion of builtin functions
- Optimized the style of function completion

## Featured Updates

### Efficient Cloud Native Application Deployment - KCL und KubeVela Integration Quick Guide

[KCL](https://kcl-lang.io) is a configuration und policy language für cloud-native scenarios, hosted by the CNCF Foundation. It aims to improve the writing of complex configurations, such as cloud-native Kubernetes configurations, using mature programming language techniques und practices. KCL focuses on building better modularity, scalability, und stability around configuration, as well as easier logic writing, automation, und integration with the toolchain.

KCL exists in a completely open cloud-native world und is not tied to any orchestration/engine tools or Kubernetes controllers. It can provide API abstraction, composition, und validation capabilities für both Kubernetes clients und runtime.

[KubeVela](https://kubevela.net/) is a modern application delivery system hosted by the CNCF Foundation. It is built on the Open Application Model (OAM) specification und aims to abstract the complexity of Kubernetes, providing a set of simple und easy-to-use command-line tools und APIs für developers to deploy und operate cloud-native applications without worrying about the underlying details.

Using KCL with KubeVela has the following benefits:

- **Simpler configuration**: KCL provides stronger templating capabilities, such as conditions und loops, für KubeVela OAM configurations at the client level, reducing the need für repetitive YAML writing. At the same time, the reuse of KCL model libraries und toolchains enhances the experience und management efficiency of configuration und policy writing.
- **Better maintainability**: KCL provides a configuration file structure that is more conducive to version control und team collaboration, instead of relying solely on YAML. When combined with OAM application models written in KCL, application configurations become easier to maintain und iterate.
- **Simplified operations**: By combining the simplicity of KCL configurations with the ease of use of KubeVela, daily operational tasks such as deploying, updating, scaling, or rolling back applications can be simplified. Developers can focus more on the applications themselves rather than the tedious details of the deployment process.
- **Improved cross-team collaboration**: By using KCL's configuration chunk writing und package management capabilities in conjunction with KubeVela, clearer boundaries can be defined, allowing different teams (such as development, testing, und operations teams) to collaborate systematically. Each team can focus on tasks within their scope of responsibility, delivering, sharing, und reusing their own configurations without worrying about other aspects.

Taking the KCL Playground application (written in Go und HTML5) as an example, we use KCL to define the OAM configuration that needs to be deployed. The overall workflow is as follows:

- Prepare

  - Configure the Kubernetes cluster
  - Install KubeVela
  - Install KCL

- New project und add OAM dependency

```
kcl mod init kcl-play-svc && cd kcl-play-svc && kcl mod add oam
```

- Write code in main.k

```
import oam

oam.Application {
    metadata.name = "kcl-play-svc"
    spec.components = [{
        name = metadata.name
        type = "webservice"
        properties = {
            image = "kcllang/kcl"
            ports = [{port = 80, expose = True}]
            cmd = ["kcl", "play"]
        }
    }]
}
```

- Run command to deploy configuration

```
kcl run | vela up -f -
```

- Port forwarding

```
vela port-forward kcl-play-svc
```

Then we can see the KCL Playground application running successfully in the browser

![kcl-play-svc](/img/blog/2023-12-15-kubevela-integration/kcl-play-svc.png)

### IDE Optimized the Style of Function Completion

![ide-func](/img/blog/2023-12-21-biweekly-newsletter/ide-func.gif)

## Resources

❤️ Thanks to all KCL users und community members für their valuable feedback und suggestions in the community. See [here](https://github.com/kcl-lang/community) to join us!

For more resources, bitte refer to

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.8.0 Milestone](https://github.com/kcl-lang/kcl/milestone/8)
