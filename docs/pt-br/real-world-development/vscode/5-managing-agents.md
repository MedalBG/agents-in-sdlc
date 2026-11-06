---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot explora o código, propõe e aplica alterações e executa comandos. O Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições em tempo real. Você pode revisar cada diferença, aceitar ou desfazer alterações e orientar a conversa com prompts complementares.

Neste exercício, você vai acompanhar o trabalho do agente de acessibilidade, revisar e aceitar as alterações e solicitar um controle de modo claro junto ao de alto contraste.

## Cenário

O agente personalizado de acessibilidade da [Lição 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe também pediu um modo claro para quem considera o tema padrão escuro demais. Oriente a sessão existente, mantendo o contexto atual.

## Acompanhar o progresso do Copilot

1. Volte à vista Copilot Chat da Lição 4.
2. Acompanhe as chamadas de ferramentas, leituras e edições de arquivos e os comandos de terminal.
3. Para cada diferença exibida no chat, selecione **Keep** para aceitar ou **Undo** para desfazer. Use **Files changed** para revisar o conjunto de arquivos alterados.

> [!NOTE]
> Se uma chamada pedir permissão, por exemplo para executar um comando de terminal, o agente exibirá **Allow**. Aprove apenas comandos que você autorizar.

4. Aguarde até que o agente termine o modo de alto contraste.

## Orientar o agente para adicionar o modo claro

Como você continuará na mesma conversa, o agente mantém o contexto dos componentes, dos estilos e do padrão de armazenamento local que usou.

1. No campo de entrada do Copilot Chat, envie:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Revise as novas alterações e selecione **Keep** ou **Undo** para cada diferença.
3. Se o agente seguir uma direção indesejada, selecione **Stop** ao lado do campo de entrada e envie uma instrução de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Peça, por exemplo, para simplificar o CSS usando variáveis, integrar o modo claro ao painel de configurações existente ou adicionar atalhos de teclado aos controles.

## Revisar o conjunto de alterações

1. Abra **Source Control** no VS Code e revise todos os arquivos alterados.
2. Examine as diferenças nos componentes Astro, nos estilos e nos testes relacionados.
3. Confirme os atributos ARIA, a navegação por teclado, o HTML semântico e a persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na próxima lição.

## Resumo e próximos passos

Você acompanhou uma sessão em andamento no modo de agente, revisou cada alteração, aceitou ou desfez edições e enviou prompts complementares para ampliar o trabalho sem perder contexto.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade como pull request e revise as alterações produzidas pelo agente local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
