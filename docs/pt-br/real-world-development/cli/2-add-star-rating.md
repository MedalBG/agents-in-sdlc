---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para alterar os cartões de jogos, revisar a mudança no navegador encaminhado e mesclá-la no primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, faça sua primeira alteração. Os jogos já têm avaliações por estrelas nos dados, mas elas ainda não aparecem nos cartões da página inicial. Peça ao agente para exibi-las, revise a alteração e mescle-a no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada em uma branch de funcionalidade;
- pedir ao agente uma pequena alteração;
- revisar as mudanças com `/diff`;
- executar o aplicativo e conferir a alteração no navegador encaminhado;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, já exibida na página de detalhes. Os cartões da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como primeiro exercício, peça ao agente para exibir a avaliação existente em cada cartão.

## Como funciona uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal no CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma árvore de trabalho dedicada. Conversas salvas permitem retomar a discussão; os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá seus prompts e as respostas do agente, as ferramentas usadas para explorar e editar arquivos e as alterações que pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a alteração

1. Volte ao Codespace. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Inicie o Copilot, se necessário:

   ```bash
   copilot --yolo
   ```

3. Use `/new` e pressione <kbd>Enter</kbd> para iniciar uma sessão.
4. Envie o prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto e criará o código necessário.

## Revisar o diff

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que cada cartão mostra a avaliação numérica e que `No rating yet` aparece quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois casos.
4. Revise os resultados das verificações e peça ao Copilot para corrigir falhas.
5. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico. O código pode variar; revise o comportamento, não uma implementação específica.

## Conferir as alterações

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões exibem as avaliações de zero a cinco.
4. Volte ao Copilot e peça para parar o servidor:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir e mesclar seu primeiro pull request

1. Peça ao agente padrão para fazer commit:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente e exibirá a URL do PR.
3. Abra o PR selecionando a URL com <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado.
4. Revise os arquivos e as verificações. Quando estiver tudo pronto, selecione **Merge pull request** e confirme.
5. Volte ao Codespace, saia do Copilot CLI com `/exit` e atualize `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Você iniciou uma conversa focada, pediu uma alteração pequena, revisou o diff, conferiu o aplicativo no navegador e abriu e mesclou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use Plan e Autopilot][next-lesson].

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
