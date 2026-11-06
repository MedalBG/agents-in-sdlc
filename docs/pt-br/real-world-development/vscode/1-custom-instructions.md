---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] fornecem ao Copilot informações específicas sobre o projeto para orientar a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos com escopo de caminho;
- gerar o primeiro conjunto de dados para filtragem (um auxiliar de editoras) usando as instruções atuais;
- adicionar um padrão para todo o repositório em `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código seguir o novo padrão;
- fazer commit das instruções e do código inicial de filtragem em `main`, para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. Observe a tendência de mudança após atualizar as instruções; não espere que a saída corresponda caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: a camada de dados precisa de testes de unidade; a interface deve ter modo escuro e aparência moderna; o código deve incluir comentários TSDoc; e cada arquivo deve começar com comentários que descrevam sua finalidade. Use arquivos de instruções para orientar o Copilot a seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para orientar o Copilot em relação ao estilo de código e aos requisitos do projeto. Elas podem descrever convenções, bibliotecas e comentários desejados, para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos:

- `.github/copilot-instructions.md` é enviado ao Copilot em **todas** as solicitações do repositório. Inclua informações gerais, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações globais.
- `.github/instructions/*.instructions.md` fornece orientações para arquivos ou tarefas específicas, como TypeScript, Astro, componentes de interface e testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para sugestões de conclusão ou próxima edição.
>
> Copilot Chat, Copilot CLI e agente de nuvem usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a outros padrões de arquivo de instruções][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` concentrado em orientações gerais, como a estrutura do projeto, o que está sendo desenvolvido e os padrões de código.
- Use arquivos `*.instructions.md` para tipos de arquivo, como testes de unidade, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras, com exemplos do que fazer e do que evitar.

Não há uma única forma correta de criar instruções. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa GitHub Copilot deve ter um conjunto sólido de instruções. Explore as orientações para [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode gerar instruções. Cada interface oferece essa opção de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para encontrar modelos, explore o repositório [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções deste projeto

1. Abra `.github/copilot-instructions.md` e observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a orientação aninhada sobre **GitHub Actions Workflows**.
2. Explore `.github/instructions`, que contém instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
3. Abra `.github/instructions/unit-tests.instructions.md` e observe `applyTo`, o padrão global que determina a quais arquivos as instruções se aplicam. Neste caso, arquivos TypeScript de teste, como `**/*.test.ts`, correspondem ao padrão.
4. Leia as orientações para testes de unidade.
5. Abra `.github/instructions/drizzle.instructions.md` e examine os links ao final. Caminhos nesses arquivos são relativos ao próprio arquivo, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões do projeto, mas ainda não exige documentação no código. Você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Criar uma branch para as alterações

1. Volte ao codespace e abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao codespace, abra o painel Copilot Chat e selecione **New Chat**. O modo e o modelo variam conforme o exercício; cada etapa indica as opções necessárias.

Primeiro, envie um prompt com as instruções atuais. Depois, atualize as instruções e envie o mesmo prompt novamente para comparar o resultado.

1. Feche as guias abertas de exercícios anteriores para limitar o contexto.
2. Abra `src/lib/publishers.ts`.
3. No menu de agentes da vista Chat, selecione **Agent**.
4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e poderá alterar o auxiliar e seus testes.
6. Observe que a função tipada recebe um cliente `db` e retorna uma matriz tipada de editoras, seguindo as orientações de `.github/instructions/drizzle.instructions.md`.
7. Observe também que o código proposto ainda não tem comentários TSDoc nem cabeçalho de arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele inclua comentários mesmo sem a instrução. Tudo bem: o objetivo é observar a melhoria de consistência depois de atualizar as instruções.

## Adicionar um padrão para o repositório

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, perto da linha 27, e observe que ainda não há uma regra de documentação no código.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve o arquivo.

> [!TIP]
> O arquivo `.github/copilot-instructions.md` é adequado para padrões globais do projeto. Use arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas.

## Executar novamente o prompt e observar a diferença

1. No Copilot Chat, selecione **New Chat** e volte a `src/lib/publishers.ts`.
2. Envie novamente o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

3. Observe que agora o arquivo pode começar com um comentário semelhante a:

   ```typescript
   /**
    * Auxiliares de acesso a dados de editoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

4. A função também deve incluir um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - O cliente do banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

1. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) e confirme as alterações:

   ```bash
   git status
   ```

2. Prepare e faça commit das instruções e do auxiliar; adicione também os arquivos de teste ou de suporte indicados por `git status`:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações de suporte (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Use os recursos do GitHub conectados ao projeto para localizar a issue sobre as instruções e abrir um pull request associado a ela:

1. Abra o Copilot Chat no codespace, inicie uma conversa e selecione **Agent**.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a vista Copilot Chat.
3. Envie:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando solicitado, revise a ação do GitHub e selecione **Allow** se estiver de acordo. Na primeira utilização, talvez seja necessário entrar na sua conta.
5. Depois que o PR for criado, peça ao Copilot que o mescle em `main` e atualize a cópia local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise as solicitações de permissão antes de selecionar **Allow**.

## Resumo e próximos passos

Você observou como o Copilot usa instruções, adicionou um padrão ao `.github/copilot-instructions.md`, executou novamente o prompt e comparou o código, e fez commit das instruções e da base do auxiliar de editoras em `main`.

Agora, use essas instruções no [modo de agente][next-lesson] para adicionar uma funcionalidade.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
