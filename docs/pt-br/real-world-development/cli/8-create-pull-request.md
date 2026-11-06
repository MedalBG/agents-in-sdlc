---
title: "Lição 8 - Criar e mesclar o PR da funcionalidade"
description: "Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois, crie o PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise o conjunto e abra um pull request. Você mesclou manualmente o PR de avaliações por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, as alterações da funcionalidade, das instruções, da skill e do agente de QA seriam divididas em vários PRs. Para simplificar o workshop, todo o fluxo de filtragem e qualidade está na mesma conversa e branch e será incluído neste PR.

Nesta lição, você vai conhecer o Agent Merge, inspecionar as alterações e as evidências de verificação, criar o PR de filtragem, habilitar o Agent Merge somente depois da revisão e confirmar o merge.

## Cenário

Você usou o Copilot para planejar, implementar e verificar a filtragem. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle da pessoa desenvolvedora.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request. Depois de habilitá-lo, o Copilot resolve impedimentos, como verificações de integração contínua (CI) com falha, comentários de revisão e necessidade de rebase. Em seguida, habilita o merge automático do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. Antes de habilitar o Agent Merge, revise o trabalho e confirme que está pronto.

## Usar o Agent Merge no PR

1. Volte ao Codespace.
2. Abra o diálogo de agente com `/agent`, selecione **Default** e pressione <kbd>Enter</kbd>.
3. Crie o PR:

   ```plaintext
   /pr create
   ```

4. Ative o Agent Merge:

   ```plaintext
   /pr agentmerge
   ```

5. O Copilot acompanhará a CI e fará o merge quando as verificações forem aprovadas. Confirme uma mensagem semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório ou permissões ausentes. Se houver um bloqueio, leia o motivo e conclua manualmente o merge revisado quando permitido.

## Resumo e próximos passos

Você revisou a alteração e as evidências, criou o PR de filtragem e habilitou o Agent Merge somente depois da revisão.

Em seguida, [explore outros comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
