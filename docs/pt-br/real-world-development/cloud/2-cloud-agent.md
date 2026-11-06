---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Muitas organizações enfrentam dívidas técnicas, como problemas de segurança não resolvidos, código legado que precisa de atualização ou funcionalidades adiadas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para revisão por uma pessoa desenvolvedora. Assim, você pode delegar tarefas repetitivas e se concentrar em atividades de maior impacto.

Neste exercício, você vai explorar:

- a personalização do ambiente usado para gerar código;
- a execução segura das operações;
- a importância de issues com escopo claro;
- a atribuição de issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver dívidas técnicas. A documentação deixada pelos prestadores que criaram a primeira versão do site está incompleta. Como primeiro passo, a equipe quer adicionar comentários TSDoc às funções exportadas do aplicativo.

A equipe de design também quer melhorar a descoberta de jogos. Ela quer mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo para testar a experiência do usuário. Essa funcionalidade ainda não precisa estar pronta para produção e está atrás de outras prioridades.

Esses são exemplos de tarefas que podem ser adiadas e que são adequadas para o agente de nuvem. Ele trabalha de forma assíncrona, enquanto você cuida de outras atividades, e depois você revisa os resultados.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Uma opção é [atribuir uma issue do GitHub ao Copilot][assign-issue]. O Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não dispensa o planejamento. Mesmo tarefas aparentemente simples podem ter detalhes complexos. Por isso, [defina com cuidado o escopo do trabalho do agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas. Se você já concluiu o exercício sobre elas, viu `.github/copilot-instructions.md` em ação; caso contrário, leia esse arquivo agora. Antes de atribuir trabalho ao agente de nuvem, examine os arquivos de instruções existentes para identificar a influência deles mais tarde.

Abra os arquivos a seguir na interface web do GitHub ou em um codespace:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente no próximo trecho, confira se o pull request inclui comentários TSDoc, segue as convenções de TypeScript e contém cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções e serão retomadas em uma lição posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e scripts de configuração. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. O projeto já inclui essa configuração para facilitar o exercício: ela instala o Node.js e as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e configura o banco de dados SQLite migrado e preenchido usado pelo aplicativo Astro.

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no menor nível necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, você precisa da permissão `contents: read`. Caso contrário, o Copilot fará isso automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros workflows do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para verificar a configuração sem esperar o agente iniciar.

## Atribuir a tarefa de documentação

Documentação desatualizada ou ausente é uma dívida técnica que reduz a produtividade e dificulta a manutenção do código. O Copilot pode gerar a documentação em segundo plano, e você revisará o resultado em um exercício futuro.

1. Em uma nova guia, acesse o repositório no github.com.
2. Selecione a guia **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `Code lacks documentation`.
5. Em **Description**, insira:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. Selecione **Create** para criar a issue.
7. À direita, selecione **Assign to Copilot** e, na janela de atribuição, selecione **Assign**.

   ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

8. Abra a guia **Pull Requests** e selecione o novo pull request, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
9. Aguarde a lista de tarefas criada pelo Copilot. Isso pode levar alguns minutos enquanto o agente configura o ambiente, analisa o projeto e planeja o trabalho.
10. Revise as tarefas e o andamento no pull request. Selecione **View session** para acompanhar a sessão em execução.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador de andamento.

O agente executará essa tarefa em segundo plano. Enquanto isso, atribua outra tarefa para que o Copilot trabalhe nela.

## Atribuir a funcionalidade de jogos relacionados

1. Volte ao repositório no github.com e abra **Issues** > **New issue** > **Blank issue**.
2. Defina **Title** como `Show related games on the game details page`.
3. Em **Description**, insira:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

4. Selecione **Create** e, no painel à direita, **Assign to Copilot** > **Assign**.

   ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

Pouco depois, um emoji de olhos na primeira resposta da issue indicará que o Copilot começou a trabalhar.

![Emoji de olhos indicando que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot trabalhará nessa segunda solicitação. Como o agente de nuvem atua de forma semelhante a uma pessoa engenheira de software, você não precisa acompanhá-lo continuamente. Revise o resultado quando estiver pronto; por enquanto, avance para aprender sobre agentes personalizados.

## Resumo e próximos passos

Você explorou o [agente de nuvem do GitHub Copilot][copilot-agents], que pode trabalhar de forma assíncrona em issues para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código entre frameworks. Você personalizou o ambiente, considerou a segurança, definiu issues com escopo claro e atribuiu tarefas ao Copilot.

Em seguida, crie e use agentes personalizados. O agente de nuvem também pode usar [servidores MCP][cloud-agent-mcp] e instruções personalizadas, que foram abordadas em exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
