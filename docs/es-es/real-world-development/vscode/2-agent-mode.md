---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Normalmente, el flujo de trabajo consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otra actualización... y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar con mayor autonomía en el IDE. Se comporta de forma parecida a una persona desarrolladora: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. Cloud agent trabaja de forma asíncrona como una persona compañera del equipo, e interactúa contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás:

- cómo el modo de agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina los cambios.
- cómo implementar funcionalidades nuevas en la interfaz y la capa de datos.
- cómo revisar los cambios y las pruebas generadas por el modo de agente antes de fusionarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que se filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales; ahora completarás el resto del trabajo de datos, interfaz y pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para entender sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha configurado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Ejecútala en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal para abrir el sitio.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Este túnel privado conecta con el Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre su página de detalles con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indica cuando corresponde.

La implementación inicial del sitio funciona, pero queremos añadir nuevas funciones. Empecemos por el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias; pide a GitHub Copilot que la muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Las personas autoras de este taller no recomiendan un modelo concreto. Usamos Claude Sonnet 4.5 al crear el taller y lo incluimos en estas instrucciones para intentar obtener sugerencias relativamente coherentes y ofrecer una experiencia adecuada. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es normal que las sugerencias difieran de las indicadas.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot podría usar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según el beneficio que podrían aportar a las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que dará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales para añadir funcionalidad, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar pautas, dividir instrucciones complejas en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios independientes:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios. El modo de agente de Copilot puede ocuparse de estas tareas.

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, incluidos los helpers de datos, la interfaz y las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras tanto, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has visto cómo:

- el modo de agente puede implementar funcionalidades en la interfaz y la capa de datos.
- Copilot explora el proyecto, identifica archivos pertinentes y coordina los cambios.
- revisar los cambios y las pruebas generadas por el modo de agente antes de fusionarlos en el código base.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo que has aprendido para pedir a Copilot que implemente esta función. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si la persona usuaria podrá seleccionar el tamaño de página o si estará fijado.
- incluye en el prompt las indicaciones necesarias para obtener la implementación que necesitas.
- puede que tengas que iterar con GitHub Copilot y pedir cambios o proporcionar contexto. Ese es el flujo normal de trabajo con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: para qué sirven y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
