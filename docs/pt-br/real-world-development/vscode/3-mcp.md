---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador para testar o recurso. Depois, publique sua branch e abra o pull request.

Neste exercício, você vai:

- entender o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot;
- adicionar o servidor MCP do Playwright ao espaço de trabalho;
- pedir ao Copilot que o use para testar manualmente a filtragem em um navegador;
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais útil quando pode acessar ferramentas externas. O MCP é um padrão que permite ao agente conversar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem em tempo real com ferramentas e serviços externos. Eles podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

Um servidor MCP faz a ponte entre o agente de IA e as ferramentas e serviços externos, gerenciando a comunicação com recursos como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos ao agente.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub. O agente pode criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador por meio do Playwright. O agente pode navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP para ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações sensíveis.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Observe que já há um servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não é necessário instalá-lo localmente. O VS Code conecta-se por HTTP, e você autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, deixando o arquivo assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é local: o VS Code inicia o processo na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário em um codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e pedirá que você confirme se confia nele.

1. Em `.vscode/mcp.json`, localize a ação **Start** acima da entrada `playwright` e selecione-a para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para que ele possa iniciar.

É opcional iniciar o servidor manualmente. Se você não fizer isso, o VS Code o iniciará e pedirá sua confirmação na próxima etapa, quando o Copilot precisar das ferramentas.

## Teste o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece um navegador real para o Copilot controlar. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, acessar outras páginas, aplicar filtros, ler os resultados e resumir o que observou.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente trabalha com elementos rotulados e estruturados, como botões, links e itens de lista, da mesma forma que tecnologias assistivas. Um teste funcional rápido também oferece uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

1. Envie este prompt:

    ```text
    Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

    1. Go to the games page and note how many games are listed.
    2. Apply a category filter and confirm the list updates to only show games in that category.
    3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
    4. Combine a category and a publisher filter and confirm the results respect both.

    Report what you observe at each step, and call out anything that does not behave as expected.
    ```

O Copilot iniciará um navegador pelo servidor MCP do Playwright, seguirá as etapas e relatará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo não estiver correto, faça perguntas de acompanhamento ou peça para corrigir o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o agente informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para que a equipe possa revisar o trabalho.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request (PR), inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR, encontre a issue correspondente e associe-a.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado na lista de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se; siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link informado pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot;
- adicionou o servidor MCP do Playwright ao espaço de trabalho;
- usou o servidor para testar manualmente a filtragem em um navegador;
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para testar um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
