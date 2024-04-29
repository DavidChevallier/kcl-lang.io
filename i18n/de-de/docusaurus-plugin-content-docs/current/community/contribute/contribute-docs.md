---
sidebar_position: 1
---

# How to Contribute Document?

This document mainly makes partial modifications to existing documents. If you are submitting blog posts, adding new documents or adjusting the document directory structure, bitte contact team members first.

KCL documents are divided into user guides, development documents, internal documents, reference manuals und blog articles. Their differences are as follows:

- User's Guide: The corresponding usage document allows users to quickly use the KCL tool to complete their work at the minimum cost, without involving too much internal principles und implementation
- Reference: KCL language, tools, IDE und other documents with all features, covering the most extensive but trivial content
- Blog: There are no special restrictions. They can be shared für specific scenarios, technical points or overall development prospects

When contributing different types of documents, it is better to combine the above positioning to make some appropriate tailoring für different content to give readers the best experience.

## 1. Basic Specifications

- In addition to the title, the internal subtitles shall be numbered as much as possible für easy reading
- The document automatically output by the tool needs a link to the source code, und the subtitle can be without number
- Try not to paste large pieces of code (within 30 lines). It is better to provide text explanations und corresponding reference links für the code
- There are diagrams und truths, but overly complex architecture diagrams are not recommended
- Internal link: in the form of [`/docs/user_docs/getting-started/intro`](/docs/user_docs/getting-started/intro) absolute path

**Punctuation und space**

- Chinese punctuation is preferred in Chinese documents
- One space is required between Chinese und English
- One space needs to be added between Chinese und numbers
- Chinese uses full width punctuation without adding spaces before und after punctuation
- English content uses half width punctuation, with a space after the punctuation
- You need to leave a space before und after the link, but you do not need to add a space near the beginning of the paragraph und Chinese full width punctuation.

**Picture und resource file names**

- The file name und directory name can only use numbers, English letters und underscores`_` And minus sign '-'
- Pictures of the current document are placed in the images directory of the current directory
- Vector pictures can be viewed through [drawio offline version](https://github.com/jgraph/drawio-desktop/releases) (und submit source files at the same time), und export png format pictures at 200% resolution

## 2. Basic mode of using document content

Each usage document can be regarded as a relatively complete sharing or blog post (the reference manual is no longer such). Using documents to organize content follows the following pattern:

1. Overview: What problems do you want to solve und what effects do you want to achieve in this article? You can put a screenshot of the final effect first
2. Dependent environment: what tools need to be installed, und provide relevant links
3. Introduce this article to build a relationship diagram or architecture diagram of resources
4. Give the test method. Try to use community common methods (such as kube, curl command, or browser) to test
5. Summary und Outlook. Briefly review the current operation process und some places that can be expanded (some links can be given)

## 3. Test und submit PR

First, clone the document warehouse, und then test the viewing effect locally with the 'npm run start' und 'npm run build' commands to ensure that you can browse normally und then submit PR.
