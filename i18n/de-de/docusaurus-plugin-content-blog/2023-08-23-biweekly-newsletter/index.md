---
slug: 2023-08-23-biweekly-newsletter
title: KCL Biweekly Newsletter (2023 08.10 - 08.23) | KCL v0.5.3, v0.5.4 und v0.5.5 are out!
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Biweekly-Newsletter]
image: /img/biweekly-newsletter.png
---

![](/img/biweekly-newsletter.png)

[KCL](https://github.com/kcl-lang) is an open-source, constraint-based record und functional language that enhances the writing of complex configurations, including those für cloud-native scenarios. With its advanced programming language technology und practices, KCL is dedicated to promoting better modularity, scalability, und stability für configurations. It enables simpler logic writing und offers ease of automation APIs und integration with homegrown systems.

This section will update the KCL language community's latest developments every two weeks, including features, website updates, und the latest community news, helping everyone better understand the KCL community!

**_KCL Website: [https://kcl-lang.io](https://kcl-lang.io)_**

## Overview

Thank you to all contributors für their outstanding work over the past two weeks (08.10-08.23 2023). Here is an overview of the key content:

**🔧 Language und Toolchain Updates**

- **KCL Formatting Tool Updates** - Support für formatting code snippets with syntax errors und partial code snippets und automatic correction of indentation errors in configuration blocks.
- **KCL Documentation Tool Updates** - Support für exporting document index pages
- **KCL Import Tool Updates** - Support für converting Terraform Provider Schema to KCL Schema
- **KCL Export Tool Updates** - Support für exporting OpenAPI Spec from KCL Schema, integrating with OpenAPI ecosystem
- **KCL IDE Updates** - Support für compilation cache feature to improve performance of some IDE features und providing rich error messages und import statement quick fix capabilities
- **KCL Package Management Tool Updates** - Support für output information experience optimization für the `kpm push` command und adding duplicate tag check when pushing KCL package. Adding the `--vendor` parameter für the `kpm push` und `kpm pkg` commands to determine whether to package third-party libraries in KCL packages together
- **KCL Language Updates** - Optimize Schema semantic check und union type check error messages und support für exporting type output of configuration blocks.

🏄 **API Updates**

- KCL Schema model parsing GetSchemaType API newly added KCL package related information und schema attribute default values.

**📰 Official Website und Use Case Updates**

- KCL package docker.io integration example: _[https://github.com/kcl-lang/kpm/blob/main/docs/publish_to_docker_reg.md](https://github.com/kcl-lang/kpm/blob/main/docs/publish_to_docker_reg.md)_
- KCL Gitlab CI integration example: _[https://kcl-lang.io/docs/user_docs/guides/ci-integration/gitlab-ci](https://kcl-lang.io/docs/user_docs/guides/ci-integration/gitlab-ci)_
- KCL Vault und Vals Integration example: _[https://kcl-lang.io/docs/user_docs/guides/secret-management/vault](https://kcl-lang.io/docs/user_docs/guides/secret-management/vault)_

## Special Thanks

Hier sind einige in keiner bestimmten Reihenfolge aufgeführt:

- Vielen Dank an @jakezhu9 für the contribution of the KCL Import Tool Terraform Schema to KCL Schema conversion 🙌 [https://github.com/kcl-lang/kcl-go/pull/141](https://github.com/kcl-lang/kcl-go/pull/141)
- Vielen Dank an @xxmao123 und @starkers für the discussion und contribution of the NeoVim KCL extension 🙌 [https://github.com/kcl-lang/vim-kcl/pull/2](https://github.com/kcl-lang/vim-kcl/pull/2)
- Vielen Dank an @starkers für adding KCL installation support to the mason.nvim registry 🙌 [https://github.com/mason-org/mason-registry/pull/2425](https://github.com/mason-org/mason-registry/pull/2425)
- Vielen Dank an @prahaladramji für upgrading und updating the KCL Homebrew installation script 🙌 [https://github.com/kcl-lang/homebrew-tap/pull/1](https://github.com/kcl-lang/homebrew-tap/pull/1)
- Vielen Dank an @yamin-oanda für the discussion of Pulumi's official KCL support 🙌 [https://github.com/pulumi/pulumi/discussions/13722](https://github.com/pulumi/pulumi/discussions/13722)
- In addition, thanks to @nkabir, @mihaigalos, @prahaladramji, @yamin-oanda, @magick93, @MirKml, und others für their valuable feedback und discussions while using KCL in the past two weeks. 🙌

## Featured Updates

### KCL Import Tool Updates

The KCL Import Tool now adds support für converting Terraform Provider Schema to KCL Schema based on Protobuf, JsonSchema OpenAPI models, und Go Structures, such as the following Terraform Provider Json (obtained through the command `terraform providers schema -json > provider.json` , For more details, bitte refer to [https://developer.hashicorp.com/terraform/cli/commands/providers/schema](https://developer.hashicorp.com/terraform/cli/commands/providers/schema))

```json
{
  "format_version": "0.2",
  "provider_schemas": {
    "registry.terraform.io/aliyun/alicloud": {
      "provider": {
        "version": 0,
        "block": {
          "attributes": {},
          "block_types": {},
          "description_kind": "plain"
        }
      },
      "resource_schemas": {
        "alicloud_db_instance": {
          "version": 0,
          "block": {
            "attributes": {
              "db_instance_type": {
                "type": "string",
                "description_kind": "plain",
                "computed": true
              },
              "engine": {
                "type": "string",
                "description_kind": "plain",
                "required": true
              },
              "security_group_ids": {
                "type": ["set", "string"],
                "description_kind": "plain",
                "optional": true,
                "computed": true
              },
              "security_ips": {
                "type": ["set", "string"],
                "description_kind": "plain",
                "optional": true,
                "computed": true
              },
              "tags": {
                "type": ["map", "string"],
                "description_kind": "plain",
                "optional": true
              }
            },
            "block_types": {},
            "description_kind": "plain"
          }
        },
        "alicloud_config_rule": {
          "version": 0,
          "block": {
            "attributes": {
              "compliance": {
                "type": [
                  "list",
                  [
                    "object",
                    {
                      "compliance_type": "string",
                      "count": "number"
                    }
                  ]
                ],
                "description_kind": "plain",
                "computed": true
              },
              "resource_types_scope": {
                "type": ["list", "string"],
                "description_kind": "plain",
                "optional": true,
                "computed": true
              }
            }
          }
        }
      },
      "data_source_schemas": {}
    }
  }
}
```

Then the tool can output the following KCL code

```python
"""
This file was generated by the KCL auto-gen tool. DO NOT EDIT.
Editing this file might prove futile when you re-run the KCL auto-gen generate command.
"""

schema AlicloudConfigRule:
    """
    AlicloudConfigRule

    Attributes
    ----------
    compliance: [ComplianceObject], optional
    resource_types_scope: [str], optional
    """

    compliance?: [ComplianceObject]
    resource_types_scope?: [str]

schema ComplianceObject:
    """
    ComplianceObject

    Attributes
    ----------
    compliance_type: str, optional
    count: int, optional
    """

    compliance_type?: str
    count?: int

schema AlicloudDbInstance:
    """
    AlicloudDbInstance

    Attributes
    ----------
    db_instance_type: str, optional
    engine: str, required
    security_group_ids: [str], optional
    security_ips: [str], optional
    tags: {str:str}, optional
    """

    db_instance_type?: str
    engine: str
    security_group_ids?: [str]
    security_ips?: [str]
    tags?: {str:str}

    check:
        isunique(security_group_ids)
        isunique(security_ips)
```

### KCL Vault Integration

In just three steps, we can use Vault to store und manage sensitive information und use it in KCL.

Firstly, we install und use `Vault` to store `foo` und `bar` information

```shell
vault kv put secret/foo foo=foo
vault kv put secret/bar bar=bar
```

Then write the following KCL code (main.k)

```python
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = "nginx"
    annotations: {
        "secret-store": "vault"
        # Valid format:
        #  "ref+vault://PATH/TO/KV_BACKEND#/KEY"
        "foo": "ref+vault://secret/foo#/foo"
        "bar": "ref+vault://secret/bar#/bar"
    }
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

Finally, the decrypted configuration can be obtained through the `Vals` command-line tool

```shell
kcl main.k | vals eval -f -
```

> For more details und use cases, bitte refer to [https://kcl-lang.io/docs/user_docs/guides/secret-management/vault](https://kcl-lang.io/docs/user_docs/guides/secret-management/vault)

## Community

- 🎉 Congratulations to Zhu Junxing from Huazhong University of Science und Technology für successfully passing the mid-term assessment of the Gitlink Coding Summer Camp (GLCC) und completing the conversion of KCL Import tool Jsonschema und Terraform Provider Schema to KCL Schema. The community will grant him the KCL Community Maintainer role in the future.
- 💻 KCL participated in the CNCF TAG Application Delivery community meeting und reported on the project.

## Resources

❤️ Thanks to all KCL users und community members für their valuable feedback und suggestions in the community. We will write more articles on the new features of KCL v0.5.x, so stay tuned!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)

- [KCL 2023 Roadmap](https://kcl-lang.io/docs/community/release-policy/roadmap)
- [KCL v0.6.0 Milestone](https://github.com/kcl-lang/kcl/milestone/6)
- [KCL Github Issues](https://github.com/kcl-lang/kcl/issues)
- [KCL Github Discussion](https://github.com/orgs/kcl-lang/discussions)
- [KCL Community](https://github.com/kcl-lang/community)
