---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você começou com uma pequena funcionalidade, mas alterações maiores exigem um processo mais robusto. O GitHub Copilot CLI foi desenvolvido para acompanhar o fluxo de trabalho da organização e ajudar a criar os recursos certos da maneira adequada. Nesta lição, você seguirá um processo comum de desenvolvimento orientado por agentes: partir de uma issue para criar um recurso, confirmar que o código está válido e que o recurso funciona como esperado e, por fim, integrá-lo ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch durante o fluxo de trabalho do recurso. Normalmente, seriam usadas branches ou PRs diferentes para cada tipo de arquivo, mas este workshop reúne o trabalho para enfatizar os conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub;
- definir requisitos no modo Plan;
- implementar o recurso no modo Autopilot;
- revisar o código;
- validar o recurso manualmente em um navegador encaminhado.

Nas próximas etapas, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e as pessoas precisam filtrar jogos por categoria e editora. A issue do backlog descreve o recurso, mas alguns detalhes, como combinar categorias, precisam ser definidos antes de começar a programar. Use o modo Plan para decidir a abordagem e depois autorize uma implementação com escopo definido no modo Autopilot.

## Contexto

Introduzir agentes de programação com IA ao fluxo de desenvolvimento não muda seus fundamentos. A maioria das pessoas desenvolvedoras segue um processo semelhante:

1. Comece com uma issue que descreva o trabalho necessário.
2. Planeje o que precisa ser criado.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua (CI), faça o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma abordagem semelhante.

Esse processo ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelas mesmas revisões que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você trabalha com o agente, que sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, prepare o codespace, o repositório e o terminal.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o.
2. Confirme que o PR de avaliações por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para mostrar todas as issues.
8. Use as setas para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt começa com `#7` ou um número semelhante. O prefixo `#` adiciona o contexto de uma issue ou pull request (PR) do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso antes de delegar o trabalho ao Copilot. Para tarefas complexas, planeje antes de começar a programar.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot explorará o projeto e definirá uma abordagem. Durante o processo, talvez faça perguntas sobre o funcionamento da filtragem.

3. Responda às perguntas de acordo com suas preferências. Não há respostas certas ou erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que altere qualquer parte que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, implemente-o no modo Autopilot. Esse modo permite que o Copilot itere até considerar que concluiu o trabalho.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot seguir o plano, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Revise o código e execute o site antes de integrá-lo, como faria com qualquer outra alteração.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e examine a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões feitas durante o planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explore a nova funcionalidade

Agora que o código parece correto, confirme que o aplicativo funciona. Inicie-o e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria e editora, além das combinações definidas no plano.
4. Confirme se a limpeza dos filtros e os resultados vazios correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub;
- definiu requisitos no modo Plan;
- implementou o recurso no modo Autopilot;
- revisou o código;
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, [explore as instruções personalizadas][next-lesson] para que o código siga os padrões documentados.

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
