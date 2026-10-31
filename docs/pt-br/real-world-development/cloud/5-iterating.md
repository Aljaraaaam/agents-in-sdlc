---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário e adicionar funcionalidades: documentar o código, criar uma seção de jogos relacionados e implementar recursos de acessibilidade com modos de alto contraste e claro. Agora, revise as alterações e, se necessário, envie comentários ao Copilot para aprimorar o trabalho.

### Cenário

Os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Antes de habilitar a revisão para o restante da equipe, revise as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot trabalha de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions e cria um ambiente separado e temporário para trabalhar.
- Uma pessoa precisa aprovar a execução de qualquer workflow do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o que adiciona documentação ao código. Faça a revisão pela interface padrão de PRs do GitHub.com.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os comentários TSDoc recém-criados e outras documentações. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas alterações seguem os arquivos de instruções personalizadas que você examinou no início do Exercício 2.
6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. As verificações do projeto para o aplicativo Astro devem ser aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários de um pull request ou em comentários sobre linhas de código, como faria com outra pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto único a usar. Você pode pedir, por exemplo, para:

- adicionar cabeçalhos de comentário no início de cada arquivo de código, descrevendo sua finalidade;
- adicionar comentários TSDoc a arquivos TypeScript e Astro;
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**. Use uma das sugestões acima ou peça outra melhoria de documentação para o código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, procure um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até que o resultado esteja satisfatório. Depois, marque o PR como pronto para revisão e faça o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, revise o PR criado pelo Copilot para adicionar jogos relacionados à página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
6. Procure um indicador de que alguns workflows aguardam aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do pull request. As verificações do projeto para o aplicativo Astro devem ser aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** teste manualmente o recurso de jogos relacionados no codespace. Abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Você pode continuar iterando com o agente de nuvem para solicitar alterações, como mudar a quantidade de jogos exibidos ou adicionar cabeçalhos de comentário e comentários TSDoc. Como essa issue foi atribuída **antes** da atualização das instruções personalizadas, você também pode pedir que esses comentários sejam adicionados. Para isso, publique outro comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no painel central do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, com atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade, como rótulos da especificação Accessible Rich Internet Applications (ARIA) e navegação por teclado;
   - código JavaScript ou TypeScript que controla a alternância entre modos.
6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. As verificações do projeto para o aplicativo Astro devem ser aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** teste manualmente os recursos de acessibilidade no codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Depois, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se eles funcionam e se as preferências permanecem após recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar os recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou melhorias possíveis, mencione **@copilot** em um comentário para solicitar atualizações.

## Exercício opcional: continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para praticar, crie algumas issues no repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse para apoiadores na página de detalhes do jogo;
- implementar paginação na lista de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Examinou os arquivos de instruções personalizadas do repositório** para ver como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e aprimorou pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar, explore outros percursos que complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma integração opcional com o agente de nuvem por meio de `/delegate`.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes;
- criar um agente personalizado para outro domínio;
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

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
