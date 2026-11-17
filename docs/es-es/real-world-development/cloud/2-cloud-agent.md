---
title: "Ejercicio 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog por falta de recursos. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de cambios en borrador para que una persona desarrolladora la revise. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en cuestiones de mayor alcance.

Explorarás con Copilot cloud agent:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. El equipo externo que creó la primera versión del sitio dejó la documentación en mal estado: comprobarás que no hay ninguna. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida; solo algo que permita hacer pruebas de aceptación de la experiencia. Este trabajo está bloqueado, pero hay otras incidencias de mayor prioridad.

Son dos ejemplos de tareas que pueden posponerse fácilmente y que conviene asignar a Copilot cloud agent. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas, para revisar el trabajo de Copilot cuando esté listo.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a una persona desarrolladora. Hay distintas maneras de asignarle trabajo, como [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de la asignación, Copilot crea una solicitud de cambios en borrador para hacer un seguimiento, configura un entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras trabaja o después. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud.

## La importancia de instrucciones bien acotadas

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las tareas aparentemente sencillas pueden ser complejas, así que [define cuidadosamente el alcance de las tareas para Copilot cloud agent][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado un ejercicio práctico sobre ellas, conoces `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo a Copilot cloud agent, inspecciona en modo de solo lectura los archivos de instrucciones que incluye el repositorio y observa su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub del repositorio o en un Codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes al agente la incidencia `Code lacks documentation`, comprueba si la solicitud de cambios incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones; volveremos a examinarlas al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo para Copilot cloud agent

Crear código suele requerir un entorno específico y scripts de configuración, independientemente de quién haga el trabajo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a una persona desarrolladora de software.

Cloud agent usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así se proporcionan las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para el laboratorio: instala Node.js y las dependencias del proyecto, los navegadores para pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación usa ahora Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Esto sirve para comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta mantener el código o incorporar a nuevas personas al equipo. Copilot destaca en la creación de documentación, por lo que esta es una incidencia ideal para asignarle. Trabajará en segundo plano y podrás revisar sus cambios en un ejercicio futuro.

1. Abre una pestaña nueva del navegador y visita el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios nueva, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está configurando el entorno mediante el flujo anterior, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR y comprueba que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende el estado del proyecto, se detiene a pensar y decidir el plan adecuado y crea código.

El proceso puede durar varios minutos. Uno de los objetivos principales de Copilot cloud agent es realizar tareas de forma asíncrona para que puedas ocuparte de otras cosas. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, nos centraremos en desarrollar funcionalidades para la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas de GitHub Copilot cloud agent es poder dividir el trabajo: tú puedes centrarte en un conjunto de tareas mientras el agente se ocupa de otro. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero ese tiempo se puede dedicar a otras tareas. Asignemos el trabajo al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después aparecerá el emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Copilot cloud agent funciona de forma parecida a una persona desarrolladora, así que no hace falta supervisarlo continuamente; revisa el resultado cuando haya terminado. Ahora veremos cómo crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignar incidencias a Copilot para que trabaje en ellas de forma asíncrona, por ejemplo, para resolver deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras cloud agent trabaja en segundo plano, puedes crear y usar agentes personalizados. [Copilot cloud agent también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que exploramos en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
