---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena mudança nos cartões de jogos, revise-a em um navegador encaminhado e integre-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira mudança no projeto. Você manterá o escopo pequeno: os jogos já têm uma avaliação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a mudança e fará o merge como seu primeiro pull request.

Nesta lição, você:

- iniciará uma conversa focada do Copilot em um branch de funcionalidade.
- pedirá ao agente que faça uma pequena mudança no projeto.
- revisará a mudança com `/diff`.
- executará o aplicativo para confirmar a mudança em um navegador encaminhado.
- abrirá e integrará seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Os cartões da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que mostre em cada cartão a avaliação existente — uma mudança pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e o branch do Git atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e o branch continuam como estado normal do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, a atividade das ferramentas que o agente usa para explorar e editar arquivos e as mudanças que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a mudança

Vamos iniciar uma nova conversa para implementar a funcionalidade.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use o prompt a seguir para solicitar a mudança:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acabou de adicionar uma funcionalidade com o Copilot CLI!

## Revise as diferenças

Todas as mudanças geradas por IA precisam ser revisadas antes do merge, mesmo as pequenas. Vamos examinar as mudanças diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme se o cartão do jogo exibe a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes abrangem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verifique as mudanças

Não devemos simplesmente ler o código e presumir que ele funciona. Vamos pedir ao Copilot que inicie o site para que possamos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cartões dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e integre seu primeiro pull request

Você criou a funcionalidade! Agora é hora de abrir um pull request (PR) para integrar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI envia o commit existente e exibe a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize seu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança usando o GitHub Copilot CLI. Especificamente, você:

- iniciou uma conversa focada do Copilot em um branch de funcionalidade.
- orientou o agente a fazer uma pequena mudança nos cartões dos jogos.
- revisou a mudança com `/diff`.
- executou o aplicativo para confirmar a avaliação por estrelas em um navegador encaminhado.
- abriu e integrou seu primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
