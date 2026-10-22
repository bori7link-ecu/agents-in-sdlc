---
title: "Lección 8: Crear y combinar la PR de la funcionalidad"
description: "Revisa los filtros, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revisarás todo en conjunto y abrirás una solicitud de incorporación de cambios. Combinaste manualmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una misma conversación y rama, y lo incluirás en esta PR.

En esta lección:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- Inspeccionarás la funcionalidad completa y las pruebas de verificación.
- Crearás la PR de filtrado.
- Habilitarás Agent Merge solo después de revisar el trabajo y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, sin quitar al desarrollador el control de la autorización de combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para completar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los obstáculos que impiden combinarla: corrige errores en las comprobaciones de integración continua (CI), responde a comentarios de revisión y actualiza la base si es necesario. A continuación, habilita la combinación automática de GitHub cuando el repositorio lo permite.

Hasta ahora, has seleccionado **Combinar solicitud de incorporación de cambios**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitarlo.

## Usa Agent Merge para gestionar la PR

Con todo el código creado, vamos a revisarlo en conjunto, crear la PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una nueva PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando se completen correctamente todas las comprobaciones, la combinará.
7. Confirma que ves un mensaje de Copilot similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si no puede continuar, lee el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el flujo de trabajo de las solicitudes de incorporación de cambios. En concreto:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- Inspeccionaste la funcionalidad completa y las pruebas de verificación.
- Creaste la PR de filtrado.
- Habilitaste Agent Merge solo después de revisar el trabajo y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir conversaciones y delegar tareas opcionalmente al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
