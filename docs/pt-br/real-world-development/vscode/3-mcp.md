---
title: "Exercício 3 - Testar a funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você criou a filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Conecte o **servidor MCP do Playwright** e peça ao Copilot para controlar um navegador real e testar a funcionalidade. Depois, publique a branch e abra o PR.

Neste exercício, você vai conhecer o Model Context Protocol (MCP), adicionar o servidor do Playwright ao espaço de trabalho, usá-lo para testar a filtragem e abrir um pull request.

## O que é o Model Context Protocol (MCP)?

O MCP permite que um agente se comunique com ferramentas e serviços externos. Um servidor MCP atua como ponte entre o agente e essas ferramentas, disponibilizando recursos e ações em tempo real.

![Diagrama da interação do modo de agente com contexto, LLM e ferramentas, incluindo ferramentas de servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

Exemplos de servidores MCP:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar um navegador para acessar páginas, preencher formulários e selecionar botões.

O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência. Revise o código, confirme o publicador e avalie as implicações de segurança antes de conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores disponíveis no espaço de trabalho.

1. Abra `.vscode/mcp.json` no codespace. O servidor `github` já está configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa configuração vem do modelo do projeto. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], que o VS Code acessa por HTTP; entre no GitHub na primeira vez que o Copilot usar uma ferramenta.

## Adicionar o servidor MCP do Playwright

1. No mesmo arquivo, adicione uma entrada `playwright`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

O servidor `playwright` é local: o VS Code o inicia com `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária no codespace.

> [!NOTE]
> O projeto já usa Playwright nos testes de ponta a ponta. Se o navegador estiver ausente, peça ao Copilot para executar `npx playwright install chromium`.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP sob demanda e pode pedir que você confirme que confia neles.

1. Selecione **Start**, que aparece acima da entrada `playwright` em `.vscode/mcp.json`, para iniciar o servidor e validar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração antes de autorizá-lo.

É opcional iniciá-lo manualmente. Se você não fizer isso, o VS Code iniciará o servidor e pedirá sua aprovação quando o Copilot precisar dele.

## Testar a filtragem

O servidor MCP do Playwright permite que o Copilot abra páginas, navegue, aplique filtros e leia os resultados. Ele usa a [árvore de acessibilidade][playwright-mcp-server], com elementos rotulados, em vez de depender de capturas de tela.

Com o servidor conectado e o aplicativo em execução, envie:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Compare o resultado aos critérios de aceitação da issue e peça correções se algo não funcionar como esperado.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes de enviar o prompt. Na primeira utilização, o Playwright pode baixar um navegador.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

1. No painel **Source Control** do codespace, revise as alterações do Copilot.
2. Selecione o ícone **+** para prepará-las e use o botão **Sparkle** para gerar uma mensagem de commit.
3. Selecione **Publish** para enviar a branch ao repositório.

   ![Painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. No Copilot Chat, selecione **New Chat** e confirme que **Agent** está selecionado.
5. Peça ao Copilot que encontre a issue de filtragem por categoria e editora e crie um PR para a branch atual associado à issue:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

6. Selecione **Continue** quando necessário e siga as solicitações de autenticação. Abra o PR pelo link gerado, mas **não faça o merge ainda**.

## Resumo e próximos passos

Você conheceu o MCP, adicionou o servidor do Playwright, testou a filtragem em um navegador, publicou a branch e abriu um pull request. Agora, [crie um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando dele?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
