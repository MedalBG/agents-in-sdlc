---
title: "Lição 6 - Validar a funcionalidade com Playwright MCP"
description: "Configure ou confira o Playwright MCP no Copilot CLI e use-o para explorar a filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Além de escrever código, é preciso trabalhar com dados, serviços externos e ferramentas adicionais. Os servidores MCP ampliam as ferramentas e os serviços disponíveis no Copilot CLI.

Nesta lição, você conhecerá o Model Context Protocol (MCP), adicionará o servidor do Playwright se necessário e pedirá ao agente que explore a filtragem em um navegador.

## Cenário

Testes de unidade e de ponta a ponta são importantes, mas validar uma alteração de interface também exige interagir com ela. Com o Playwright MCP, o Copilot pode usar o site como uma pessoa usuária e verificar o comportamento.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos, obtenham informações atualizadas e executem ações em seu nome. Um servidor MCP faz a ponte entre o agente e as ferramentas e recursos que ele pode acessar.

Exemplos incluem o **[GitHub MCP Server][github-mcp]**, que oferece APIs para gerenciar repositórios, issues e pull requests, e o **[Playwright MCP Server][playwright-mcp-server]**, que automatiza um navegador. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta de servidores.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência. Revise o código, confirme o publicador e avalie as implicações de segurança antes de usá-los.

## Adicionar o servidor MCP do Playwright

Se o Playwright MCP ainda não estiver configurado no Copilot CLI:

1. Volte ao Codespace e abra o Copilot CLI.
2. Digite:

   ```plaintext
   /mcp add
   ```

3. Para o nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo **STDIO** com <kbd>Enter</kbd> e pressione <kbd>Tab</kbd>.
5. No campo **Command**, insira:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor. Pressione <kbd>Esc</kbd> para fechar a caixa de diálogo.

> [!NOTE]
> Se o Playwright MCP já estiver configurado, ignore as etapas de configuração.

## Pedir ao Copilot para explorar a funcionalidade

Peça ao Copilot que valide a funcionalidade com este prompt:

```plaintext
Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
```

O Copilot normalmente identifica sozinho que deve usar o servidor MCP, mas mencioná-lo no prompt pode tornar o resultado mais consistente.

1. Acompanhe as etapas executadas no navegador.
2. Leia o relatório e confirme que o comportamento está correto. O agente iniciará e encerrará o servidor e apresentará os resultados.

## Resumo e próximos passos

Você conheceu o MCP, configurou o Playwright MCP quando necessário e usou o agente para explorar a filtragem em um navegador real.

Em seguida, [crie um agente personalizado de QA][next-lesson] que combine a skill de verificações com as ferramentas do navegador.

## Recursos

- [O que é MCP e por que todos estão falando dele?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
