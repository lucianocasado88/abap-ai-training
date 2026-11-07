[Início - RAP130 - Crie Apps SAP Fiori com ABAP Cloud e SAP Joule for Developers no Visual Studio Code](../../README.md)

# Exercício 6: Criar um Custom Agent _(Opcional)_

## Introdução

Neste exercício, você vai criar um **custom agent** para adaptar o comportamento do seu coding agent ao desenvolvimento ABAP neste workshop. Um custom agent permite definir instruções específicas, convenções de nomenclatura, contexto e preferências de tools, para que o seu agente se comporte de forma consistente sem que você precise repetir a mesma orientação em cada prompt.

Para o desenvolvimento ABAP com o RAP130, um custom agent é útil para instruir o seu coding agent a:
- Sempre usar as ADT MCP tools nas operações ABAP
- Seguir as convenções de nomenclatura RAP e usar o pacote/ID do grupo corretos
- Manter o estilo do código ABAP consistente com as restrições do ABAP Cloud

> ℹ️ **Nota**: Este exercício é feito com o **Claude Code**. O arquivo de instruções do Claude Code se chama `CLAUDE.md` e fica na raiz do projeto.

A documentação oficial traz um [exemplo de custom agent para desenvolvimento ABAP](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agent-configuration?locale=en-US) que ajuda você a começar.

### Exercícios

- [6.1 - Criar as Instruções do Custom Agent](#exercício-61-criar-as-instruções-do-custom-agent-)
- [Resumo](#resumo)

> ℹ️ **Lembrete**: Não se esqueça de substituir todas as ocorrências do placeholder **`###`** pelo seu ID do grupo nos passos do exercício abaixo.

---

## Exercício 6.1: Criar as Instruções do Custom Agent 💎
[^Topo da página](#)

> Crie um arquivo de instruções para personalizar o comportamento do Claude Code no desenvolvimento ABAP.

<details>
  <summary>🔵 Claude Code - Clique para expandir!</summary>

1. Na raiz do seu projeto, crie um novo arquivo chamado **`CLAUDE.md`**.

2. Adicione instruções adaptadas a este workshop RAP130. Por exemplo:

   ```markdown
   # Custom Agent ABAP RAP

   Você é um desenvolvedor ABAP construindo um app SAP Fiori elements transacional usando o framework ABAP RESTful Application Programming Model (RAP) e o ADT MCP Server no Visual Studio Code.

   ## Regras
   - Utilize sempre sintaxe ABAP e APIs compatíveis com a nuvem (cloud).
   - Ao criar classes utilize o prefixo `ZCL`.
   - Utilize o servidor MCP `abap-adt` para criar novos objetos de desenvolvimento ABAP
   e realizar outras tarefas de desenvolvimento, como criar aplicações RAP, executar testes unitários, ativar objetos ABAP, etc.
   - Para localizar objetos de desenvolvimento ABAP por meio de um padrão de nome, utilize a ferramenta de busca de objetos ABAP.
   - Para obter a URI de um objeto ABAP no sistema de arquivos virtual do ABAP (para leitura ou edição), utilize a ferramenta de resolução de URIs de arquivos ABAP ou a ferramenta de listagem de arquivos AFF, caso precise de um *include* específico.
   - Sempre ative o objeto ABAP após realizar edições.
   - Sempre use as ADT MCP tools (`abap_objects_generator_*`, `abap_object_create`, `abap_object_activate` etc.) para todas as operações de backend ABAP. Nunca gere código ABAP manualmente quando uma MCP tool puder fazer isso.
   - O sufixo do meu ID do grupo é `###`. Use esse sufixo em todos os nomes de artefatos (por exemplo, `ZSITFOR_AI_###`, `ZTRAVEL###`, `ZR_TRAVEL###`).
   - Pacote para todos os objetos: `ZSITFOR_AI_###`.
   ```

   > ℹ️ Substitua `###` pelo seu ID do grupo nas instruções acima.

3. **Salve** o arquivo (**`Ctrl+S`** / **`Cmd+S`**).

   > ✅ O Claude Code carrega o `CLAUDE.md` do projeto ao iniciar uma sessão e aplica as instruções aos seus prompts.

</details>

---

## Resumo
[^Topo da página](#)

Neste exercício, você:
- Criou um arquivo de instruções (custom agent) para adaptar o comportamento do seu coding agent ao desenvolvimento ABAP RAP
- Configurou o agente com o seu ID do grupo, o nome do pacote, as convenções de nomenclatura e a preferência pelo uso das ADT MCP tools

Um custom agent economiza tempo em todos os exercícios: você não precisa mais repetir o contexto sobre o seu ID do grupo, o pacote ou as convenções RAP em cada prompt.

**[↑ Voltar ao início do tutorial](../../README.md)**

---
