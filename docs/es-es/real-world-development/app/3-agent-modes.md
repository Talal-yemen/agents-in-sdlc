---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña función al proyecto, pero los cambios mayores necesitan un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo de una organización y ayudarnos a desarrollar lo correcto de la forma adecuada. En esta lección, la primera de varias, seguirás un proceso de desarrollo habitual dirigido por agentes: partirás de una incidencia para crear una función, comprobarás que el código es válido y que la función se comporta como se espera y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de la funcionalidad. Normalmente usarías sesiones o PR distintas para los diferentes tipos de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una nueva sesión de agente desde una incidencia de GitHub.
- definirás los requisitos en modo **Plan**.
- implementarás la funcionalidad con el modo **Autopilot**.
- revisarás el código.
- validarás manualmente la funcionalidad en un lienzo del navegador.

Mientras continúas con esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan acotar la lista de juegos por categoría y editor. La incidencia del backlog describe la función, pero antes de programar hay que acordar detalles como la combinación de categorías. Utilizarás el modo Plan para resolver estas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un flujo parecido:

1. Abrir una incidencia registrada con los detalles del trabajo.
2. Crear un plan de lo que hay que desarrollar.
3. Escribir y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Comprobar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se haya completado correctamente el proceso de integración continua.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen un patrón parecido.

Este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** determina cuánta autonomía tiene el agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. El agente sugiere cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente primero crea un plan. Revisas y apruebas el plan antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar instrucciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Iniciar una sesión desde la incidencia

Antes de empezar, confirma que la PR de las valoraciones por estrellas se ha combinado y que tu `main` local está actualizado.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys y, después, selecciona **Issues** en la navegación superior del repositorio. Abre la incidencia existente **Allow users to filter games by category and publisher**. No hace falta crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, comprueba la variable `PATH` o la configuración del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que hayan fallado. No reinstales npm solo porque el agente no lo encuentre.

## Planificar la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud de la función como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editor, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para la validación posterior.

## Aprobar Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba que el indicador de modo muestra **Autopilot**.

Copilot comenzará a implementar la funcionalidad. Verás cómo sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisar y comprobar la implementación

Como cualquier otro código, hay que revisar el código generado antes de combinarlo. Revisemos el código y ejecutemos el sitio para comprobar que funciona.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editores. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio en un lienzo del navegador.

1. Utiliza este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo del navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona un editor y vuelve a comprobar los resultados.
4. Combina los filtros de categoría y editor y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo de la sesión y cierre el lienzo del navegador:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantener la sesión para las lecciones siguientes

Todavía no crees ni combines la PR de filtrado. Conserva esta sesión, el árbol de trabajo y la rama para las lecciones 4 a 8. En la lección siguiente aplicarás instrucciones personalizadas al código de filtrado y, después, añadirás mejoras de habilidades y QA antes de crear y combinar la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener el servidor de desarrollo; vuelve a la misma sesión para la lección siguiente.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para desarrollar y revisar una función. En esta lección:

- has iniciado una sesión de agente nueva desde una incidencia de GitHub.
- has definido los requisitos en modo **Plan**.
- has implementado la función con el modo **Autopilot**.
- has revisado el código.
- has validado manualmente la funcionalidad en un lienzo del navegador.

A continuación, analizaremos cómo se genera el código y cómo seguir las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
