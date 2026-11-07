---
title: "Lección 8: Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, autoriza Agent Merge y confirma la combinación."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlo todo junto y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias solicitudes. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- revisarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la solicitud y resuelve los impedimentos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la rama sobre su base cuando es necesario. La combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando al reiniciar la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede hacerse cargo de esa responsabilidad, pero para editar código y combinar todavía necesita tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Utilizar Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Dejemos que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Seleccionar la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge comienza. Hará lo siguiente:

- Crear una solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, incluir una referencia a la incidencia relacionada en el cuerpo de la descripción.
- Reorganizar la rama sobre su base o gestionar los posibles conflictos con la rama de destino.
- Supervisar el proceso de CI para garantizar que se superen todas las comprobaciones.
- Supervisar los comentarios de otros desarrolladores y de la revisión de código de Copilot, y realizar cambios para responder a ellos.
- Opcionalmente, combinar automáticamente la PR cuando se haya superado todo.

Dejemos que Agent Merge también combine la PR cuando todas las comprobaciones se hayan superado.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que está marcada la opción **Merge pull request**.
8. Pide explícitamente al agente que combine la solicitud:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el progreso y resuelve los bloqueos que indique el agente. Confirma que el estado de la PR es **Merged** antes de continuar. Tener permiso para combinar o un agente en ejecución no confirma que la PR ya se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación, y ahora el flujo de las solicitudes de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- has revisado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado la combinación de la PR.

A continuación, [utilizarás un lienzo existente y crearás un lienzo de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
