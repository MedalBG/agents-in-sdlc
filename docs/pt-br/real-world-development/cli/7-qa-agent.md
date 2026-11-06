---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine requisitos da issue, a skill quality-checks e o Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o Playwright MCP para observar a filtragem no navegador. Agora, combine esses recursos em um agente personalizado com um processo de QA definido.

Nesta lição, você vai explorar como o agente combina instruções, skills e ferramentas MCP, criar e revisar um perfil reutilizável de garantia de qualidade (QA) e avaliar a funcionalidade conforme a issue de filtragem.

## Cenário

A Tailspin Toys quer revisar requisitos, qualidade do código, verificações automatizadas, cobertura de testes e comportamento no navegador antes de abrir um pull request. Um agente personalizado pode coordenar esse processo e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida por um perfil Markdown que descreve sua finalidade, instruções e ferramentas disponíveis. Neste workshop, o perfil de QA fica em `.github/agents/qa.agent.md`.

Cada personalização tem uma função: as instruções do repositório descrevem os padrões da equipe; a skill `quality-checks` agrupa verificações repetíveis; o Playwright MCP fornece ferramentas de navegador; e o perfil de QA orienta o Copilot a avaliar requisitos e relatar resultados.

## Criar o perfil de QA

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o perfil:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecionar o perfil

Antes de usar o agente, confirme que o Copilot capturou o processo e os limites desejados:

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome claro no seletor.
3. Confirme que o perfil começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme que o agente apresenta evidências, pede autorização antes de alterar código de implementação e não faz commits nem abre pull requests.
5. Se faltar alguma responsabilidade ou limite, peça ao agente padrão para revisar o perfil.

## Executar o QA para a issue

O Copilot CLI carrega os agentes do projeto ao iniciar uma conversa. Retome a conversa de filtragem e selecione o agente QA para aproveitar a issue e as decisões do plano já presentes no contexto.

1. Ative o agente:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como o agente acabou de ser criado, talvez ele não apareça na lista. O comando acima deve ativá-lo.

2. Peça uma revisão:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Leia o relatório do agente QA.

## Resumo e próximos passos

Você explorou como um agente personalizado usa instruções, skills e ferramentas MCP, criou e inspecionou um perfil reutilizável e revisou os resultados conforme a issue de filtragem.

Agora, você tem a implementação, a atualização da skill, o perfil de QA, os testes e as evidências de verificação prontos para revisão. Em seguida, [reúna-os no PR da funcionalidade e use o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
