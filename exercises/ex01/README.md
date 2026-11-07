[Início - Hands On - SAP Inside Track Fortaleza 2026 - Crie Apps SAP Fiori utilizado IA Generativa](../../README.md)

# Exercício 1: Configurar os MCP Servers (ADT e Fiori) 💎

## Introdução

No exercício anterior, você instalou o Visual Studio Code e a extensão ADT for Visual Studio Code, e se conectou ao seu sistema ABAP (_veja [Primeiros Passos](../ex0/README.md)_).

Neste exercício, você vai configurar os dois MCP Servers usados no workshop e verificar se as MCP tools estão disponíveis no seu coding agent:

- **Parte A – ADT MCP Server**: integrado à extensão ADT for Visual Studio Code. Expõe recursos de desenvolvimento ABAP como **Model Context Protocol (MCP) tools**, permitindo criar pacotes, criar transports, gerar aplicações RAP completas, ativar objetos e muito mais por meio de prompts em linguagem natural. Para mais informações, veja [Agentic AI for ABAP Development](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agentic-ai-development?locale=en-US)

- **Parte B – Fiori MCP Server**: servidor open source da SAP (`@sap-ux/fiori-mcp-server`) que permite gerar e modificar aplicações SAP Fiori elements a partir de prompts. Ele será usado mais adiante para gerar o app Fiori sobre o serviço OData criado com o ADT MCP Server.

### Exercícios

