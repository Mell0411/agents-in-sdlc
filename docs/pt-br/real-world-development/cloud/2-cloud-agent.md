---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se alguma — não enfrentem problemas com dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente na nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele cria um rascunho de PR para que uma pessoa revise. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em atividades de maior impacto.

Neste exercício, você explorará com o agente na nuvem do Copilot:

- a personalização do ambiente de geração de código;
- como garantir que as operações sejam realizadas com segurança;
- a importância de definir issues com escopo claro;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os prestadores de serviço que criaram a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Por enquanto, a equipe não precisa de uma implementação refinada; basta algo que possa ser usado para testes de aceitação da experiência. Essa tarefa é um bloqueio, mas outras issues têm prioridade no momento.

Esses são exemplos de tarefas que podem acabar sendo adiadas e que são ótimas candidatas para o agente na nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, permitindo que a pessoa desenvolvedora cuide de outras atividades e depois revise o trabalho para garantir que tudo esteja correto.

## Conheça o agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa, há várias maneiras de atribuir tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um rascunho de pull request para acompanhar o andamento, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — não é possível pular a etapa de raciocínio. Mesmo operações aparentemente simples ficam complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente na nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu a atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo rapidamente. Antes de atribuir trabalho ao agente na nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório, sem alterá-los, para depois identificar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace, caso já tenha um em execução:

- `.github/copilot-instructions.md` — examine a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos correspondentes a `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários no pull request resultante. Essas orientações vêm dos arquivos de instruções que você acabou de examinar. Voltaremos a esse assunto ao revisar os PRs em uma lição posterior.

## Configure o ambiente de desenvolvimento do agente na nuvem do Copilot

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale para as tarefas atribuídas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml` e executado antes de o agente começar. Assim, ele pode acessar as ferramentas de desenvolvimento e dependências necessárias. O workflow já está configurado para facilitar o laboratório e esta oportunidade de aprendizado. Ele fornece acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no menor nível necessário para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se quiser clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o workflow funciona sem esperar o Copilot executá-lo.

## Adicione documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, o que reduz a produtividade e dificulta a manutenção da base de código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é muito útil para criar documentação, e esta é uma issue adequada para atribuir ao agente na nuvem. Ele trabalhará em segundo plano para produzir a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse o repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e definindo uma abordagem para a tarefa.

13. Revise a lista e as tarefas que serão realizadas.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode acompanhar a sessão em tempo real e ver como o Copilot resolve o problema. Ele explora o código, entende o estado, pausa para planejar a abordagem adequada e cria código.

Isso provavelmente levará alguns minutos. Um dos objetivos principais do agente na nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, escrever código para adicionar funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente na nuvem do GitHub Copilot é poder dividir o trabalho: você cuida de um conjunto de tarefas enquanto ele trabalha em outro. Talvez a inclusão de uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas ainda assim você pode usar esse tempo em outras atividades. Vamos atribuir a tarefa ao agente na nuvem do Copilot.

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente na nuvem funciona de maneira semelhante a uma pessoa engenheira de software: não é necessário monitorá-lo continuamente, mas sim revisar o trabalho quando ele for concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente na nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam executadas de forma assíncrona. Use o Copilot para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam realizadas com segurança;
- definir issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente na nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e pode receber instruções personalizadas, que foram abordadas nos módulos anteriores.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
