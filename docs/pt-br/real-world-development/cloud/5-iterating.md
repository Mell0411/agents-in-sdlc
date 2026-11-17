---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Monitorar agentes][previous-lesson] |
|:--|

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade com opções de alto contraste e modo claro. Vamos explorar as alterações e, se necessário, enviar comentários ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar documentação, implementar o recurso de jogos relacionados e adicionar recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente na nuvem do GitHub Copilot

Como o agente na nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para garantir que tudo continue protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente na nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Um humano precisa aprovar a execução de todos os workflows do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente na nuvem do GitHub Copilot: o PR para adicionar documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente na nuvem do Copilot][agent-firewall].

1. Acesse novamente o repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título parecido com **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request conterá a indicação **[WIP]**. Nesse caso, aguarde até que o Copilot conclua o trabalho. Isso pode levar alguns minutos. Você pode fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os comentários TSDoc recém-criados e outras documentações. As alterações exatas variam.

    Ao revisar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do pull request ou em comentários sobre linhas específicas do código, como faria com outros membros da equipe. O Copilot verá esses comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não podemos fornecer um texto prescritivo para solicitar alterações. Algumas ideias:

- Adicionar cabeçalhos de comentários no início de cada arquivo de código com uma breve descrição da finalidade do arquivo.
- Adicionar comentários TSDoc aos arquivos TypeScript e Astro.
- Criar um README com uma descrição da estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para implementar as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até que o resultado esteja satisfatório. Depois, você poderá marcar o PR como pronto para revisão e mesclá-lo à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, vamos voltar ao PR criado pelo Copilot para resolver a issue de exibir jogos relacionados na página de detalhes de cada jogo.

1. Acesse novamente o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Show related games on the game details page** ou outro título mais específico.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você verá um indicador de que alguns workflows aguardam aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode alternar para essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode trabalhar de forma iterativa com o agente na nuvem para solicitar atualizações. Por exemplo, você pode pedir que o Copilot altere o número de jogos relacionados exibidos ou garanta a inclusão de cabeçalhos de comentários e comentários TSDoc (lembre-se: a issue foi atribuída **antes** da atualização das instruções personalizadas). Assim como nas etapas anteriores, faça essas solicitações adicionando um comentário na guia **Conversation**; o Copilot verá a mensagem e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de controle do Exercício 4.

1. Acesse novamente o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Add high contrast mode to website** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request conterá a indicação **[WIP]**. Nesse caso, aguarde até que ele conclua o trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para persistir as preferências;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - código JavaScript/TypeScript que controla a alternância entre modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** alterne para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador para confirmar que funcionam como esperado e que as preferências são mantidas depois de atualizar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou melhorias de acessibilidade, mencione **@copilot** em um comentário para solicitar atualizações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O agente na nuvem funciona melhor quando você pode delegar itens reais do backlog e se concentrar em outras atividades. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para demonstrar interesse em apoiar o projeto na página de detalhes do jogo.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o ambiente do agente na nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizadas do repositório** para ver como eles afetariam mais tarde o resultado do agente na nuvem.
- **Atribuiu issues ao agente na nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e direcionar a sessão de acessibilidade.
- **Revisou e iterou sobre os pull requests do agente na nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o ambiente do agente na nuvem. Para continuar explorando, os outros ambientes complementam o que você praticou:

- 🖥️ **[Ambiente do VS Code](../../vscode/)** — explore o modo agente e a integração com MCP diretamente no IDE.
- 💻 **[Ambiente do CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando os modos Plan e Autopilot, habilidades de agente, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente na nuvem.

No próprio repositório, experimente estas ideias:

- Atribua ao agente na nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente na nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Monitorar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
