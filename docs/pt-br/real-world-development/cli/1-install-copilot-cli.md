---
title: "Lição 1 - Instale o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça a interface e encontre a issue de filtragem criada pelo modelo."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um assistente de programação agêntico avançado que funciona no terminal e permite explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas — tudo pela linha de comando. Ele permite delegar tarefas, solicitar mudanças e manter o foco. Como você pode imaginar, o primeiro passo é instalar a ferramenta. Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você:

- instalará o GitHub Copilot CLI usando npm.
- fará a autenticação com sua conta do GitHub.
- confiará no repositório do workshop e experimentará uma conversa rápida.
- encontrará a issue de filtragem pelo servidor MCP integrado do GitHub.

## Cenário

Sua equipe está começando a usar agentes de IA para avançar em um backlog crescente. O Copilot CLI leva essa capacidade para o terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você fará a instalação e a autenticação e estará pronto para usá-lo no restante do workshop.

## Instale o Copilot CLI

Você pode instalar o Copilot CLI por [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará o npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme se o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou superior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifique a instalação:

   ```bash
   copilot --version
   ```

   O número da versão deverá aparecer.

## Faça a autenticação com o GitHub

Na primeira inicialização, o Copilot CLI solicitará que você autentique sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções de autenticação com código do dispositivo e autorize o Copilot CLI.
3. O Copilot CLI exibirá este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme se o caminho corresponde ao repositório Tailspin Toys e responda afirmativamente selecionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, talvez você já esteja autenticado por meio da sessão do GitHub. Se o Copilot CLI iniciar sem pedir autenticação, está tudo certo.

## Conheça a interface

Os comandos digitados no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI inicia, a linguagem natural é enviada ao agente e os comandos slash controlam a conversa.

1. Digite `/model`, use as setas para selecionar **Auto**, pressione <kbd>Enter</kbd> e, em seguida, pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a ajuda.
3. Faça uma pergunta simples ao Copilot para confirmar se tudo está funcionando:

   ```plaintext
   What are the key files in this project?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme se o servidor MCP integrado do GitHub está disponível.
6. Peça ao Copilot que encontre a issue de filtragem:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abra a URL e leia a issue. Você a usará depois de concluir uma primeira alteração rápida.

> [!TIP]
> Uma sessão normal do Copilot CLI trabalha no branch selecionado no terminal; ela não cria uma worktree automaticamente. Você criará um branch de funcionalidade antes de cada mudança.

## Use o atalho do workshop

Normalmente, o Copilot CLI pede autorização antes de usar ferramentas fora das permissões já concedidas. Neste workshop, você o iniciará novamente com `--yolo`, um atalho autorizado por você que remove esses pedidos de permissão dentro do Codespace para que possa se concentrar nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma máquina virtual, e nunca o defina como alias padrão para o desenvolvimento diário. Consulte [Permitir e negar o uso de ferramentas][allow-all-warning] para obter detalhes.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas MCP de leitura e gravação do GitHub usadas nas lições posteriores para trabalhar com issues e pull requests. O Codespace limita o acesso ao computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as mudanças antes de publicá-las ou fazer merge.

1. Saia do Copilot CLI com `/exit`.
2. Inicie-o novamente na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar se a conversa está funcionando e, em seguida, saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de alterar instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa de funcionalidade e ao mesmo branch.

## Resumo e próximos passos

Parabéns! Nesta lição, você:

- instalou o GitHub Copilot CLI usando npm.
- fez a autenticação com sua conta do GitHub.
- confiou no repositório do workshop e experimentou uma conversa rápida.
- encontrou a issue de filtragem pelo servidor MCP integrado do GitHub.

Em seguida, você [iniciará sua primeira mudança focada][next-lesson] e usará o Copilot CLI para exibir uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
