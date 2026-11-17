---
title: "Ejercicio 1 - Instrucciones personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Copilot cloud agent →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] permiten proporcionar esa orientación para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de código y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas para determinadas rutas.
- añadirás un estándar general nuevo a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts de antes y después. Copilot cloud agent trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influyen las instrucciones más adelante, al revisar las solicitudes de cambios que genera el agente.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Usarás los archivos de instrucciones para que Copilot tenga la información necesaria y pueda realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de código y los requisitos. Permiten orientar las sugerencias y los fragmentos de código, por ejemplo, indicando las convenciones, las bibliotecas o los tipos de comentarios que se prefieren. Puedes crear instrucciones para todo el repositorio o para determinados tipos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información sobre el proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se desarrolla, las prácticas recomendadas y otras pautas globales.
- `.github/instructions/*.instructions.md`, archivos que puedes crear para tareas o tipos de archivos específicos. Sirven para proporcionar pautas para lenguajes concretos, como TypeScript o Astro, o para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código con Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot [admite otros formatos de archivos de instrucciones][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Crear en detalle archivos de instrucciones queda fuera del ámbito del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en pautas del proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares globales de código.
- Usa archivos `*.instructions.md` para pautas de tipos de archivo —pruebas unitarias, componentes Astro o la capa de datos— o para tareas concretas.
- Escribe en lenguaje natural. Explica las pautas con claridad e incluye ejemplos de cómo debería ser el código y cómo no debería serlo.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para cada proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, verás pautas para numerosas tareas, como las [actualizaciones de interfaz][ui-instructions] y el código [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller del entorno que uses lo explicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas para **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que encaje con `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos pequeños y reutilizables, y enlazar ejemplos para que Copilot los siga al generar código. Las rutas de esos enlaces son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Añade un estándar para todo el repositorio

Ahora harás el único cambio de este ejercicio: añadir una regla para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube la usará más adelante al trabajar en las incidencias que le asignes.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero confirmar los cambios en su propia rama te permitirá mantenerlos ordenados:

1. En el terminal de tu Codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade un estándar de código para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de código del proyecto, pero aún no incluye una regla de documentación, por lo que el helper generado no tenía comentarios.
3. Añade estas líneas de Markdown justo después de los estándares existentes para pedir a Copilot que añada encabezados de comentario a los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para pautas globales o archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas específicas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Confirma, envía y fusiona la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot, que creará una rama a partir de `main`. Por eso, los cambios de instrucciones deben llegar a `main` para que el agente pueda usarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios de `custom-instructions-cloud` a `main` y fusiónala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios en `main`. El paso de la rama refleja cómo trabajarías en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot usa el contexto de los archivos de instrucciones del proyecto y has añadido un estándar general nuevo a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de cambios que genere el agente durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia a Copilot cloud agent][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: recopilación de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
