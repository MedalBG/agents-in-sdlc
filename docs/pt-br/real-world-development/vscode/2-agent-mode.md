---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até uma pequena alteração normalmente exige mudanças em vários arquivos e a execução de testes. O modo de agente do Copilot pode explorar a estrutura do projeto, atualizar os arquivos necessários, executar tarefas como testes e corrigir problemas encontrados. Veja como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora tenham nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando issues e pull requests para colaborar.

Neste exercício, você vai aprender a usar o modo de agente para explorar o projeto, identificar arquivos relevantes, coordenar alterações na interface e na camada de dados e revisar alterações e testes antes do merge.

## Cenário

À medida que a lista de jogos cresce, as pessoas precisam filtrar por categoria e editora. Você já criou um auxiliar de editoras no exercício anterior; agora, concluirá o trabalho restante na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de alterar o código, explore o site. A Tailspin Toys é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de programação. O site é um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente o banco de dados SQLite local com o Drizzle ORM. Não há uma API de backend separada nem um framework de interface no cliente. Os auxiliares de acesso a dados ficam em `src/lib/`; a interatividade usa scripts Astro pequenos e APIs padrão do DOM.

### Iniciar o site

1. Volte ao codespace. Você deve estar na branch `main`.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um aviso semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir `http://localhost:4321` em uma janela do navegador, feche-a selecionando **x**.

5. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor no terminal.

> [!NOTE]
> No codespace, o link de localhost redireciona para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor em execução no codespace.

### Explorar o site

A página inicial mostra jogos de tabuleiro com títulos, imagens e descrições. Ao selecionar um jogo, a página de detalhes exibe mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa as opções relevantes.

O modelo do repositório criou um backlog de issues. Consulte-o com o Copilot:

1. No menu de agentes da vista Chat, selecione **Agent**.

   ![Seletor de agente na vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> A inclusão de Claude Sonnet 4.5 não expressa preferência por um modelo. Ele foi usado na criação do workshop para que as sugestões fossem relativamente consistentes. Como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes; isso é normal.

> [!NOTE]
> O Copilot pode usar um comando MCP diferente, mas ainda deve conseguir concluir a tarefa.

3. Envie este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o Copilot liste as issues.
5. Revise a lista e a priorização sugerida.

## Revisar os arquivos de instruções

Antes de pedir ao agente para gerar código, examine `.github/instructions/ui.instructions.md`, que explica como adicionar funcionalidades ao site.

1. Abra `.github/instructions/ui.instructions.md` no codespace.
2. Observe a arquitetura, os princípios de design de componentes, testabilidade e acessibilidade, além dos links para instruções de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os caminhos nos arquivos de instruções são relativos ao próprio arquivo. Assim, você pode reutilizar orientações, dividir conjuntos complexos e incluir exemplos.

## Implementar a filtragem

A funcionalidade exige alterações na lógica de filtragem em `src/lib/`, testes do comportamento e uma interface de filtragem na página de listagem de jogos. O modo de agente pode executar essas tarefas.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e o arquivo `ui`.

   ![Selecionar o arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** e **Claude Sonnet 4.5** continuam selecionados.
4. Envie este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Acompanhe a exploração do projeto e a localização dos auxiliares de dados, da interface e dos testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

6. Revise as alterações sugeridas nos arquivos. O Copilot pode mostrar os botões **Keep** e **Undo**; aguarde até que ele termine antes de aceitar ou desfazer as alterações.
7. Quando solicitado, selecione **Continue** para executar os testes.

   ![Solicitação do Copilot Chat para confirmar a execução dos testes](../../../_images/ex3-agent-mode-run-tests.png)

8. O Copilot pode pausar ou encontrar falhas enquanto itera entre a geração de código e os testes.

   ![Sessão concluída do Copilot Agent Mode com alterações propostas](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revise o código e os testes gerados.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador, teste a funcionalidade e, quando estiver satisfeito, selecione **Keep** no Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente para explorar o projeto, identificar arquivos e implementar funcionalidades na interface e na camada de dados. Também revisou as alterações e os testes antes do merge.

Agora, [teste a funcionalidade com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra: implementar paginação

À medida que a lista cresce, talvez seja necessário adicionar paginação. Peça ao Copilot para implementá-la. Siga os padrões existentes e os arquivos de instruções, decida se o tamanho da página será configurável ou fixo e forneça contexto suficiente. Iterar e pedir alterações faz parte do fluxo normal com o Copilot.

## Recursos

- [Modo Ask, Edit e Agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
