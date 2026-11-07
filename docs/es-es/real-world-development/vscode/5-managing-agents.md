---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat en VS Code te muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- dirigirás al agente durante la sesión para añadir un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado además un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de empezar otra conversación, dirigirás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerá una diferencia en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos con los que estés de acuerdo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final, mostrará un resumen de los cambios.

## Pedir al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la conversación para añadir un control del modo claro. Al continuar en la misma sesión, el agente conservará el contexto del trabajo anterior, como los componentes que modificó, el enfoque de estilos y el uso del almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde el trabajo anterior, explorará los cambios y propondrá mejoras para el modo claro.
3. Revisa cada edición nueva a medida que aparezca, igual que con el modo de alto contraste. Utiliza **Keep** o **Undo** para cada diferencia.
4. Si el agente toma una dirección que no quieres (por ejemplo, intenta reorganizar la interfaz de los controles), detenlo con el botón **Stop** junto al campo de chat y envía un mensaje de seguimiento que aclare el enfoque.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS y utiliza variables CSS en vez de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar los archivos modificados

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que siguen las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos concretas.
- has dirigido una sesión en curso con prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente local personalizado durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
