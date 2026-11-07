---
title: "Lección 0: Requisitos previos"
description: "Prepara el entorno para las lecciones de la aplicación GitHub Copilot: instala Node.js para Tailspin Toys y crea una copia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que funciona como centro de operaciones para Copilot y GitHub. Proporciona acceso rápido a incidencias y solicitudes de incorporación de cambios y permite desarrollar con GitHub Copilot. En este taller trabajarás en local con la aplicación Tailspin Toys, creada con Astro, mediante la aplicación GitHub Copilot. Antes de empezar, comprueba que Node.js está instalado en tu equipo y, después, instala la aplicación Copilot.

En esta lección:

- instalarás Node.js para ejecutar las pruebas del proyecto en tu equipo.
- crearás una copia del proyecto Tailspin Toys a partir de la plantilla.

## Instalar Node.js

En varias lecciones pedirás a un agente que cree funciones y ejecute el conjunto de pruebas de Tailspin Toys en local. Para ello se necesita **[Node.js][nodejs]**, el único entorno de ejecución que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla en todas las plataformas es utilizar el instalador oficial:

1. Abre un terminal en el sistema operativo con Windows Terminal, Terminal de macOS o la aplicación que utilices habitualmente.
2. Ejecuta estos comandos para comprobar las versiones instaladas de Node.js y npm:

    ```shell
    node --version
    npm --version
    ```

3. Si se ejecutan correctamente y Node.js cumple los requisitos del README y de `package.json` del proyecto, puedes pasar a la sección siguiente.

> [!TIP]
> Solo tienes que completar estos pasos si no tienes Node instalado o si necesitas actualizarlo.

4. Abre la [página de descarga de Node.js][node-download].
5. Descarga la versión **LTS** de tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, mantén seleccionada la opción **Add to PATH**.
7. Cuando termine la instalación, abre un terminal nuevo.
8. Confirma la instalación ejecutando estos comandos en el terminal nuevo:

    ```bash
    node --version
    npm --version
    ```

9. Deberían aparecer las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en el terminal habitual no garantiza que estén disponibles en la aplicación. Cierra completamente la aplicación y vuelve a abrirla después de instalar Node.js o cambiar `PATH`. Si alguno de los comandos sigue fallando en una sesión, sigue los [pasos de solución de problemas del entorno de la sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada árbol de trabajo también necesita las dependencias del proyecto y Chromium de Playwright para las comprobaciones E2E. Sigue el README del repositorio de Tailspin Toys al preparar un árbol de trabajo y revisa cualquier solicitud de instalación antes de aprobarla.

## Configurar el repositorio del taller

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][template-repository]. El nuevo repositorio contiene todos los archivos necesarios para el taller y lo conectarás cuando instales la aplicación.

1. Abre una ventana nueva del navegador y ve al repositorio de GitHub del taller: `https://github.com/github-samples/tailspin-toys`.
2. Crea tu propia copia del repositorio seleccionando **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template con la opción Create a new repository seleccionada en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio rellenado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), porque la necesitarás más adelante en el taller.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

Utiliza una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, el código de la aplicación, pruebas, una habilidad `quality-checks` y una extensión de lienzo existente. Personalizarás la habilidad y crearás un agente de QA durante el taller. Si utilizas una copia antigua, confirma con la persona que facilita el taller que contiene los archivos necesarios.

## Resumen y pasos siguientes

Ya tienes todo preparado. En esta lección:

- has instalado Node.js para que el proyecto se pueda compilar y probar en tu equipo.
- has creado tu propia copia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio que acabas de crear y te familiarizarás con el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#iniciar-una-sesión-desde-la-incidencia
