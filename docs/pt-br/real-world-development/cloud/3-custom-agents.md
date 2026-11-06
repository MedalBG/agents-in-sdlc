---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Você os define em arquivos Markdown na pasta `.github/agents` do repositório, fornecendo instruções, práticas recomendadas, padrões de código e conhecimento de domínio. Assim, as equipes podem transformar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que verifica a conformidade com as [WCAG][wcag], um agente de segurança ou um agente de testes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido por um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. A distinção mais simples é: **o agente personalizado é quem executa o trabalho; as skills são ferramentas**.

Agentes personalizados têm sua própria janela de contexto e coordenam skills e até outros agentes. Neste exercício, o agente de acessibilidade revisa e atualiza o site conforme as diretrizes de acessibilidade e pode usar skills de fluxo de pull request ou de execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única forma correta de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai aprender:

- como agentes personalizados são definidos;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Pessoas usuárias relataram que o tema escuro atual é difícil de ler devido ao baixo contraste entre texto e fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado. Como a acessibilidade é importante, você usará um agente personalizado para implementar a funcionalidade rapidamente.

## Revisar o agente personalizado de acessibilidade

O projeto já inclui um agente personalizado de acessibilidade. Volte ao codespace e examine `.github/agents/accessibility.md`:

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que descrevem responsabilidades para gerar código acessível, práticas recomendadas, exemplos de HTML, CSS e JavaScript, além de problemas comuns a evitar.

## Criar e atribuir uma issue

O painel de agentes é o local central para atribuir tarefas ao agente de nuvem, acompanhar o trabalho e fornecer orientações adicionais. Crie uma issue para o modo de alto contraste:

1. Acesse seu repositório e selecione a guia **Issues**.
2. Selecione **New issue** e, em seguida, **Blank issue**.
3. Defina **Title** como `Add high contrast mode to website`.
4. Em **Description**, insira:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. Selecione **Create**. No painel à direita, selecione **Assign to Copilot**.
6. Na lista de agentes personalizados, selecione **Accessibility agent** e depois **Assign**.

   ![Atribuição do agente de nuvem com o agente personalizado de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Você explorou [agentes personalizados][custom-agents], assistentes especializados que permitem codificar a experiência e os padrões da equipe em agentes reutilizáveis. Você viu como eles são definidos e atribuiu uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, aprenda a [acompanhar e orientar a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