**Parte A: ADT MCP Server**
- [1.1 - Habilitar o ADT MCP Server nas Configurações do Visual Studio Code](#exercício-11-habilitar-o-adt-mcp-server-nas-configurações-do-visual-studio-code)
- [1.2 - Verificar se o MCP Server Está em Execução](#exercício-12-verificar-se-o-mcp-server-está-em-execução)
- [1.3 - Inserir as ADT MCP Tools no seu coding agent](#exercício-13-inserir-as-adt-mcp-tools-no-seu-coding-agent)

**Parte B: Fiori MCP Server**
- [1.4 - Verificar os pré-requisitos do Fiori MCP Server](#exercício-14-verificar-os-pré-requisitos-do-fiori-mcp-server)
- [1.5 - Inserir o Fiori MCP Server no seu coding agent](#exercício-15-inserir-o-fiori-mcp-server-no-seu-coding-agent)
- [1.6 - Verificar as Fiori MCP Tools](#exercício-16-verificar-as-fiori-mcp-tools)

- [Resumo e Próximo Exercício](#resumo-e-próximo-exercício)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

# Parte A: ADT MCP Server

## Sobre o ADT MCP Server 💎

O **ADT MCP Server** é um servidor HTTP local que roda dentro da extensão ADT for Visual Studio Code. Ele implementa o **Model Context Protocol (MCP)**, um padrão aberto que permite que assistentes de IA (como o GitHub Copilot) chamem tools de forma estruturada e autenticada.

Quando habilitado, o MCP server expõe um conjunto de ferramentas de desenvolvimento ABAP para qualquer cliente de IA compatível com MCP. Neste workshop, os exercícios usam o **GitHub Copilot** como cliente. Qualquer coding agent que suporte o virtual workspace filesystem do Visual Studio Code é compatível: o GitHub Copilot está confirmado, e outros também são suportados.

**Principais tools disponíveis via ADT MCP Server:**

| Tool | Descrição |
|------|-----------|
| `abap_activate-objects` | Ativa objetos ABAP no sistema backend |
| `abap_business_services-fetch_service_information` | Busca os metadados de um OData service |
| `abap_creation-create_object` | Cria objetos de desenvolvimento ABAP |
| `abap_generators-list_generators` | Lista os RAP generators ABAP disponíveis |
| `abap_generators-get_schema` | Recupera o JSON schema de um generator |
| `abap_generators-generate_objects` | **Executa um RAP generator**: a tool principal do Exercício 2 |
| `abap_run_unit_tests` | Executa ABAP unit tests para um conjunto de objetos |
| `abap_transport-create` | Cria uma transport request |
| `abap_transport-get` | Obtém as transport requests relevantes para um objeto |

Veja [ADT MCP Tools](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/mcp-tools?locale=en-US) para a lista completa de tools disponíveis.

> ⚠ **Aviso sobre resultados gerados por IA** ⚠
> O ADT MCP Server é um **recurso experimental** que pode mudar a qualquer momento sem aviso prévio. Ele não se destina a uso produtivo. Faça backup dos seus dados antes de utilizá-lo.
>
> **Leitura adicional**: [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

---


## Exercício 1.1: Habilitar o ADT MCP Server nas Configurações do Visual Studio Code
[^Topo da página](#)

> Habilite o ADT MCP Server integrado nas configurações da extensão do Visual Studio Code.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra as **Configurações do Visual Studio Code**:
   - Abra o **Command Palette** com **`Ctrl+Shift+P`** (macOS: **`Cmd+Shift+P`**)
   - Digite `Preferences: Open Settings (UI)` e selecione **Preferences: Open Settings...**.

2. Na barra de pesquisa, digite:
   ```
   adt mcp
   ```

3. Localize a configuração **"Adt: Enable MCP Server"** (ou similar) e **habilite-a** marcando a caixa de seleção.

   > ℹ️ **Dica**: Você também pode alternar para o modo JSON (clique no ícone `{}` no canto superior direito das Settings) e adicionar:
   > ```json
   >  "adt.mcpServer.enabled": true
   > ```

   ![Habilitar MCP](images/ex1_mcp_setting_2.png)

4. Opcionalmente, você pode configurar a **porta do MCP server** (padrão: `2236`). Altere apenas se a porta 2236 estiver ocupada na sua máquina:
   - Pesquise por `ADT MCP port` nas Settings
   - Defina qualquer porta disponível entre `0` e `65535`

   ![Porta MCP](images/ex1_mcp_setting_3.png)

5. **Recarregue o Visual Studio Code** se for solicitado, ou feche e abra o aplicativo novamente.

   ![Configuração do ADT MCP Server habilitada](images/ex1_mcp_setting.png)

6. Caso vá utilizar algum agente externo como Claude Code ou Codex por exemplo, obter o token gerado nessa mesma tela para utilização posterior.

   ![Token MCP](images/ex1_mcp_adt_token.png)

> ⚠️ **Importante**: Certifique-se de que a destination do seu sistema ainda está adicionada ao workspace (do Exercício 0.8). O MCP server só inicia quando há uma destination ativa no workspace.

</details>

---

## Exercício 1.2: Verificar se o MCP Server Está em Execução
[^Topo da página](#)

> Confirme que o ADT MCP Server iniciou com sucesso.

<details>
  <summary>🔵 Clique para expandir!</summary>

Depois de habilitar a configuração e ter uma destination no workspace, o servidor deve iniciar automaticamente.

### Método 1: Verificar a notificação de inicialização

1. Observe o **canto inferior direito** do Visual Studio Code para ver uma notificação:
   ```
   ADT MCP Server running on port 2236
   ```

   ![ADT MCP Server em execução](images/ex1_mcp_running.png)

   > ℹ️ Se você não vir a notificação, siga para o Método 2.


### Método 2: Verificar pela lista de MCP Servers

1. Abra o **Command Palette** (**`Ctrl+Shift+P`**).

2. Digite:
   ```
   >MCP: List Servers
   ```

3. Selecione **"MCP: List Servers"** na lista.

4. Você deve ver uma entrada para o **ADT MCP Server** na lista de seleção rápida (quick pick).

5. Selecione-a e clique em **Start Server** caso ele ainda não esteja em execução.

   ![MCP Server listado e em execução](images/ex1_mcp_list_servers.png)

### Solução de problemas

Se o ADT MCP Server não aparecer ou falhar ao iniciar:

1. **Desabilite e habilite novamente** a configuração "Adt: Enable MCP Server".
2. **Remova** a pasta da destination do workspace e **adicione-a novamente** (Command Palette → `ABAP: Add Destination as Folder to the Workspace...`).
3. **Reinicie** o Visual Studio Code.

</details>

---

## Exercício 1.3: Inserir as ADT MCP Tools no seu coding agent
[^Topo da página](#)

> Abra o seu coding agent em agent mode e confirme que as ADT MCP tools estão carregadas e disponíveis.

<details>
  <summary>🔵 Cline - Clique para expandir!</summary>

1. Abra o **Cline** no Visual Studio Code:
   - Clique no **ícone do Cline** na Activity Bar (esquerda)

      ![Cline](images/ex1_cline.png)

   - Clique no **ícone de customização** no topo direito da tela do Cline

      ![Cline customização](images/ex1_cline_1.png)
   
   - Clique na aba **MCP** 

      ![Cline MCP](images/ex1_cline_3.png)

   - Clique em **Edit Configuration**

      ![Cline configurações do MCP](images/ex1_cline_4.png)

2. Deverá substituir o JSON atual pelo seguinte JSON:

   ```JSON
   {
      "mcpServers": {
         "abap-adt": {
            "type": "streamableHttp",
            "url": "http://localhost:2236/mcp",
            "headers": {
               "Authorization": "Bearer <TOKEN>"
            },
            "disabled": false
         }
      }
   }
   ```

   > ℹ️ **Nota**: Informar o seu token obtido no passo do item 6 do tópico 1.1 (_veja_ [Exercício 1.1: Habilitar o ADT MCP Server nas Configurações do Visual Studio Code](#exercício-11-habilitar-o-adt-mcp-server-nas-configurações-do-visual-studio-code)).


3. **Verifique se o MCP está ativo**: na aba **MCP** do Cline, o servidor **`abap-adt`** deve aparecer na lista com o botão de ativação ligado e uma **bolinha verde** ao lado.

   ![MCP abap-adt ativo no Cline](images/ex1_cline_mcp_ativo.png)

   > ℹ️ **Nota**: Se a bolinha não estiver verde, confira se o ADT MCP Server está em execução (Exercício 1.2) e se o token no JSON está correto. Depois clique no ícone de recarregar ao lado do servidor.

4. **Teste rápido (sanity test)**: digite o prompt a seguir no chat do seu coding agent e pressione **Enter**:
   ```
   Liste todos os ABAP RAP generators disponíveis usando a MCP tool.
   ```

   O seu coding agent vai pedir permissão para chamar a tool `abap_generators-list_generators`. **Permita** a chamada da tool quando solicitado.

   Você deve ver uma lista de generators disponíveis, incluindo uma entrada chamada **"OData UI Service from Scratch"**.

   > ✅ Se você vir os nomes dos generators, o ADT MCP Server está funcionando corretamente com o seu coding agent!
   
   ![ABAP RAP Generators disponíveis](images/ex1_available_generators.png)

</details>

<details>
  <summary>🔵 Claude Code - Clique para expandir!</summary>

1. Para iniciar a configuração, você precisa do token gerado automaticamente no Visual Studio Code. Falamos desse ponto no item 6 do tópico 1.1 (_veja_ [Exercício 1.1: Habilitar o ADT MCP Server nas Configurações do Visual Studio Code](#exercício-11-habilitar-o-adt-mcp-server-nas-configurações-do-visual-studio-code)).

2. Abra um **terminal** (ou PowerShell no Windows) e execute o comando abaixo, substituindo **`<SEU_TOKEN>`** pelo token obtido no passo anterior:

   ```bash
   claude mcp add --transport http abap-adt http://localhost:2236/mcp --header "Authorization: Bearer <SEU_TOKEN>"
   ```

   > ℹ️ **Nota**: Esse comando registra o ADT MCP Server no Claude Code apenas para a pasta (projeto) em que você executou o comando. Para usá-lo em qualquer pasta, acrescente `-s user` logo após `claude mcp add`.

   > ⚠️ **Atenção**: não grave o token em arquivos do projeto que possam ser enviados ao Git.

3. Confirme que o servidor foi registrado:

   ```bash
   claude mcp list
   ```

   Você deve ver o **abap-adt** com o status ✓ **Connected**.

   > ℹ️ O ADT MCP Server só responde enquanto o Visual Studio Code estiver aberto com a destination do seu sistema no workspace.

4. Abra o **Claude Code** e digite o prompt a seguir para testar:

   ```
   Liste todos os ABAP RAP generators disponíveis usando a MCP tool.
   ```

   **Permita** a chamada da tool quando solicitado. Você deve ver uma lista de generators, incluindo uma entrada chamada **"OData UI Service from Scratch"**.

</details>

---

# Parte B: Fiori MCP Server

## Sobre o Fiori MCP Server 💎

O **Fiori MCP Server** ([`@sap-ux/fiori-mcp-server`](https://www.npmjs.com/package/@sap-ux/fiori-mcp-server)) é um MCP server open source da SAP que ajuda o coding agent a **criar e modificar aplicações SAP Fiori elements** a partir de prompts em linguagem natural.

Diferente do ADT MCP Server, ele **não roda dentro do Visual Studio Code**: o próprio coding agent inicia o servidor localmente via `npx` (transporte `stdio`). Por isso **não há porta nem token** para configurar.

**Principais tools disponíveis via Fiori MCP Server:**

| Tool | Descrição |
|------|-----------|
| `search_docs` | Pesquisa na documentação de SAP Fiori elements, Annotations, UI5 e SAP Fiori tools |
| `list_sap_systems` | Lista os sistemas SAP salvos no ambiente do usuário |
| `download_odata_service_metadata` | Baixa o metadata (EDMX) de um OData service de um sistema SAP |
| `generate_fiori_app_odata` | **Gera um app SAP Fiori** para um OData service (por exemplo, um serviço RAP) |
| `list_fiori_apps` | Procura apps SAP Fiori existentes em um diretório |
| `list_functionality` | Lista as modificações suportadas para um app existente |
| `get_functionality_details` | Detalha os parâmetros de uma modificação |
| `execute_functionality` | Executa uma modificação no app |

Veja o [README oficial do Fiori MCP Server](https://github.com/SAP/open-ux-tools/tree/main/packages/fiori-mcp-server) para a lista completa e atualizada.

---

## Exercício 1.4: Verificar os pré-requisitos do Fiori MCP Server
[^Topo da página](#)

> Confirme que o Node.js está instalado na versão exigida pelo Fiori MCP Server.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. Abra um terminal no Visual Studio Code (**`Ctrl+'`**) e execute:
   ```
   node -v
   ```

2. A versão exibida deve ser **v22 ou superior** (por exemplo, `v22.x.x` ou `v24.x.x`).

   > ⚠️ **Importante**: Se o comando não for reconhecido, ou a versão for inferior à 22, instale a versão **LTS** do Node.js em [nodejs.org](https://nodejs.org/) e **reinicie o Visual Studio Code** para que o novo `PATH` seja carregado.

3. Confirme também que o `npx` está disponível (ele é instalado junto com o Node.js):
   ```
   npx -v
   ```

</details>

---

## Exercício 1.5: Inserir o Fiori MCP Server no seu coding agent
[^Topo da página](#)

> Adicione o Fiori MCP Server à configuração de MCP do seu coding agent, ao lado do ADT MCP Server.

> ℹ️ **Versão**: os exemplos abaixo fixam a versão **`1.15.19`** para que todos os participantes usem o mesmo comportamento. Para usar sempre a mais recente, troque por `@latest`.

<details>
  <summary>🔵 Cline - Clique para expandir!</summary>

1. Abra novamente a configuração de MCP do Cline, como no [Exercício 1.3](#exercício-13-inserir-as-adt-mcp-tools-no-seu-coding-agent): **ícone de customização** → aba **MCP** → **Edit Configuration**.

2. **Adicione** a entrada `fiori-mcp` dentro de `mcpServers`, **mantendo** a entrada `abap-adt` configurada anteriormente. O JSON final deve ficar assim:

   ```JSON
   {
      "mcpServers": {
         "abap-adt": {
            "type": "streamableHttp",
            "url": "http://localhost:2236/mcp",
            "headers": {
               "Authorization": "Bearer <TOKEN>"
            },
            "disabled": false
         },
         "fiori-mcp": {
            "type": "stdio",
            "timeout": 600,
            "command": "npx",
            "args": ["--yes", "@sap-ux/fiori-mcp-server@1.15.19", "fiori-mcp"],
            "disabled": false
         }
      }
   }
   ```

   > ⚠️ **Atenção**: não substitua o JSON inteiro, senão a configuração do ADT MCP Server será perdida. Repare na **vírgula** entre o fechamento de `abap-adt` e a entrada `fiori-mcp`.

3. Salve o arquivo. Na aba **MCP** → **Installed** do Cline, o servidor **fiori-mcp** deve aparecer junto com o **abap-adt**.

   > ℹ️ Na primeira inicialização, o `npx` baixa o pacote do npm. Isso pode levar alguns segundos.

</details>

<details>
  <summary>🔵 Claude Code - Clique para expandir!</summary>

1. No terminal, dentro da pasta do seu projeto, execute:

   - **Windows**:
     ```
     claude mcp add fiori-mcp -- cmd /c npx --yes @sap-ux/fiori-mcp-server@1.15.19 fiori-mcp
     ```
   - **macOS / Linux**:
     ```
     claude mcp add fiori-mcp -- npx --yes @sap-ux/fiori-mcp-server@1.15.19 fiori-mcp
     ```

2. Confirme que o servidor foi adicionado e está conectado:
   ```
   claude mcp list
   ```

   Você deve ver o **fiori-mcp** com o status ✓ **Connected**.

</details>

---

## Exercício 1.6: Verificar as Fiori MCP Tools
[^Topo da página](#)

> Confirme que o coding agent consegue chamar as tools do Fiori MCP Server.

<details>
  <summary>🔵 Clique para expandir!</summary>

1. **Teste rápido (sanity test)**: digite o prompt a seguir no chat do seu coding agent e pressione **Enter**:
   ```
   Usando o Fiori MCP Server, pesquise na documentação como habilitar o "initial load" em um List Report do SAP Fiori elements.
   ```

2. O seu coding agent vai pedir permissão para chamar a tool `search_docs`. **Permita** a chamada da tool quando solicitado.

   > ℹ️ **Primeiro uso**: a tool `search_docs` baixa um modelo de busca semântica de aproximadamente **86 MB** na primeira execução. Se a rede estiver lenta, a primeira resposta pode demorar; as próximas usam o cache local.

3. Você deve receber uma resposta com trechos da documentação do SAP Fiori elements.

   > ✅ Se o coding agent chamou a `search_docs` e trouxe a resposta, o Fiori MCP Server está funcionando corretamente!

### Solução de problemas

- **`npx` não reconhecido / servidor não inicia**: confira o Node.js (Exercício 1.4) e reinicie o Visual Studio Code.
- **Timeout na primeira execução**: aguarde o download do pacote e tente novamente; o `timeout` de 600 segundos na configuração existe para isso.
- **Proxy corporativo**: se a rede bloquear o npm, configure o proxy com `npm config set proxy` / `npm config set https-proxy`.

</details>

---

## Resumo e Próximo Exercício
[^Topo da página](#)

Agora que você:
- Habilitou o ADT MCP Server nas configurações da extensão do Visual Studio Code
- Verificou que o servidor está em execução (via notificação ou `MCP: List Servers`)
- Confirmou que as ADT MCP tools estão visíveis e podem ser chamadas pelo seu coding agent em agent mode
- Adicionou o Fiori MCP Server ao seu coding agent e confirmou que as Fiori MCP tools respondem

você pode continuar com o próximo exercício: **[Exercício 2: Gerar o App SAP Fiori via MCP Tools](../ex02/README.md)**

---

