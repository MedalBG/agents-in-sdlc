---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma forma específica — ou se o Copilot precisa conhecer informações adicionais —, esse contexto deve estar acessível. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não só *o que* você quer, mas também *como* quer que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo de caminho;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não fará aqui um prompt de comparação antes e depois. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o efeito lado a lado em tempo real. Você verá a influência do arquivo de instruções ao revisar os pull requests gerados pelo agente nos próximos exercícios.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação em comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com arquivos de instruções, você fornecerá ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para o Copilot, ajudando-o a entender seu estilo de código e seus requisitos. Você pode especificar convenções de código, bibliotecas e os tipos de comentários que deseja. As instruções podem valer para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações relevantes para a maioria das solicitações, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para orientar linguagens como TypeScript ou Astro, ou tarefas como criar componentes de interface e testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para sugestões de conclusão de código ou de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Criar arquivos de instruções não é o foco deste workshop, mas os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` concentrado em orientações para todo o projeto, como sua estrutura, o que está sendo desenvolvido e os padrões globais de código.
- Use arquivos `*.instructions.md` para orientações específicas de tipos de arquivo, como testes de unidade, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos do que fazer e do que evitar.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para tarefas como [atualizar a interface][ui-instructions] e trabalhar com [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. O percurso correspondente explica o recurso quando ele for relevante.
>
> Para encontrar modelos e exemplos, explore o repositório [awesome-copilot][awesome-copilot], que reúne arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizados deste projeto

Leia os arquivos de instruções que acompanham este repositório. Há um arquivo principal, `copilot-instructions.md`, e vários arquivos `*.instructions.md` para tarefas específicas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Leia a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam às interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo. Ele define um padrão global relativo à raiz do repositório que determina quais arquivos recebem as instruções. Neste caso, arquivos de teste TypeScript, como `**/*.test.ts`, correspondem ao padrão.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções e exemplos existentes. Isso permite dividir conjuntos grandes de orientações em arquivos menores e reutilizáveis. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicionar um novo padrão para o repositório

Agora, você fará uma alteração: adicionar uma regra para que a documentação seja escrita no código em comentários TSDoc e em cabeçalhos de comentários no nível do arquivo. O agente de nuvem aplicará essa orientação às issues que você atribuir a ele mais adiante.

Antes de editar, crie uma branch. O agente de nuvem lerá as instruções da branch de destino da issue, mas você fará o commit da alteração separadamente:

1. No terminal do codespace, crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como explicado, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Atualize os padrões de código para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de código, mas ainda não exige documentação no código — por isso, o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo após os padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem estar no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação adicionada neste exercício.

## Fazer commit, enviar e mesclar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, faça o merge das instruções em `main` para que o agente possa usá-las.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. O agente de nuvem lerá as instruções ao trabalhar nas issues que você atribuir no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, ignore as etapas da branch e faça commit diretamente em `main`. A branch foi incluída para que o exercício se aproxime de um fluxo de trabalho real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e adicionou um novo padrão para todo o repositório a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente de nuvem nos próximos exercícios.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
