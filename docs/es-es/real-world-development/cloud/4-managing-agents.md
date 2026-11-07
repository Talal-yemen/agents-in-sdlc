---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se ocupara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de operaciones puedes ver todas las tareas de agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo ha decidido que también sería buen momento para añadir un modo claro. Como ya se están actualizando los estilos y se está añadiendo funcionalidad para cambiar de modo, parece lógico incluirlo. Dirigirás el trabajo del agente para que añada tanto el modo claro como el alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Veamos el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot, por ejemplo:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar varias operaciones para completar la tarea.

> [!NOTE]
> Los pasos exactos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya utilizado.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Desde allí puedes consultar la solicitud y los archivos modificados para seguir supervisando el trabajo.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya utilizado y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que haya terminado de configurar el entorno y empiece a trabajar. Lo sabrás cuando empiecen a aparecer mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes, con el cuadro para dirigir la sesión en curso resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde este panel puedes realizar el seguimiento de las tareas activas y completadas, revisar el trabajo y cambiar el rumbo de las sesiones en curso para ajustar el alcance o aportar más orientación.

Has explorado estos conceptos:

- utilizar el panel de operaciones y la página de agentes para supervisar tareas.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, podemos [iterar sobre las solicitudes de incorporación de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
