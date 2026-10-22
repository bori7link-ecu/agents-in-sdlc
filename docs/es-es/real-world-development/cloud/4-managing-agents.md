---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. El agente está diseñado para trabajar de forma autónoma y asíncrona, pero sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes con solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar una sesión en curso.

En esta lección:

- Explorarás la página de agentes para supervisar las tareas del agente en la nube.
- Orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Ya se está actualizando el estilo del sitio y añadiendo controles, así que parece lógico incluir esta función. Quieres orientar el trabajo del agente para que añada el modo claro junto al modo de alto contraste.

## Revisa las tareas del agente en la nube

Veamos el estado actual de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que asignaste a Copilot, como:
    - Actualizar la documentación del código.
    - Generar API para modificar juegos.
    - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones realizadas por Copilot. Pueden incluir:
    - Obtener el código del repositorio.
    - Crear el entorno de trabajo.
    - Configurar servidores MCP.
    - Realizar diferentes acciones para completar la tarea.

> [!NOTE]
> Los pasos que se muestran dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de la solicitud de incorporación de cambios (PR), que aparece a la derecha. Permite consultar la PR y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pedirás a Copilot que añada el control del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la creación del modo de alto contraste. El título exacto depende del nombre elegido por Copilot y del estado actual de la tarea.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, añade este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la página de agentes, con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma el mensaje y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo después de completar el paso que estaba realizando cuando enviaste el mensaje.

Esto tardará varios minutos, así que puedes hacer una pausa y repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente de Copilot en la nube. Desde aquí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para cambiar su alcance o proporcionar instrucciones.

Has explorado estos conceptos:

- Usar el panel de control y la página de agentes para supervisar las tareas del agente en la nube.
- Redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
