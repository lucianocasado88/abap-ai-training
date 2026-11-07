[Início - Treinamento - Crie Apps SAP Fiori utilizando IA Generativa](../../README.md)

# Exercício 6: Usar um Projeto com Agentes Pré-configurados _(Opcional)_

## Introdução

Nos exercícios anteriores, você pediu tudo ao seu coding agent "do zero", repetindo o contexto em cada prompt. Neste exercício, você vai **baixar um projeto que já vem com os agentes configurados**: um time de agentes especializados em desenvolvimento SAP, mantido por José Victor Martins da Silva ([abap-full-stack-sample](https://github.com/josevictormartinsdasilva/abap-full-stack-sample)).

O projeto usa uma **arquitetura multiagente**: a sessão principal atua como **Orquestrador** (planeja e delega) e quatro especialistas executam o trabalho:

| Agente | Especialidade |
|--------|---------------|
| `abap-rap-dev` | Backend ABAP Cloud / RAP: CDS, behaviors, metadata extensions, service definitions e bindings |
| `abap-classic-dev` | ABAP clássico e RICEFW: reports, function modules, dynpros, dicionário SE11 |
| `fiori-elements-dev` | Frontend SAP Fiori elements e UI5: `manifest.json`, Page Map, anotações, linter |
| `abap-qa-auditor` | Qualidade: ATC, QuickFixes e testes ABAP Unit |

Cada ferramenta lê a configuração de um conjunto diferente de arquivos, e **todos já vêm no projeto**:

| Item | Claude Code | Antigravity |
|------|-------------|-------------|
| Instruções do orquestrador | `CLAUDE.md` (importa o `AGENTS.md`) | `AGENTS.md` |
| Subagentes | `.claude/agents/*.md` | Declarados no `AGENTS.md` |
| Skills | `.claude/skills/*/SKILL.md` | `.agents/skills/*/SKILL.md` |
| Servidores MCP | `.mcp.json` | `.agents/mcp_config.json` |

> ℹ️ **Nota**: Este projeto segue um padrão corporativo de nomenclatura (por exemplo, `ZI_<MD>_*` para CDS views), documentado nos arquivos `padrao-desenvolvimento.md` e `naming-conventions.json` do projeto. Ele **não** é o mesmo padrão do workshop (`ZTRAVEL###`, pacote `ZSITFOR_AI_###`). Por isso, neste exercício o objetivo é **explorar como os agentes trabalham**, e não regenerar o app do workshop.

> ℹ️ **Versão utilizada**: o material foi validado com o commit **`5a08949`** do projeto. Os passos abaixo fixam essa versão para que todos usem exatamente o mesmo conteúdo.

### Exercícios

- [6.1 - Baixar o Projeto com os Agentes](#exercício-61-baixar-o-projeto-com-os-agentes)
- [6.2 - Conectar os MCP Servers ao seu coding agent](#exercício-62-conectar-os-mcp-servers-ao-seu-coding-agent)
- [6.3 - Testar os Agentes](#exercício-63-testar-os-agentes)
- [Resumo](#resumo)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

> ⚠️ **Pré-requisito**: o ADT MCP Server continua rodando **dentro do Visual Studio Code**. Mantenha o Visual Studio Code aberto, com a destination do seu sistema no workspace e o servidor em execução na porta `2236` (_veja_ [Exercício 1.2](../ex01/README.md#exercício-12-verificar-se-o-mcp-server-está-em-execução)). Você também vai precisar do **token** do ADT MCP Server (_veja_ [Exercício 1.1](../ex01/README.md#exercício-11-habilitar-o-adt-mcp-server-nas-configurações-do-visual-studio-code), item 7) e do Node.js 22 ou superior (_veja_ [Exercício 1.4](../ex01/README.md#exercício-14-verificar-os-pré-requisitos-do-fiori-mcp-server)).

---

## Exercício 6.1: Baixar o Projeto com os Agentes 💎
[^Topo da página](#)

> Baixe o projeto do José Victor para uma pasta no seu computador. A estrutura de agentes já vem configurada.

<details>
  <summary>🔵 Claude Code e Antigravity - Clique para expandir!</summary>

1. Escolha uma pasta **fora** do repositório deste treinamento. Abra um terminal nela e execute:

   ```bash
   git clone https://github.com/josevictormartinsdasilva/abap-full-stack-sample.git
   ```

2. Entre na pasta do projeto e fixe a versão validada para o workshop:

   ```bash
   cd abap-full-stack-sample
   ```

   ```bash
   git checkout 5a08949
   ```

   > ℹ️ **Nota**: o Git avisa que você está em _detached HEAD_. Isso é esperado: você está apenas lendo uma versão fixa do projeto.

   > ℹ️ **Sem Git instalado?** Abra o [repositório no GitHub](https://github.com/josevictormartinsdasilva/abap-full-stack-sample), clique em **Code → Download ZIP** e extraia o arquivo numa pasta.

3. Confira a estrutura. As pastas que começam com ponto (`.agents`, `.claude`, `.kimi-code`) ficam **ocultas** por padrão:
   - **macOS**: no Finder, pressione **`Cmd+Shift+.`**
   - **Windows**: no Explorer, em **Exibir → Mostrar → Itens ocultos**
   - **Terminal**: `ls -a` (macOS/Linux) ou `dir /a` (Windows)

   Você deve encontrar, entre outros: `AGENTS.md`, `CLAUDE.md`, `.mcp.json`, `.claude/agents/`, `.agents/skills/` e `.agents/mcp_config.json`.

4. Abra a pasta `abap-full-stack-sample` no seu editor:
   - **Claude Code**: use o terminal do Visual Studio Code, ou um terminal comum, **dentro dessa pasta**. O Claude Code carrega a configuração da pasta onde é iniciado.
   - **Antigravity**: abra a pasta com **File → Open Folder**.

   > ⚠️ **Importante**: não feche o Visual Studio Code com a destination do seu sistema, porque é ele que mantém o ADT MCP Server ativo.

</details>

---

## Exercício 6.2: Conectar os MCP Servers ao seu coding agent
[^Topo da página](#)

> O projeto traz os arquivos de MCP, mas o **token** é pessoal e não vem preenchido. Configure-o na sua máquina.

> ⚠️ **Atenção**: nunca faça commit do seu token em arquivos do projeto.

<details>
  <summary>🔵 Claude Code - Clique para expandir!</summary>

O projeto traz um `.mcp.json` que depende de uma variável de ambiente e foi escrito para Windows. Para evitar esses dois problemas, registre os servidores **na sua pasta**, como no [Exercício 1](../ex01/README.md). O registro local tem prioridade sobre o `.mcp.json` do projeto para servidores de mesmo nome.

1. No terminal, **dentro da pasta `abap-full-stack-sample`**, registre o ADT MCP Server, substituindo **`<SEU_TOKEN>`** pelo seu token:

   ```bash
   claude mcp add --transport http abap-adt http://localhost:2236/mcp --header "Authorization: Bearer <SEU_TOKEN>"
   ```

2. Registre o Fiori MCP Server de acordo com o seu sistema operacional:

   - **Windows**:
     ```
     claude mcp add fiori-mcp -- cmd /c npx --yes @sap-ux/fiori-mcp-server@1.15.19 fiori-mcp
     ```
   - **macOS / Linux**:
     ```
     claude mcp add fiori-mcp -- npx --yes @sap-ux/fiori-mcp-server@1.15.19 fiori-mcp
     ```

3. Inicie o Claude Code **nesta mesma pasta**:

   ```bash
   claude
   ```

   > ℹ️ **Nota**: se o Claude Code perguntar se você confia nos servidores MCP do projeto, **aprove**. Se ele avisar sobre o `.mcp.json`, pode aprovar também: os servidores que você registrou acima têm prioridade.

4. Confirme a conexão digitando:

   ```
   /mcp
   ```

   Os servidores **abap-adt** e **fiori-mcp** devem aparecer como **connected**.

   > ℹ️ Na primeira execução, o `npx` baixa o pacote do Fiori MCP Server e pode levar alguns segundos.

5. Confirme que os agentes foram carregados:

   ```
   /agents
   ```

   Você deve ver os quatro subagentes: `abap-rap-dev`, `abap-classic-dev`, `fiori-elements-dev` e `abap-qa-auditor`.

   > ℹ️ **Alternativa**: você também pode usar o `.mcp.json` do projeto, definindo antes a variável de ambiente `ABAP_ADT_TOKEN` com o seu token e iniciando o `claude` pelo terminal. No macOS/Linux, será preciso trocar o bloco `fiori-mcp` do arquivo, pois ele usa `cmd /c`. O README do projeto detalha esse caminho.

</details>

<details>
  <summary>🔵 Antigravity - Clique para expandir!</summary>

1. Com a pasta `abap-full-stack-sample` aberta no Antigravity, abra o arquivo **`.agents/mcp_config.json`**.

2. Substitua **`<SEU_TOKEN>`** pelo seu token do ADT MCP Server:

   ```JSON
   {
     "mcpServers": {
       "abap-adt": {
         "type": "streamableHttp",
         "url": "http://localhost:2236/mcp",
         "headers": {
           "Authorization": "Bearer <SEU_TOKEN>"
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

   > ⚠️ **Atenção**: altere apenas a **sua cópia local** e **não faça commit** desse arquivo com o token preenchido.

3. Salve o arquivo e abra a lista de **MCP Servers** do Antigravity (painel do agente → menu **MCP Servers**). Clique em **View raw config** e confira se os servidores **abap-adt** e **fiori-mcp** aparecem e estão habilitados.

4. Se a lista de servidores estiver vazia ou com erro, veja a [Solução de problemas](#solução-de-problemas) abaixo.

5. Confirme que o agente leu as instruções do projeto, perguntando no chat do Antigravity:

   ```
   Quais agentes e skills este projeto define? Liste os nomes e o que cada um faz.
   ```

   Você deve ver os quatro papéis (`abap-rap-dev`, `abap-classic-dev`, `fiori-elements-dev` e `abap-qa-auditor`), vindos do `AGENTS.md` e de `.agents/skills/`.

### Solução de problemas

- **Os servidores não aparecem**: o Antigravity pode ler a configuração de MCP de um arquivo **global** em vez do arquivo do projeto. Em **View raw config**, cole o conteúdo do bloco `mcpServers` acima. O caminho exato desse arquivo varia conforme a versão do Antigravity (por exemplo, `~/.gemini/antigravity/mcp_config.json` ou `~/.gemini/config/mcp_config.json`).
- **O `abap-adt` falha ao conectar**: confira se o ADT MCP Server está em execução no Visual Studio Code (Exercício 1.2) e se o token está correto.
- **O `abap-adt` é rejeitado pelo formato do arquivo**: em algumas versões, servidores HTTP remotos usam a chave `serverUrl` no lugar de `type` + `url`. Teste esta variante:
  ```JSON
  "abap-adt": {
    "serverUrl": "http://localhost:2236/mcp",
    "headers": {
      "Authorization": "Bearer <SEU_TOKEN>"
    }
  }
  ```
- **`npx` não reconhecido**: confira o Node.js (Exercício 1.4) e reinicie o Antigravity.

</details>

---

## Exercício 6.3: Testar os Agentes
[^Topo da página](#)

> Faça um teste que **só lê** informações do sistema, para ver o orquestrador e um especialista trabalhando.

<details>
  <summary>🔵 Claude Code e Antigravity - Clique para expandir!</summary>

1. No chat do seu coding agent, peça um **plano**, sem criar nada. Substitua **`###`** pelo seu ID do grupo:

   ```
   Planeje, sem executar, como você criaria uma entidade CDS de consumo para o pacote ZSITFOR_AI_###. Mostre os nomes dos objetos conforme o padrão do projeto e diga qual agente faria cada etapa.
   ```

   O orquestrador deve consultar `padrao-desenvolvimento.md` e `naming-conventions.json`, apresentar um **plano com os nomes dos objetos** e esperar a sua aprovação. Você deve ver os nomes no padrão do projeto (por exemplo, `ZI_<MD>_*` e `ZC_<MD>_*`).

   > ℹ️ **Nota**: o `<MD>` é o código do módulo do padrão corporativo. Se o agente perguntar, informe um valor de exemplo, como `FS`.

2. Se você já concluiu o [Exercício 2](../ex02/README.md), peça ao auditor para **inspecionar** o seu pacote:

   ```
   Peça ao abap-qa-auditor para rodar a inspeção ATC no pacote ZSITFOR_AI_### e relatar os apontamentos encontrados, sem aplicar correções.
   ```

   **Permita** as chamadas das tools quando solicitado.

   > ℹ️ **Nota**: dependendo do seu sistema, as tools de ATC podem não estar disponíveis. Se o agente informar isso, o comportamento está correto: ele deve relatar a falha, e não inventar resultados.

3. Observe como o trabalho foi dividido. No **Claude Code**, o orquestrador delega aos subagentes. No **Antigravity**, os papéis são seguidos a partir do `AGENTS.md` e das skills.

   > ✅ Se o agente apresentou um plano com nomes de objetos e indicou o especialista de cada etapa, a estrutura multiagente está funcionando!

</details>

---

## Resumo
[^Topo da página](#)

Neste exercício, você:
- Baixou um projeto que já traz um time de agentes especializados configurados para Claude Code e Antigravity
- Conectou o ADT MCP Server e o Fiori MCP Server ao projeto, usando o seu token pessoal
- Viu o orquestrador planejar e delegar o trabalho entre os agentes de backend, frontend e qualidade

Em vez de escrever as instruções do zero em cada projeto, você parte de uma estrutura pronta e a adapta ao seu contexto: basta editar o `AGENTS.md` ou o `CLAUDE.md` e as skills, mantendo as duas ferramentas sincronizadas.

**[↑ Voltar ao início do tutorial](../../README.md)**

---
