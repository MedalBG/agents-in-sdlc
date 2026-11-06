---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como outras ferramentas de CLI, o GitHub Copilot CLI inclui comandos de barra para acessar recursos avançados, informações e configurações. Você já usou `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Agora, explore outros comandos.

Nesta lição, você usará `/context` e `/compact`, explorará modelos com `/model`, verá como `/share` exporta sessões e conhecerá comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

O fluxo principal do CLI está concluído. Agora, explore o gerenciamento de contexto, a seleção de modelos, o compartilhamento de sessões e, opcionalmente, a delegação ao [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Em tarefas grandes, você pode se aproximar do limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente, mas você também pode consultar e compactar o contexto manualmente.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se necessário.
2. Digite `/context` e observe o modelo, o uso de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
3. Digite `/compact` e depois `/context` novamente para comparar. Talvez a diferença seja pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` para escolher o momento; use `/clear` ou `/new` para iniciar uma conversa limpa ao mudar para uma tarefa sem relação.

## Escolher um modelo

1. Digite `/model`.
2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

O comando `/share` pode exportar uma sessão para Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e consulte as opções de `/share` na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino. Por exemplo, `/share file` cria um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo ou publicá-lo. A exportação pode incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não queira compartilhar.

## Opcional: ampliar o trabalho ou delegar

O fluxo principal do workshop terminou. O Copilot CLI também oferece:

- `/fleet` para dividir subtarefas independentes entre subagentes e executá-las em paralelo;
- `/worktree` para criar uma árvore de trabalho Git isolada;
- `/delegate` para enviar uma tarefa ao agente de nuvem, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees ou trabalho remoto. Inicie tarefas novas e bem definidas e revise os resultados pelo fluxo normal. Para explorar o trabalho assíncrono, consulte o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Você usou `/context` e `/compact` para examinar o contexto, `/model` para explorar modelos e `/share` para exportar ou compartilhar sessões, além de conhecer comandos opcionais de paralelismo, worktrees e delegação.

Há muitos outros comandos de barra. Agora, [revise o que aprendeu][next-lesson] e veja os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
