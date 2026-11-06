---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` permitem fornecer instruções, práticas recomendadas, padrões de código e conhecimento de domínio. As equipes podem codificar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem as [WCAG][wcag], de segurança ou de testes.

Os agentes são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido por um prompt Markdown que especifica comportamento, experiência e instruções.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] usam arquivos Markdown para orientar tarefas. Uma distinção útil: **o agente personalizado é quem executa o trabalho; as skills são ferramentas**. O agente tem sua própria janela de contexto e pode coordenar skills ou outros agentes. Neste exercício, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes e poderá usar skills para fluxos de pull request ou testes.

> [!NOTE]
> Não há uma única forma correta de criar um agente. Teste e itere para encontrar o que funciona no seu ambiente.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma acessível a todas as pessoas. Usuários relataram que o tema escuro é difícil de ler devido ao baixo contraste entre texto e fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado. Você usará um agente personalizado para implementar a funcionalidade.

Neste exercício, você revisará um agente de acessibilidade existente e o usará no Copilot Chat para implementar o modo.

## Revisar o agente personalizado de acessibilidade

Volte ao codespace e crie uma branch a partir de `main` para manter este trabalho separado do PR de filtragem:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md` e observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

2. Leia as responsabilidades, práticas recomendadas, exemplos de HTML, CSS e JavaScript e os problemas comuns listados no arquivo.

## Usar o agente personalizado no Copilot Chat

O menu de agentes na parte inferior da vista Copilot Chat do VS Code exibe os agentes definidos em `.github/agents`. Selecione um agente para aplicar suas instruções e ferramentas à conversa.

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo necessários variam conforme o exercício.

1. No menu de agentes da vista Chat, selecione **Agent**, se ainda não estiver selecionado.

   ![Seletor de agente na vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra o menu de agentes na parte inferior da vista Chat e selecione **Accessibility agent**.
3. Envie o prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. O Copilot explorará o código, proporá alterações e editará os arquivos. Revise as diferenças exibidas no chat e o indicador **Files changed**.

> [!NOTE]
> O processo pode levar alguns minutos enquanto o Copilot altera componentes Astro, CSS e testes relacionados. Você acompanhará e orientará o trabalho na próxima lição.

## Resumo e próximos passos

Você revisou agentes personalizados, que codificam o conhecimento da equipe em assistentes reutilizáveis, e usou o agente de acessibilidade no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações e adicione um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
