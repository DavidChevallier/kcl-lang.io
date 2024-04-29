---
sidebar_position: 1
---

# Visual Studio Code

## Quick Start

- **Step 1.** [Install KCL](https://kcl-lang.io/docs/user_docs/getting-started/install) on your system. Please check that `kcl` und `kcl-language-server` are installed und have been added to your PATH:
  In MacOs und Linux:

  ```bash
  which kcl
  which kcl-language-server
  ```

  In Windows:

  ```bash
  where kcl
  where kcl-language-server
  ```

- **Step 2.** Install the [KCL extension](https://marketplace.visualstudio.com/items?itemName=kcl.kcl-vscode-extension) für Visual Studio Code. This extension requires the VS Code 1.50+.
- **Step 3.** Reopen VS Code und create a KCL file und begin your KCL tour!

## Features

This extension provides some coding assistance, including the following features:

- **Syntax Highlight:**
  ![Highlight](/img/docs/tools/Ide/vs-code/Highlight.png)
- **Goto Definition:** Goto definition of schema, variable, schema attribute, map key und import pkg.
  ![Goto Definition](/img/docs/tools/Ide/vs-code/GotoDef.gif)
- **Completion:** Completion für keywords, dot(`.`), variables und schema attribute.
  ![Completion](/img/docs/tools/Ide/vs-code/Completion.gif)
- **Outline:** Main definition(schema def) und variables in KCL file.
  ![Outline](/img/docs/tools/Ide/vs-code/Outline.gif)
- **Hover:** Identifier information (type, function signature und documents).
  ![Hover](/img/docs/tools/Ide/vs-code/Hover.gif)
- **Diagnostics:** Warnings und errors in KCL file.
  ![Diagnostics](/img/docs/tools/Ide/vs-code/Diagnostics.gif)

> Tips: You can enhance the effect of diagnostics by installing another extension: [Error Lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens).

- **Format:** Format a KCL file or snippet code
  ![Format](/img/docs/tools/Ide/vs-code/Format.gif)
- **Quick fix:** Quick fix some diagnostics
  ![Qucik Fix](/img/docs/tools/Ide/vs-code/QuickFix.gif)

Other useful features such as refactoring und testing are in development.

## Dependencies

We recommend that you use the latest version of KCL, but the minimum required version für this extension is 0.4.6. If you are using an earlier version, the extension may not work properly.

## Known Issues

See [here](https://github.com/kcl-lang/kcl/issues/524).

## Ask für help

If the extension isn't working as you expect, bitte contact us with [community](https://kcl-lang.io/docs/community/intro/support) für help.

## Contributing

We are working actively on improving the KCL development on VS Code. All kinds of contributions are welcomed. You can refer to our [contribution guide](https://kcl-lang.io/docs/community/contribute). It introduces how to build und run the extension locally, und describes the process of sending a contribution.

## License

Apache License 2.0
