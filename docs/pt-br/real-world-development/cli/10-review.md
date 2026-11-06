---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo na Tailspin Toys. Você:

- preparou um Codespace, instalou o CLI, explorou o projeto e encontrou a issue de filtragem;
- adicionou avaliações por estrelas, revisou o resultado no navegador encaminhado e mesclou manualmente seu primeiro pull request (PR);
- começou pela issue de filtragem, planejou no modo Plan, implementou no Autopilot e revisou no Interactive;
- orientou o agente com instruções personalizadas e personalizou a skill `quality-checks` para executar testes de unidade, lint e verificações de tipos;
- adicionou o servidor Model Context Protocol (MCP) do Playwright e explorou a filtragem em um navegador;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, verificações e evidências do navegador;
- revisou a alteração completa e autorizou o Agent Merge no PR de filtragem;
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop inclui dois PRs, cada um em sua própria branch criada a partir de `main` atualizado:

1. **Avaliações por estrelas:** exibir o valor existente de `starRating` e um estado explícito para jogos sem avaliação nos cartões.
2. **Filtragem e qualidade:** implementar filtros, atualizar e aplicar instruções, personalizar o relatório da skill `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Do planejamento da filtragem à abertura do PR, você usou a mesma conversa e branch. O workshop reúne o trabalho em um único PR para simplificar o percurso.

## Diferentes tipos de verificação

Você verificou o código com testes automatizados, inspeção manual no navegador e exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos no formato de relatório configurado. O agente QA reuniu os resultados e revisou requisitos e cobertura.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não precise criar testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas oferecidos ao Copilot influenciam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especialistas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para definir uma abordagem, **Interactive** para orientar alterações específicas e **Autopilot** para tarefas com escopo claro. Contexto também é essencial: explicar *o que* você quer, *por que* e *como* influencia significativamente o resultado.

## Mais recursos para explorar

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para discutir um problema e obter outra perspectiva;
- `/fleet` para coordenar subtarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa;
- `/delegate` para enviar trabalho ao agente de nuvem.

## Próximos passos

Continue usando o Copilot em código de produção, projetos pessoais ou no aplicativo que você sempre quis criar. Compartilhe o que aprendeu com a equipe, aprenda com as experiências dela e consulte a documentação.

Para ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore os workshops de [VS Code][vscode], [aplicativo GitHub Copilot][app] e [agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
