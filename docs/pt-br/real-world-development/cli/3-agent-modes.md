---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para implementar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você começou com uma pequena funcionalidade. Alterações maiores exigem um processo mais robusto. O GitHub Copilot CLI pode seguir o fluxo de trabalho da organização: começar por uma issue, planejar, implementar, validar e, depois da revisão, mesclar a alteração.

Nesta lição, você vai iniciar uma conversa a partir de uma issue do GitHub, definir os requisitos no modo Plan, implementar a funcionalidade no modo Autopilot, revisar o código e validá-lo manualmente no navegador.

> [!NOTE]
> Você continuará o fluxo de trabalho da funcionalidade na mesma conversa e branch. Normalmente, cada grupo de alterações teria sua própria branch ou PR; neste workshop, usamos um fluxo simplificado para destacar os conceitos.

## Cenário

O catálogo da Tailspin Toys está crescendo e as pessoas precisam filtrar jogos por categoria e editora. A issue descreve a funcionalidade, mas algumas decisões — como combinar filtros — precisam ser definidas antes de começar a programar. Use Plan para decidir a abordagem e Autopilot para executar uma implementação com escopo definido.

## Contexto

O desenvolvimento com agentes de IA não muda os fundamentos do fluxo de trabalho:

1. Comece por uma issue que descreva o trabalho.
2. Planeje o que precisa ser implementado.
3. Desenvolva e revise o código.
4. Execute testes para validar o código.
5. Valide manualmente a funcionalidade.
6. Abra um pull request (PR).
7. Depois da revisão e da integração contínua, faça o merge.

> [!NOTE]
> O fluxo exato varia conforme a equipe e a organização, mas normalmente segue essa estrutura.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar:

- **Interactive**: o agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente escreve código, executa testes e itera até considerar a tarefa concluída.

Comece em Plan, revise o plano e depois use Autopilot para implementá-lo.

## Começar pela issue

1. Volte ao Codespace e reinicie-o se estiver parado.
2. Confirme que o PR de avaliações por estrelas foi mesclado.
3. Abra um terminal e atualize `main`; depois, crie uma branch:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

4. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

5. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues** e <kbd>A</kbd> para mostrar todas as issues.
6. Use as setas para selecionar **Allow users to filter games by category and publisher**.
7. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**. A referência, como `#7`, adiciona a issue ou o PR do GitHub ao contexto.

## Planejar a filtragem

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para selecionar Plan e confirme o indicador abaixo do prompt.
2. Depois da referência da issue, insira:

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. Responda às perguntas do Copilot sobre o comportamento dos filtros.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo, revise-o e peça ajustes se necessário.

## Aprovar o Autopilot

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na sua versão.
2. Confirme que o indicador mostra **Autopilot**.
3. Acompanhe a implementação e a execução dos testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente. Revise o plano antes de aprová-lo. Se houver dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para voltar ao modo Interactive.
2. Digite `/diff` e revise a implementação e os testes.
3. Compare o resultado com a issue e com as decisões do plano.
4. Saia da visualização do diff com <kbd>Esc</kbd> e revise as verificações; peça correções se houver falhas.

## Explorar a funcionalidade

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando a porta `4321` estiver disponível, selecione **Open in Browser**.
3. Teste os filtros de categoria e editora, inclusive as combinações definidas no plano.
4. Confirme o comportamento de limpar filtros e de resultados vazios.
5. Volte ao Codespace e peça ao Copilot que pare o servidor.

## Resumo e próximos passos

Você começou por uma issue, definiu requisitos no modo Plan, implementou com Autopilot, revisou o código e validou a funcionalidade no navegador.

Em seguida, [oriente o Copilot com instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
