# Historias de usuario — FlowSync MVP

**Fuente**: `docs/prd.md` (v1.0 · Q3 2026). **Alcance**: solo MVP. No incluye features fuera de alcance (equipos, calendarios no-Google, push/email, app nativa, etiquetas/proyectos/subtareas). **Convenciones**: rol único del MVP \= *usuario* (producto estrictamente individual). Las ambigüedades del PRD se marcan como `[AMBIGÜEDAD: …]` y deben resolverse en refinamiento, no rellenarse.

---

## Épica 1 — Autenticación y gestión de cuenta

### HU-1.1 — Registro de cuenta

**Como** visitante, **quiero** crear una cuenta con email y contraseña, **para** tener un espacio propio donde gestionar mis tareas.

- Dado un visitante en la pantalla de registro, Cuando introduce un email no registrado y una contraseña de al menos 8 caracteres, Entonces la cuenta se crea y queda autenticado.  
- Dado un visitante en la pantalla de registro, Cuando introduce una contraseña de menos de 8 caracteres, Entonces el sistema rechaza el registro y muestra un mensaje comprensible indicando el mínimo de caracteres.  
- Dado un visitante, Cuando introduce un email con formato inválido, Entonces el sistema muestra un error de validación claro, no un error técnico.

### HU-1.2 — Email ya registrado

**Como** visitante, **quiero** ser avisado si mi email ya tiene cuenta, **para** ir a iniciar sesión sin crear un duplicado.

- Dado un visitante en el registro, Cuando introduce un email que ya existe, Entonces el sistema lo indica y ofrece ir al inicio de sesión.

### HU-1.3 — Inicio de sesión

**Como** usuario registrado, **quiero** iniciar sesión con mi email y contraseña, **para** acceder a mis tareas.

- Dado un usuario registrado, Cuando introduce credenciales correctas, Entonces accede a su cuenta y se mantiene la sesión mediante token de acceso.  
- Dado un usuario registrado, Cuando introduce credenciales incorrectas, Entonces el acceso se rechaza con un mensaje comprensible.

### HU-1.4 — Cierre de sesión

**Como** usuario autenticado, **quiero** cerrar sesión, **para** proteger mi cuenta en un dispositivo compartido.

- Dado un usuario autenticado, Cuando cierra sesión, Entonces su token deja de ser válido y vuelve a la pantalla de inicio de sesión.

### HU-1.5 — Onboarding mínimo tras registro

**Como** usuario recién registrado, **quiero** una pantalla de bienvenida que explique qué hace FlowSync, **para** entender el valor y crear mi primera tarea sin fricción.

- Dado un registro exitoso, Cuando el usuario llega por primera vez, Entonces ve una pantalla de bienvenida que explica en una frase qué hace FlowSync e invita a crear la primera tarea.

### HU-1.6 — Aislamiento de datos entre cuentas

**Como** usuario, **quiero** que mis datos no sean visibles para otros usuarios, **para** mantener mi información privada.

- Dado un usuario autenticado, Cuando consulta tareas o datos, Entonces solo accede a los suyos y nunca a los de otra cuenta.  
- Dado un usuario, Cuando se almacenan sus tokens de Google, Entonces se guardan de forma segura y no son accesibles por otros usuarios.

---

## Épica 2 — Gestión de tareas (CRUD)

### HU-2.1 — Crear tarea

**Como** usuario, **quiero** crear una tarea indicando al menos un título, **para** capturar un pendiente rápidamente.

- Dado un usuario autenticado, Cuando crea una tarea con título y opcionalmente descripción y fecha límite, Entonces la tarea se guarda con estado `pending`.  
- Dado un usuario, Cuando intenta crear una tarea sin título, Entonces el sistema lo impide y muestra que el título es obligatorio.

### HU-2.2 — Ver listado de tareas

**Como** usuario, **quiero** ver el listado de mis tareas, **para** saber qué tengo pendiente.

- Dado un usuario con tareas, Cuando abre el listado, Entonces ve sus tareas con título, estado y fecha límite cuando exista.  
- Dado un usuario con hasta 200 tareas, Cuando abre el listado, Entonces este carga en menos de 1 segundo.

### HU-2.3 — Editar tarea

**Como** usuario, **quiero** editar cualquier campo de una tarea existente, **para** mantenerla actualizada.

- Dado un usuario con una tarea, Cuando modifica título, descripción, fecha límite o estado, Entonces los cambios se guardan y se reflejan en el listado.

### HU-2.4 — Borrar tarea

**Como** usuario, **quiero** borrar una tarea, **para** eliminar lo que ya no me sirve.

- Dado un usuario con una tarea, Cuando la borra, Entonces deja de aparecer en su listado.

### HU-2.5 — Cambiar estado de una tarea

**Como** usuario, **quiero** cambiar el estado de una tarea entre `pending`, `completed` y `archived`, **para** reflejar su progreso.

- Dado un usuario con una tarea en `pending`, Cuando la marca como `completed`, Entonces su estado cambia a `completed`.  
- Dado un usuario con una tarea, Cuando la archiva, Entonces su estado cambia a `archived`.

---

## Épica 3 — Organización y filtrado

### HU-3.1 — Filtrar tareas por estado

**Como** usuario, **quiero** filtrar mis tareas por estado, **para** centrarme solo en lo que me interesa (por ejemplo, solo pendientes).

