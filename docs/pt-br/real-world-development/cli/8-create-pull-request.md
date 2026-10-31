---
title: "Lição 8 - Crie e integre o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra um pull request. Você integrou manualmente o pull request (PR) de avaliações por estrelas; desta vez, deixe que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, o recurso, as atualizações de instruções e da skill e o agente de QA seriam divididos em alguns PRs. Para agilizar o workshop, todo o fluxo de filtragem e qualidade permaneceu em uma conversa e branch e será incluído neste PR.

Nesta lição, você vai:

- conhecer o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionar toda a alteração do recurso e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi integrado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request. Quando habilitado, o Copilot resolve o que estiver bloqueando o PR: corrige verificações de integração contínua (CI) que falharam, responde a comentários de revisão e faz rebase quando necessário. Depois, habilita o merge automático do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Antes de habilitá-lo, revise o trabalho e confirme que ele está pronto.

## Use o Agent Merge para gerenciar o PR

Com o código pronto, revise-o, crie o PR e permita que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agente digitando `/agent`.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua do PR e fará o merge quando tudo for aprovado.
7. Confirme se o Copilot informa algo semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branches, filas de merge, configurações do repositório nem permissões ausentes. Se o processo estiver bloqueado, leia o motivo informado e, quando o repositório permitir, conclua manualmente o merge que você já revisou.

## Resumo e próximos passos

Você automatizou várias etapas do desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Nesta lição, você:

- conheceu o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionou a alteração completa do recurso e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi integrado.

Em seguida, [explore outros comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
