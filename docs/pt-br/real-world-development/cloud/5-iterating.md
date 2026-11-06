---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Neste laboratório, você usou o GitHub Copilot para melhorar a experiência do usuário e adicionar funcionalidades: pediu documentação para o código, uma funcionalidade de jogos relacionados para a equipe de design e recursos de acessibilidade com modos de alto contraste e claro. Agora, revise as alterações e, se necessário, envie comentários ao Copilot para aprimorá-las.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. Revise o código gerado e siga o processo normal de DevOps. Neste exercício, você revisará as sugestões do Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão direta, há restrições para manter a segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para as alterações.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e temporário.
- Workflows do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem, que adiciona documentação ao código.

> [!NOTE]
> Talvez você veja um aviso de que o Copilot foi bloqueado por um firewall. Isso é **esperado**: o acesso a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, [personalize ou desabilite o firewall do agente de nuvem][agent-firewall].

1. Acesse o repositório no github.com e selecione **Pull Requests**.
2. Abra o PR com um título semelhante a **Add missing documentation**. Se ele ainda estiver em andamento e marcado como **[WIP]**, aguarde alguns minutos até que o Copilot termine.
3. Selecione **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

4. Examine os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. O conteúdo exato varia; as instruções personalizadas do exercício anterior orientam essas alterações.
5. Volte à guia **Conversation**. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

6. Confirme na seção de verificações do PR que os workflows foram iniciados e que as verificações do aplicativo Astro passaram.

## Solicitar alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou em comentários de linha no código. O Copilot inicia outra sessão para atender à solicitação. Como os resultados são probabilísticos, não há um texto único a usar. Você pode pedir, por exemplo, cabeçalhos de comentário nos arquivos, comentários TSDoc em arquivos TypeScript e Astro ou um README que descreva a estrutura do aplicativo Astro.

1. No PR, adicione um comentário com uma solicitação e mencione **@copilot**.
2. Selecione **View Session** para acompanhar a nova sessão e as alterações.
3. Use **Back to pull request** para voltar ao PR.
4. Quando o Copilot terminar, revise o novo commit na guia **Files changed**.

Continue iterando até ficar satisfeito. Depois, marque o PR como pronto para revisão e mescle-o na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

1. Volte ao repositório e abra **Pull Requests**.
2. Selecione o PR com um título semelhante a **Show related games on the game details page** e abra **Files changed** para revisar o código.
3. Se as verificações estiverem aguardando aprovação, volte a **Conversation** e selecione **Approve and run workflows**.
4. Confirme que as verificações do aplicativo Astro foram aprovadas.
5. **Opcional:** teste a funcionalidade no codespace. No terminal, substitua `<branch-name>` pelo nome da branch criada pelo Copilot (por exemplo, `copilot/fix-8`) e execute:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Você pode continuar pedindo alterações por meio de comentários na guia **Conversation**, como ajustar o número de jogos relacionados ou adicionar cabeçalhos e comentários TSDoc. Observe que a issue foi atribuída antes de você atualizar as instruções personalizadas.

## Revisar os recursos de acessibilidade

Por fim, revise o PR do agente personalizado de acessibilidade. Ele deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no Exercício 4.

1. Em **Pull Requests**, abra o PR com um título semelhante a **Add high contrast mode to website**. Se ainda estiver marcado como **[WIP]**, aguarde o Copilot terminar.
2. Na guia **Files changed**, examine a implementação, principalmente:
   - os controles de interface para alternar entre modos;
   - o armazenamento local usado para persistir as preferências;
   - as alterações de CSS para os modos de alto contraste e claro;
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado;
   - o código JavaScript ou TypeScript responsável pela alternância.
3. Volte a **Conversation** e, se necessário, selecione **Approve and run workflows**. Confirme que as verificações foram aprovadas.
4. **Opcional:** teste o PR no codespace, mudando para a branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os controles dos modos claro e de alto contraste. Confirme que as preferências continuam ativas depois de recarregar a página.

O agente personalizado de acessibilidade orientou a implementação conforme as práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações.

## Exercício opcional: continue delegando

O agente de nuvem é mais útil quando recebe itens reais do backlog enquanto você se dedica a outras tarefas. Crie mais algumas issues e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na página de listagem de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Você examinou os arquivos de instruções do repositório, atribuiu issues ao agente e acompanhou a configuração e a execução assíncrona do trabalho. Também criou e usou um agente personalizado para implementar modos de alto contraste e claro, monitorou e orientou a sessão na página de agentes e revisou e aprimorou os pull requests mencionando `@copilot` e aprovando workflows.

## Revisão e próximos passos

Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com Plan e Autopilot, skills, um agente de QA, Playwright MCP, Agent Merge e a delegação opcional `/delegate` para o agente de nuvem.

No seu repositório, experimente atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes, criar um agente personalizado para outro domínio ou configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
