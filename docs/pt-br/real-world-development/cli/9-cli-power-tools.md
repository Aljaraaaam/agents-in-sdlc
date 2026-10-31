---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como outras boas ferramentas de interface de linha de comando (CLI), o GitHub Copilot CLI inclui comandos de barra para interagir com ele. Esses comandos oferecem recursos avançados, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Agora, explore outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usar `/model` para conhecer os modelos disponíveis;
- aprender como `/share` exporta ou compartilha uma sessão;
- explorar comandos opcionais para trabalho em paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora, explore outros recursos: gerenciamento de contexto, troca de modelo, compartilhamento de sessões e delegação opcional ao [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Em tarefas grandes ou complexas, você pode atingir o limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário; os comandos de barra permitem inspecionar ou compactar o contexto manualmente.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. A diferença pode ser pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` para escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação e iniciar uma conversa limpa.

## Escolha um modelo

Modelos diferentes têm pontos fortes diferentes, e cada pessoa desenvolvedora pode ter preferências próprias. O Copilot CLI permite listar e selecionar o modelo.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Compartilhar aprendizados ajuda a equipe a aprimorar o uso de ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e revise as opções de `/share` na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino apropriado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretende compartilhar.

## Opcional: amplie o trabalho ou delegue

O fluxo principal do workshop está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo;
- `/worktree` pode criar uma worktree Git isolada para outra tarefa;
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou trabalho remoto. Para experimentá-los, siga este processo:

1. Comece uma tarefa nova e com escopo claro.
2. Revise os resultados pelo fluxo de trabalho habitual.

Para saber mais sobre trabalho assíncrono com agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra permitem configurar o Copilot CLI, compartilhar sessões e conhecer seu funcionamento interno. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto;
- usou `/model` para conhecer os modelos disponíveis;
- aprendeu como `/share` exporta ou compartilha uma sessão;
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há outros comandos de barra para explorar. Agora, [revise o que aprendeu][next-lesson] e veja os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
