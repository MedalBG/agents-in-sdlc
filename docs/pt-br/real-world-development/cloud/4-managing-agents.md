---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma autônoma e assíncrona, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver as tarefas com pull requests abertos, examinar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas;
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe percebeu que também seria útil adicionar um modo claro. Como o trabalho já inclui mudanças de estilo e controles de alternância, peça ao agente para implementar também o modo claro.

## Revisar as tarefas do agente de nuvem

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine as tarefas listadas nos painéis principal e esquerdo. Você deverá encontrar as tarefas que atribuiu, incluindo documentação, APIs para alterar jogos e o modo de alto contraste.
3. Selecione uma tarefa em andamento e examine as operações realizadas pelo Copilot, como obter o código do repositório, preparar o ambiente, configurar servidores MCP e executar etapas para concluir o trabalho.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Examine também o painel de pull request à direita para acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

1. Selecione a sessão que está implementando o modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde até que o agente conclua a configuração e comece a executar a tarefa.
3. Na caixa de diálogo **Steer active session while Copilot is working**, insira:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Caixa de diálogo para orientar uma sessão ativa do agente de nuvem](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará a nova solicitação depois de concluir a etapa que estava executando quando você enviou a mensagem. Isso pode levar alguns minutos; aproveite a pausa para refletir sobre o que aprendeu.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o painel central para acompanhar e orientar tarefas do agente de nuvem. Você acompanhou tarefas ativas e concluídas, revisou o trabalho e redirecionou uma sessão em andamento para incluir uma funcionalidade.

Em seguida, [itere sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
