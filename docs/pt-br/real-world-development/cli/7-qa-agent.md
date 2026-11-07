---
title: "Lição 7 - Crie e use um agente QA"
description: "Crie um agente QA personalizado que combine requisitos de issues, a skill quality-checks e o Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o Playwright MCP para observar a experiência de filtragem no navegador. Agora, reunirá esses recursos em um agente personalizado com um processo QA bem definido.

Nesta lição, você:

- explorará como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criará e inspecionará um perfil reutilizável de garantia de qualidade.
- selecionará o agente QA e revisará as constatações com base na issue de filtragem.

## Cenário

A Tailspin Toys quer revisar requisitos, qualidade do código, verificações automatizadas, cobertura de testes e comportamento no navegador de forma consistente antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O Playwright MCP fornece ferramentas de navegador. O perfil QA orienta o Copilot sobre como usar esses recursos para avaliar os requisitos e relatar as constatações. Ele não substitui esses recursos nem exige outra conversa.

## Crie o perfil QA

Antes de abrir o PR da funcionalidade, você pedirá ao Copilot que crie um perfil QA reutilizável. O perfil definirá tanto as verificações que o QA executará quanto os limites que precisará respeitar.

1. Volte ao Codespace e confirme se a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar se o Copilot registrou o fluxo de QA e os limites pretendidos. Assim, você evita que um agente incompleto ou com escopo amplo demais altere a funcionalidade quando você só quer verificá-la.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas dá ao agente um nome claro na interface.
3. Leia as instruções do perfil e confirme se o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme se o QA apresenta evidências, pergunta antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão que o revise antes de continuar.

## Execute o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa é iniciada. Retome a mesma conversa de filtragem depois de criar o perfil e selecione o QA para que ele possa usar a issue e as decisões de planejamento já presentes no contexto.

1. Ative o agente com este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele não apareça na lista de agentes disponíveis. Ele está disponível e será ativado pelo comando acima.

2. Use o prompt a seguir para pedir ao agente QA que revise a funcionalidade:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. O agente QA começará a trabalhar.
4. Quando terminar, leia o relatório.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou seu resultado. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente QA e revisou suas constatações com base na issue de filtragem.

Agora, a implementação, a atualização da skill, o perfil QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, você [reunirá tudo em um PR de funcionalidade e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
