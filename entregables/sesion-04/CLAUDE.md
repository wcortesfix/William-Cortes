# FlowSync — Contexto de proyecto

## Qué es FlowSync
<!-- COMPLETAR: 2-4 bullets de alto nivel -->
- FlowSync es FlowSync es una aplicación web de gestión de tareas personales que **mantiene sincronizadas las tareas del usuario con su Google Calendar**. 
- Usuario objetivo:  profesional del conocimiento (knowledge worker) entre 25 y 45 años, que ya usa Google Calendar a diario para su trabajo y que actualmente gestiona sus pendientes en una herramienta separada (Todoist, Notion, una libreta, o post-its). Tiene entre 5 y 30 tareas activas en un momento dado. Valora la simplicidad y se frustra con herramientas que requieren mantenimiento manual.
- Estado actual: MVP en definición.

## Mi rol
- Product Owner Senior con experiencia en aplicaciones SSAS, responsable de escribir las historias de usuario.

## Documento fuente (PRD)
- El PRD vive en `docs/prd.md`.
- Para cualquier trabajo sobre historias del MVP, lee este PRD. No trabajes de memoria ni infieras features.

## Reglas para historias de usuario (siempre)
Estas reglas aplican cada vez que se escriben historias en este proyecto:

1. **Formato de cada historia**, literal:
   `Como [rol], quiero [acción], para [beneficio].`

2. **Criterios de aceptación**: en Gherkin:
   `Dado [contexto], Cuando [acción], Entonces [resultado verificable].`
   Deben ser específicos y comprobables.

3. **Alcance**: nunca inventar funcionalidades que no estén en el PRD, no estimar tiempos, no proponer arquitectura. Si algo es ambiguo o falta información, márcalo como `[AMBIGÜEDAD: descripción]` en lugar de rellenarlo.