- Dado un usuario con tareas en distintos estados, Cuando aplica un filtro por estado, Entonces el listado muestra únicamente las tareas de ese estado.

### HU-3.2 — Orden por relevancia para "hoy"

**Como** usuario, **quiero** que las tareas más relevantes para hoy se vean primero, **para** identificar de un vistazo qué debo hacer.

- Dado un usuario con varias tareas, Cuando abre el listado por defecto, Entonces las tareas se ordenan priorizando las más relevantes para "hoy".  
- `[AMBIGÜEDAD: el criterio exacto de ordenación queda a decisión del equipo durante el refinamiento (PRD §3.3).]`

### HU-3.3 — Estado vacío

**Como** usuario sin tareas, **quiero** ver una invitación a crear la primera, **para** saber cómo empezar.

- Dado un usuario sin tareas (cuenta nueva o todas archivadas), Cuando abre el listado, Entonces ve un estado vacío que invita a crear la primera tarea.

---

## Épica 4 — Exportación

### HU-4.1 — Exportar tareas a CSV

**Como** usuario, **quiero** exportar mis tareas a un archivo CSV, **para** llevarme mis datos.

- Dado un usuario con tareas, Cuando exporta a CSV, Entonces se genera un archivo que incluye, como mínimo, título, descripción, estado y fecha límite de cada tarea.

---

## Épica 5 — Sincronización con Google Calendar

Funcionalidad diferenciadora y de mayor riesgo técnico del MVP. Dirección primaria: **FlowSync → Google Calendar**.

### HU-5.1 — Conectar cuenta de Google

**Como** usuario, **quiero** conectar mi cuenta de Google a FlowSync, **para** que mis tareas se reflejen en mi calendario.

- Dado un usuario autenticado, Cuando autoriza el acceso vía OAuth con Google, Entonces FlowSync queda conectado a su cuenta de Google con permiso de lectura y escritura en el calendario.

### HU-5.2 — Tarea con fecha límite genera evento

**Como** usuario, **quiero** que mis tareas con fecha límite aparezcan como eventos en mi Google Calendar, **para** no copiarlas a mano.

- Dado un usuario con Google conectado, Cuando crea o tiene una tarea con fecha límite, Entonces aparece un evento correspondiente en su Google Calendar.  
- `[AMBIGÜEDAD: relación entre la fecha límite de la tarea y la hora/zona horaria del evento; debe definirse para no crear eventos a horas erróneas (PRD §4 Riesgos / §7).]`

### HU-5.3 — Cambio de fecha actualiza el evento

**Como** usuario, **quiero** que al cambiar la fecha de una tarea se actualice su evento, **para** mantener el calendario alineado sin esfuerzo.

- Dado un usuario con Google conectado y una tarea ya sincronizada, Cuando cambia la fecha de la tarea en FlowSync, Entonces el evento correspondiente en Google Calendar se actualiza.

### HU-5.4 — Completar o borrar tarea refleja en el evento

**Como** usuario, **quiero** que al completar o borrar una tarea su evento se actualice, **para** que el calendario no muestre pendientes que ya resolví.

- Dado un usuario con Google conectado y una tarea sincronizada, Cuando completa o borra la tarea en FlowSync, Entonces el evento correspondiente se elimina o se marca según corresponda.  
- `[AMBIGÜEDAD: el PRD dice "se elimina o se marca según corresponda"; falta definir el comportamiento exacto para completar vs. borrar (PRD §3.5).]`

### HU-5.5 — Desconectar Google sin perder tareas

**Como** usuario, **quiero** desconectar mi cuenta de Google cuando quiera, **para** dejar de sincronizar conservando mis tareas.

- Dado un usuario con Google conectado, Cuando desconecta su cuenta, Entonces FlowSync deja de sincronizar pero conserva las tareas ya creadas.

### HU-5.6 — Tolerancia a fallos de la API de Google

**Como** usuario, **quiero** que mis tareas se guarden aunque la sincronización falle, **para** no perder información cuando Google no esté disponible.

- Dado un usuario con Google conectado, Cuando la API de Google no está disponible o devuelve error al sincronizar, Entonces la tarea se guarda igualmente en FlowSync y la sincronización se reintenta más tarde.  
- Dado un fallo de sincronización, Cuando ocurre, Entonces queda registrado en logs para diagnóstico.  
- `[AMBIGÜEDAD: política de reintentos (frecuencia, número máximo) no especificada en el PRD (§3.5 / §4).]`

### HU-5.7 — Sincronización inversa (Google → FlowSync)

**Como** usuario, **quiero** que editar un evento en Google se refleje en la tarea, **para** una alineación bidireccional completa.

- `[AMBIGÜEDAD: el PRD marca la sincronización inversa como deseable pero NO comprometida para el MVP; su alcance requiere un spike técnico antes de decomponerse (PRD §3.5 Nota / §7). No estimar ni implementar hasta validar.]`

## Notas de trazabilidad

- Todas las historias derivan de los requisitos funcionales del PRD (§3.1–§3.5). Los requisitos no funcionales (§4) se incorporan como criterios de aceptación donde aplican (rendimiento en HU-2.2, privacidad en HU-1.6, errores claros en HU-1.1, observabilidad en HU-5.6).  
- No se incluyen historias para elementos fuera de alcance (PRD §1) ni se proponen arquitectura, estimaciones ni features no presentes en el PRD.

