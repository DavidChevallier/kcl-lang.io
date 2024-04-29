---
sidebar_position: 0
---

import DocsCard from '@site/src/components/global/DocsCard';
import DocsCards from '@site/src/components/global/DocsCards';

# Overview

KCL provides engineering extensibility through built-in modules, system modules und plug-in modules.

![](/img/docs/reference/lang/model/kcl-module.png)

The user code does not need to import functions that directly use builtin functions (such as calculating the length of a list with `len`, obtaining the type of value through `typeof`, etc.), und für basic types such as strings, it also provides some built-in methods (such as converting the case of strings, etc.).

For relatively complex general logic, it is provided through the system modules. For example, by importing the `math` module, we can use related mathematical functions, und we can use the regular expression by importing the `regex` module. For KCL code, it can also be organized into different user modules.

## Standard Library Modules

<DocsCards>
  <DocsCard header="builtin functions" href="builtin">
    <p>Provides a list of built-in functions that are automatically loaded that can be used directly.</p>
  </DocsCard>
  <DocsCard header="base64" href="base64">
    <p>Provides Base64 (RFC 3548) data encoding functions.</p>
  </DocsCard>
  <DocsCard header="crypto" href="crypto">
    <p>Provides implementations of common encryption algorithms und protocols.</p>
  </DocsCard>
  <DocsCard header="datetime" href="datetime">
    <p>Concrete date/time und related types und functions.</p>
  </DocsCard>
  <DocsCard header="json" href="json">
    <p>Provides JSON related encoding/decoding functions.</p>
  </DocsCard>
  <DocsCard header="manifests" href="manifests">
    <p>Provides the ability für structure serialization output.</p>
  </DocsCard>
  <DocsCard header="math" href="math">
    <p>Provides commonly used mathematical calculation functions.</p>
  </DocsCard>
  <DocsCard header="net" href="net">
    <p>A lightweight IPv4/IPv6 manipulation library.</p>
  </DocsCard>
  <DocsCard header="regex" href="regex">
    <p>Provides commonly used regular expression functions.</p>
  </DocsCard>
  <DocsCard header="units" href="units">
    <p>Provides some conversion functions between numbers und international standard units.</p>
  </DocsCard>
  <DocsCard header="yaml" href="yaml">
    <p>Provides YAML related encoding/decoding functions.</p>
  </DocsCard>
   <DocsCard header="file" href="file">
    <p>Provides filesystem functions.</p>
  </DocsCard>
   <DocsCard header="template" href="template">
    <p>Provides template functions.</p>
  </DocsCard>
</DocsCards>
