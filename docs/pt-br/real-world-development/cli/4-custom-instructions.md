---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] orientam não só *o que* o Copilot deve criar, mas também *como* o código deve ser estruturado. Agora que você implementou a filtragem, examine as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai explorar instruções do repositório e de caminhos específicos, atualizar os padrões de código e observar o efeito das instruções no resultado.

## Cenário

A Tailspin Toys tem padrões de desenvolvimento: comentários devem explicar a intenção e decisões não óbvias, não repetir o código; funções exportadas em `db/` e `src/lib/` devem documentar propósito, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo o argumento `db` injetável quando houver; componentes Astro reutilizáveis devem documentar o contrato `Props`; e os comentários devem continuar corretos quando o código mudar. Preserve também as orientações existentes de formatação e lint.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências para orientar o estilo e os requisitos do Copilot:

- `.github/copilot-instructions.md` é enviado ao Copilot em **todas** as solicitações do repositório e deve conter orientações gerais.
- `.github/instructions/*.instructions.md` fornece orientações para linguagens, tipos de arquivo e tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções

O projeto inicial já inclui arquivos de instruções:

1. Volte ao Codespace.
2. No editor do Codespaces, abra `.github/copilot-instructions.md`.
3. Leia a descrição do projeto e as orientações de código, aplicadas a todas as interações com o Copilot neste repositório.
4. Explore `.github/instructions`, que contém orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe `applyTo`, que define um padrão global para os arquivos aos quais as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para instruções e arquivos do projeto. Esses links permitem dividir orientações em arquivos reutilizáveis e fornecer exemplos.

## Atualizar as instruções conforme os padrões da equipe

Adicione às instruções principais uma regra para que o código TypeScript novo inclua comentários TSDoc:

1. Em `.github/copilot-instructions.md`, localize **Code formatting guidance**, perto da linha 35.
2. Adicione como último item:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Aplicar as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Encerre e retome a conversa de filtragem para que ela use as novas orientações sem perder o contexto da funcionalidade.

1. Se a conversa ainda estiver aberta, digite `/exit`. O CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está ativa.
3. Peça ao Copilot que aplique as orientações:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript. Confirme que os comentários TSDoc explicam o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI usa instruções e aplicou um novo padrão à funcionalidade: examinou as instruções globais e de caminhos, atualizou o arquivo do repositório e observou seu efeito no código.

Em seguida, [personalize e execute a skill reutilizável de verificações de qualidade][next-lesson] para padronizar o lint e os testes.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
