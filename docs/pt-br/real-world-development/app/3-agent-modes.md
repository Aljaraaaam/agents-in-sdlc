---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores exigem um processo mais robusto. O aplicativo GitHub Copilot acompanha o fluxo de trabalho da organização para ajudar a criar os recursos certos da maneira correta. Nesta lição, você seguirá um processo comum de desenvolvimento orientado por agentes: partir de uma issue para criar um recurso, confirmar que o código é válido e que o recurso funciona como esperado e, por fim, integrá-lo ao projeto.

> [!NOTE]
> Você usará a mesma sessão durante todo o fluxo de trabalho do recurso. Normalmente, seriam usadas sessões ou pull requests (PRs) diferentes para cada tipo de arquivo, mas este workshop reúne o trabalho para enfatizar os conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub;
- definir requisitos no modo **Plan**;
- implementar o recurso no modo **Autopilot**;
- revisar o código;
- validar o recurso manualmente em um canvas de navegador.

Nas próximas lições, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação pelo Model Context Protocol (MCP), criará um agente de garantia de qualidade (QA) e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e as pessoas precisam filtrar jogos por categoria e editora. A issue do backlog descreve o recurso, mas alguns detalhes, como combinar categorias, precisam ser definidos antes de começar a programar. Use o modo Plan para decidir a abordagem e depois autorize uma implementação com escopo definido no modo Autopilot.

## Contexto

Introduzir agentes de programação com IA ao fluxo de desenvolvimento não muda seus fundamentos. A maioria das pessoas desenvolvedoras segue um processo semelhante:

1. Abra uma issue registrada com os detalhes do trabalho.
2. Crie um plano do que precisa ser desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, faça o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma abordagem semelhante.

Esse processo ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelas mesmas revisões que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode selecioná-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você trabalha com o agente, que sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você o revisa e aprova antes que o agente o execute.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera sem aguardar sua orientação.

Comece no modo Plan, revise o plano e depois use Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Prepare a sessão a partir da issue depois de confirmar que o PR de avaliações por estrelas foi integrado e que sua branch `main` local está atualizada.

1. Confirme que o PR de avaliações por estrelas foi integrado e que sua branch `main` local está atualizada.
2. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já existe no modelo; não crie outra.
3. Selecione **New session** e escolha uma **new working tree** baseada em `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

4. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não for encontrado, peça ao agente para executar `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o `PATH` ou a configuração do gerenciador de versões do aplicativo e feche e reabra o aplicativo completamente. Repita a operação e as verificações que falharam. Não reinstale o npm apenas porque o agente não o encontrou.

## Planeje o recurso de filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva código. Como você começou pela issue, o Copilot já tem a solicitação como contexto. Envie:

1. Peça ao Copilot que crie um plano:

   ```plaintext
   Build this feature.
   ```

2. Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Verifique se ele abrange a filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Discuta qualquer comportamento que não esteja claro, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.
3. Confirme se o plano inclui lint, testes unitários, testes de ponta a ponta (E2E) e verificação de tipos com as ferramentas existentes do projeto. Mantenha-o focado na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade.
4. Peça alterações no plano antes de aprová-lo e mantenha à mão a URL da issue e os esclarecimentos acordados para a validação posterior.

## Aprove explicitamente o Autopilot

Quando estiver satisfeito com o plano, aprove-o e confirme o modo antes da implementação.

1. Selecione **Approve and implement this plan** ou a opção equivalente na sua versão.
2. Confirme que o indicador mostra **Autopilot**.

O Copilot começará a trabalhar na implementação, seguindo o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revise e verifique a implementação

Revise o código e execute o site antes de integrá-lo, como faria com qualquer outra alteração.

1. Abra **Changes** e examine a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovada.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright cria e disponibiliza uma versão de pré-visualização e pode reutilizar um servidor local. Confirme que o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explore a nova funcionalidade

Agora que o código parece correto, confirme que ele funciona. Inicie o aplicativo e abra o site em um canvas de navegador.

1. Peça ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme os resultados.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que pare o servidor iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Mantenha o trabalho de filtragem na sessão atual para continuar o fluxo de qualidade nas próximas lições.

1. Não crie nem integre o PR de filtragem ainda.
2. Mantenha esta sessão, worktree e branch para as Lições 4 a 8. Em seguida, aplique instruções personalizadas ao código de filtragem e adicione melhorias à skill e ao agente de QA antes de criar e integrar o PR combinado na Lição 8.
3. Depois de parar o servidor de desenvolvimento, deixe a sessão inativa e volte a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para criar e revisar um recurso. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub;
- definiu requisitos no modo **Plan**;
- implementou o recurso no modo **Autopilot**;
- revisou o código;
- validou manualmente o recurso em um canvas de navegador.

Em seguida, [explore as instruções personalizadas][next-lesson] para que o código siga as práticas documentadas.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
