[Início - Hands On - SAP Inside Track Fortaleza 2026 - Crie Apps SAP Fiori utilizado IA Generativa](https://github.com/lucianocasado88/abap-ai-handson)

# Hands On - SAP Inside Track Fortaleza 2026 - Crie Apps SAP Fiori utilizado IA Generativa

> 🌐 **Nota sobre este material**
>
> Este material é uma adaptação do repositório original [SAP-samples/abap-platform-rap130](https://github.com/SAP-samples/abap-platform-rap130), licenciado sob a [Apache-2.0](LICENSE). Foi adaptado por Luciano Casado para o hands-on do SAP Inside Track Fortaleza 2026. O conteúdo original é de autoria da SAP SE e de seus colaboradores.
>
> **Principais alterações em relação ao original:** tradução para o português (Brasil), uso do Cline e do Claude Code no lugar do GitHub Copilot e novo fluxo de criação do app Fiori com o Fiori MCP Server.
>
> Este é um material da comunidade e **não é um material oficial da SAP**. SAP, SAP Fiori e SAP BTP são marcas registradas da SAP SE.

## Descrição

Este repositório contém o material da sessão hands-on **Hands On - SAP Inside Track Fortaleza 2026 - Crie Apps SAP Fiori utilizado IA Generativa** 

Você vai construir um app SAP Fiori elements transacional para gerenciamento de viagens usando o **ABAP RESTful Application Programming Model (RAP)**, inteiramente a partir do **Visual Studio Code**, com o **ADT MCP Server** gerando e aprimorando a sua aplicação.

Também vamos criar o App Fiori no padrão List Report + Object Page inteiramente a partir do **Visual Studio Code**, com o **Fiori MCP Server** gerando toda a codificação.

**Sumário**
- [Requisitos](#requisitos)
- [Visão geral](#visão-geral)
- [Exercícios](#exercícios)
- [Mais informações](#mais-informações)

## Requisitos

> Para concluir os exercícios práticos, você precisa de:
> - **Visual Studio Code**: faça o download em [https://code.visualstudio.com/](https://code.visualstudio.com/).
>
> - **Node.js** (versão **22 ou superior**, recomendada a versão LTS): necessário para executar o Fiori MCP Server. Faça o download em [https://nodejs.org/](https://nodejs.org/).
>
> - **ADT for Visual Studio Code**: a extensão ADT for Visual Studio Code inclui o ADT MCP Server integrado. Aqui está o [link](https://marketplace.visualstudio.com/items?itemName=SAPSE.adt-vscode) para baixar a extensão. 
>
> - **SAP Fiori Tools - Extension Pack**: conjunto de extensões para desenvolvimento de aplicativos SAP Fiori elements no Visual Studio Code. Aqui está o [link](https://marketplace.visualstudio.com/items?itemName=SAPSE.sap-ux-fiori-tools-extension-pack) para baixar a extensão.
>
> - **Cline**: extensão de **coding agent** para o Visual Studio Code, usada para chamar as MCP tools e ler/editar arquivos ABAP. Aqui está o [link](https://marketplace.visualstudio.com/items?itemName=saoudrizwan.claude-dev) para baixar a extensão.
>
>   > ℹ️ **IMPORTANTE!** O **Cline** é o agente principal testado, utilizado e recomendado neste tutorial. Porém cada usuário pode utilizar o seu agente preferido, desde que suporte MCP local e o virtual workspace filesystem do Visual Studio Code (Claude Code, Codex, Antigravity...).
>
> - Acesso a um sistema **SAP BTP ABAP Environment**, **SAP S/4HANA Cloud Public Edition** ou **SAP S/4HANA Cloud Private Edition** que tenha o [ABAP Flight Reference Scenario](https://github.com/SAP-samples/abap-platform-refscen-flight) importado.
>
>   (*) Atualmente são suportados o SAP BTP ABAP environment, o SAP S/4HANA Cloud Public Edition e o SAP S/4HANA Cloud Private 2025 Edition.
>- Durante o evento, será disponibilizada a chave de integração com o SAP AI Core para utilizar com o Cline.



## Visão geral

> Nesta sessão hands-on, você vai aprender a usar o **ADT MCP Server** e o **Fiori MCP Server** para construir um app SAP Fiori elements transacional inteiramente a partir do Visual Studio Code, com geração assistida por IA, aprimoramento de código e suporte a unit tests, tudo sobre ABAP Cloud e o ABAP RESTful Application Programming Model (RAP).

<details>
  <summary>🔵 Clique para expandir!</summary>

  Este workshop hands-on cobre toda a jornada do desenvolvedor, desde a configuração do ambiente do Visual Studio Code até um app transacional totalmente funcional e aprimorado por IA:

  - Configurar o Visual Studio Code com a extensão ADT for Visual Studio Code e conectar ao seu sistema ABAP Cloud

  - Configurar o **ADT MCP Server** e o **Fiori MCP Server** integrado e conectá-lo ao **seu coding agent**

  - Usar as ADT MCP tools para **gerar um RAP business object completo** (Travel + Booking) e o seu OData UI service, tudo por meio de prompts em linguagem natural

  - Usar as Fiori MCP tools para **gerar a aplicação Fiori** (List Report + Object Page) seguindo o Fiori Elements, tudo por meio de prompts em linguagem natural

  - Explorar e ajustar os artefatos gerados (CDS views, behavior definitions, metadata extensions) diretamente no Visual Studio Code

  - Publicar o service binding e visualizar o **app SAP Fiori elements** no navegador

  - Adicionar validações no backend
</details>

## Exercícios

Siga estes passos para construir um app SAP Fiori usando o ADT MCP Server / Fiori MCP Server e um coding agent (GitHub Copilot ou compatível) no Visual Studio Code.

| Exercícios | -- |
| ------------- | -- |
| [Primeiros Passos](exercises/ex0/README.md)| -- | 
| [Exercício 1 - Parte A: Configurar o ADT MCP Server](exercises/ex01/README.md#parte-a-adt-mcp-server) | -- | 
| [Exercício 1 - Parte B: Configurar o Fiori MCP Server](exercises/ex01/README.md#parte-b-fiori-mcp-server) | -- | 
| [Exercício 2: Gerar o serviço oData no modelo RAP para o App SAP Fiori via MCP Tools](exercises/ex02/README.md)| -- |
| [Exercício 3: Publicar e Visualizar o App Travel via oData UI Metadata](exercises/ex03/README.md)| -- |
| [Exercício 4: Adicionar uma Validação](exercises/ex04/README.md)| -- |
| [Exercício 5: Gerar o App SAP Fiori via Fiori MCP](exercises/ex05/README.md) | -- | 

#### Exercícios Opcionais

| Exercícios | -- |
| ------------- | -- | 
| [Exercício 6: Criar um Custom Agent](exercises/ex06/README.md) | -- | 

## Mais informações

Você pode encontrar mais informações sobre ABAP AI, ABAP Cloud, RAP e ADT for Visual Studio Code aqui:

- [AI in ABAP Cloud](https://help.sap.com/docs/abap-ai/generative-ai-in-abap-cloud/generative-ai-in-abap-cloud?locale=en-US)
- [Agentic AI for ABAP Development](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agentic-ai-development?locale=en-US)
- [ABAP Cloud Roadmap Information - GenAI](https://help.sap.com/docs/abap-cross-product/roadmap-info/genai?locale=en-US)
- [ABAP Development Tools for Visual Studio Code - Official Documentation](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/abap-development-tools-for-visual-studio-code?locale=en-US)
- [ABAP Development Tools for Visual Studio Code: Everything You Need to Know](https://community.sap.com/t5/technology-blog-posts-by-sap/abap-development-tools-for-vs-code-everything-you-need-to-know/ba-p/14258129)
- [Behind the Design: How We Transformed the ABAP Development Tools Architecture to Support More IDEs](https://community.sap.com/t5/technology-blog-posts-by-sap/behind-the-design-how-we-transformed-the-abap-development-tools/ba-p/14258121)
- [Getting Started with ABAP RESTful Application Programming Model (RAP)](https://pages.community.sap.com/topics/abap/rap)
- [SAP MCP Servers, SAP AI Skills, and Claude Plugins (Open Source)](https://github.com/marianfoo/sap-ai-mcp-servers)
