---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios de código y, si hace falta, proporcionemos comentarios a Copilot para mejorar el resultado.

### Escenario

Como hemos destacado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las sugerencias de GitHub Copilot para crear documentación, añadir los juegos relacionados y mejorar la accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent realiza tareas de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- Cloud agent se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Los flujos de GitHub Actions necesitan la aprobación de una persona antes de ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de cambios (PR) de GitHub Copilot cloud agent: añadir documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas un aviso de que el cortafuegos ha bloqueado a GitHub Copilot. **Es normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o desactivar el cortafuegos de Copilot cloud agent][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

Trabajar con Copilot en una PR no es una comunicación unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otras personas del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendar un texto único. Por ejemplo, podrías pedirle que:

- añada al principio de cada archivo de código encabezados de comentario con una breve descripción de su contenido.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot** como harías con cualquier otra persona. Usa una de las ideas anteriores o pide otra mejora de documentación que quieras ver en el código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás un commit nuevo en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot ha creado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar con Copilot cloud agent para pedir cambios; por ejemplo, puedes modificar el número de juegos que se muestran o pedir que añada encabezados de comentario y comentarios TSDoc (recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que solicitaste desde el panel de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre los modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Confirma que funcionan y que la preferencia se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas. Si detectas problemas de accesibilidad o posibles mejoras, menciona a **@copilot** en un comentario y solicita los cambios, igual que en las PR anteriores.

## Ejercicio opcional: sigue delegando

Cloud agent resulta especialmente útil cuando puedes asignarle tareas reales del backlog y ocuparte de otra cosa. Para adquirir esa costumbre, crea varias incidencias en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para indicar interés en un juego en la página de detalles.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido de cloud agent. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para observar su efecto en el trabajo de cloud agent.
- **Has asignado incidencias a Copilot cloud agent** y observado cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para accesibilidad, con controles de modo de alto contraste y modo claro.
- **Has usado la página de agentes de Copilot como panel de control** para supervisar y guiar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de cambios de cloud agent**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido de cloud agent. Para continuar, puedes explorar otros recorridos que complementan lo que has aprendido:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con los modos Plan y Autopilot, skills de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna a cloud agent una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el cortafuegos de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
