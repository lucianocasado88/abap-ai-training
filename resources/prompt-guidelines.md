# Diretrizes de Prompts para o RAP130 💎

Este arquivo traz diretrizes e exemplos de prompts para usar de forma eficaz um **coding agent com ADT MCP tools** (exemplos mostrados com o GitHub Copilot) durante o workshop RAP130.

---

## Princípios gerais

- Sempre especifique explicitamente os **nomes dos objetos** e o **sufixo `###`** no seu prompt
- Especifique o **pacote de destino** ao criar ou gerar objetos
- Permita as chamadas de MCP tools quando o Copilot solicitar, revisando os parâmetros antes de aprovar
- Se uma geração produzir resultados inesperados, refine o prompt e tente novamente

---

## Exemplos de prompts por exercício


### Exercício 2: Listar generators

```
Liste todos os ABAP RAP generators disponíveis usando a MCP tool.
```

### Exercício 2: Executar o RAP generator

```
Gere um app SAP Fiori transacional para gerenciamento de viagens com DRAFT usando o ABAP RAP generator.

Use a seguinte especificação:
- Pacote: ZSITFOR_AI_###
- Entidade 1: Travel, baseada na estrutura /DMO/TRAVEL_DATA
- Entidade 2: Booking, baseada em /DMO/BOOKING_DATA (filha de Travel)
- Todos os nomes de objetos gerados devem terminar com o sufixo "###"
```

### Exercício 2: Ativar todos os objetos

```
Ative todos os objetos do pacote ZSITFOR_AI_### usando a MCP tool.
```


### Exercício 4: Implementar a validação

```
Adicione a validação validateCustomer. Na sua implementação, deve ser usado o método validate_customer da classe ABAP ZCL_TRAVEL_HELPER_###. Peça para revisar as alterações antes de prosseguir com a ativação
```

### Exercício 5: Gerar unit tests

```
Gere ABAP unit tests para a classe de teste local LTCL_TRAVEL_HELPER_### na classe ZCL_TRAVEL_HELPER_###.
Crie métodos de teste para validate_customer (casos válidos e inválidos) e get_booking_status (Booked/New/Cancelled).
```

### Exercício 5: Executar unit tests via MCP

```
Execute os ABAP unit tests da classe ZCL_TRAVEL_HELPER_### usando a MCP tool.
```

---

## Dicas para obter melhores resultados com MCP

| Dica | Por que ajuda |
|------|---------------|
| Seja explícito sobre os nomes dos objetos | Evita ambiguidade quando vários objetos correspondem |
| Especifique o sufixo `###` | Garante que os artefatos gerados não colidam com os de outros participantes |
| Diga "use a MCP tool" | Sinaliza ao seu coding agent que deve preferir chamadas de tool a sugestões de código |
| Permita todas as chamadas de tool em sequência | Algumas operações (por exemplo, generate) exigem várias chamadas de tool |
| Atualize o Explorer do Visual Studio Code após a geração | Pressione **F5** para ver os objetos recém-criados |
