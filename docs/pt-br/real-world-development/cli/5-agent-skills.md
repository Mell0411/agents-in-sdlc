---
title: "Lição 5 - Personalize e use uma habilidade quality-checks"
description: "Explore a habilidade quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas escrever código. Já validamos manualmente que o código funciona e usamos arquivos de instruções para garantir que ele siga nossos padrões. Mas e os testes? E o lint? E todas as outras partes da integração contínua (CI)?

Para esses tipos de tarefa, as **habilidades de agente** são a melhor opção. As habilidades ajudam o Copilot a entender como executar operações desse tipo corretamente.

Nesta lição, você vai:

- explorar a habilidade existente `quality-checks`;
- personalizar o formato dos resultados;
- recarregar e executar a habilidade.

## Cenário

A Tailspin Toys usa a habilidade `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As habilidades de agente agrupam instruções reutilizáveis de tarefas, scripts executáveis e recursos de apoio que um agente carrega quando necessário. No núcleo, são uma pasta com o nome da habilidade que contém um arquivo Markdown chamado `SKILL.md`. O Markdown inclui frontmatter com `name` e `description` para definir a habilidade, uma visão geral de seu funcionamento e orientações sobre quando ela deve ser usada. A pasta também pode conter subpastas com scripts e outros recursos que a habilidade utiliza.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A habilidade `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes do projeto.

As habilidades podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta de habilidades do usuário, em `~/.copilot/skills`.

## Explore a habilidade

Vamos explorar a habilidade criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificações de tipos, chamada `quality-checks`.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a saber quando chamar a habilidade.
3. Leia as instruções e observe como elas orientam o Copilot no processo de execução de testes e lint.
4. Observe que a habilidade ainda não tem uma seção **Results output formatting**.

## Execute a habilidade antes de fazer alterações

É possível chamar habilidades diretamente no Copilot CLI ou usando linguagem natural. Vamos pedir ao Copilot que execute as três verificações da habilidade.

1. Volte à conversa de filtragem no modo Interactive.
2. Use o prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório apresentado no final.

## Personalize o relatório

Queremos um relatório melhor, que informe o que foi executado, se houve sucesso e o que as ferramentas relataram. Vamos atualizar a habilidade para produzir esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo é salvo automaticamente.

## Execute a habilidade atualizada

Agora que fizemos a alteração, vamos vê-la em ação. O Copilot CLI pode recarregar habilidades editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma habilidade de agente existente. Nesta lição, você:

- explorou a habilidade `quality-checks` para testes unitários, lint e verificações de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a habilidade.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de habilidades

Estes exemplos da comunidade servem como referências, não são tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
