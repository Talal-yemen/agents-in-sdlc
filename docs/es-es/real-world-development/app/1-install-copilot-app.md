---
title: "Lección 1: Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en vez de realizar todo el trabajo por tu cuenta, y automatizas tareas repetitivas. Con Node.js instalado y una copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- te familiarizarás con el espacio de trabajo, incluido el backlog creado por la plantilla.
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instalar y configurar la aplicación GitHub Copilot

Como cabe esperar, el primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Vamos a instalar la aplicación, autenticarnos y añadir nuestro repositorio Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si utilizas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** y, cuando se te solicite, introduce el nombre de host `*.ghe.com`. GitHub Enterprise Server no es compatible.
5. Si el proceso inicial te pide que selecciones un repositorio, selecciona el repositorio Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadir el proyecto más adelante.
6. Cuando se te solicite, elige un tema y termina la configuración inicial.
7. Busca tu repositorio en **Sessions**, en la barra lateral. Si ya aparece, no hace falta volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Indica el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En el diseño utilizado aquí, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions** y las sesiones individuales se muestran debajo de cada repositorio.

## Familiarizarse con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza todo en distintas áreas de la barra lateral:

- **New**: inicia una sesión de chat con Copilot.
- **Pull requests**: solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Desde aquí puedes explorarlas y filtrarlas, comprobar el estado de CI y revisar sus cambios sin salir de la aplicación.
- **Issues**: incidencias del proyecto. Desde aquí puedes explorarlas y filtrarlas, e iniciar una sesión a partir de una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Sirven para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar otras tareas repetitivas. El repaso las presenta como un paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. Lo utilizarás para configurar Playwright MCP.
- **Chats**: conversaciones ligeras para preguntas o lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona el nombre del repositorio, como `tailspin-toys`, para abrir la vista del repositorio, o selecciona una sesión debajo para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, por lo que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás la primera sesión cuando añadas las valoraciones por estrellas.

Durante el taller explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede ayudarte a orientarte.

### Encontrar el backlog creado por la plantilla

Prácticamente todos los proyectos tienen trabajo pendiente y Tailspin Toys no es una excepción. Exploremos las incidencias creadas al preparar el repositorio a partir de la plantilla.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título en lugar de dar por hecho su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también sirve como punto de partida para una sesión de agente. Partirás de la incidencia de filtrado después de realizar un primer cambio breve.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de trabajo de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Probar un chat rápido

Una buena forma de familiarizarse con la aplicación es utilizarla para conocer la *propia aplicación*. Para eso sirve un **chat rápido**. Puedes hacer una pregunta o plantear ideas sin crear una rama o un árbol de trabajo, por lo que es ideal para consultas breves que no necesitan una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar tu primer chat. **Chats** aparecerá después.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra en cualquier momento.

## Resumen y pasos siguientes

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- utilizar un chat rápido para realizar una consulta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
