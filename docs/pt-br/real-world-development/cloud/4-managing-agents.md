---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem tenha sido criado para trabalhar de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você vê todas as tarefas de agentes com pull requests (PRs) abertos. Você pode explorar as operações realizadas e até orientar uma sessão em andamento.

Nesta lição, você:

- explorará a página de agentes para monitorar as tarefas do agente de nuvem.
- orientará uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar uma funcionalidade de alternância, pareceu lógico incluir esse modo. Você quer orientar o agente para garantir que ele adicione um modo claro além do alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você verá as tarefas que atribuiu ao Copilot, incluindo:
   - Atualizar a documentação da base de código.
   - Gerar APIs para modificar jogos.
   - Adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as ações realizadas pelo Copilot. Elas podem incluir:
   - Clonar o código do repositório.
   - Criar o ambiente de trabalho do Copilot.
   - Configurar servidores MCP.
   - Executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam de acordo com o estado do trabalho do Copilot e a abordagem utilizada.

4. Observe também o painel de pull request (PR) exibido à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você viu as tarefas ativas, vamos pedir ao Copilot que inclua uma alternância para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando começarem a aparecer mensagens semelhantes às seguintes.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada! Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa específica que estava executando quando você enviou a mensagem.

Assim como antes, isso levará alguns minutos; aproveite esse tempo para refletir sobre tudo o que aprendeu e explorou até agora.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, seu painel central para monitorar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nesse painel, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho realizado e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- explorar o painel de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre os agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
