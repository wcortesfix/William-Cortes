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

---

## Análisis complementario

Complemento a los criterios de aceptación. Identifica casos límite, supuestos implícitos, escenarios no cubiertos y dependencias/riesgos. No compromete features: lo marcado con `[SUPUESTO]` debe confirmarse y lo marcado con `[RIESGO]`/`[AMBIGÜEDAD]` resolverse en refinamiento. Nada aquí amplía el alcance del PRD sin validación.

### Épica 1 — Autenticación y gestión de cuenta

**Edge cases**

- Email con distinta capitalización (`Ana@x.com` vs `ana@x.com`): ¿se tratan como la misma cuenta?  
- Email o contraseña con espacios al inicio/final, caracteres Unicode o emojis; contraseña de exactamente 8 caracteres (límite).  
- Registro duplicado por envío doble o condición de carrera (dos peticiones con el mismo email casi simultáneas).  
- Login con credenciales correctas pero cuenta inexistente vs contraseña errónea (¿mismo mensaje genérico por seguridad?).  
- Acción con token expirado o ya invalidado tras logout; uso simultáneo en varios dispositivos.  
- Intentos repetidos de login fallido (fuerza bruta).

**Supuestos implícitos**

- `[SUPUESTO]` El email es el identificador único de cuenta y se normaliza (minúsculas) antes de comparar.  
- `[SUPUESTO]` Las contraseñas se almacenan con hashing seguro (el PRD no lo dice explícitamente; es un NFR de privacidad implícito).  
- `[SUPUESTO]` Los mensajes de error y la UI están en un único idioma; no hay i18n en el MVP.  
- `[SUPUESTO]` El token de acceso tiene una expiración definida y un mecanismo de renovación.

**Escenarios faltantes (no en el PRD)**

- Recuperación de contraseña: el PRD la **excluye** explícitamente (sin "recordar contraseña"); consecuencia: un usuario que olvide su contraseña pierde acceso. Conviene aceptarlo de forma consciente.  
- Cambio de contraseña, cambio de email, edición de perfil y **eliminación de cuenta** (relevante para privacidad/GDPR) no están contemplados.

**Dependencias / riesgos**

- `[RIESGO]` Sin límite de intentos de login (rate limiting) el MVP queda expuesto a fuerza bruta; el PRD no lo menciona.  
- `[RIESGO]` Sin verificación de email, pueden crearse cuentas con direcciones falsas (aceptado por el PRD, pero impacta métricas de adopción).  
- Dependencia de `@adonisjs/auth` para emisión/almacenamiento de tokens (§5).

### Épica 2 — Gestión de tareas (CRUD)

**Edge cases**

- Título solo con espacios en blanco (¿cuenta como vacío?), título y descripción muy largos (¿límite de longitud?).  
- Fecha límite en el pasado o con formato inválido; tarea con hora vs solo fecha.  
- Caracteres especiales/emoji en título y descripción.  
- Borrar o editar una tarea ya sincronizada con Google (cruza con Épica 5).  
- Edición concurrente de la misma tarea desde dos dispositivos.

**Supuestos implícitos**

- `[SUPUESTO]` "Borrar" es eliminación permanente, distinta de `archived` (estado lógico que conserva la tarea).  
- `[SUPUESTO]` La fecha límite es una fecha; si lleva hora asociada o se deriva una hora por defecto debe definirse (impacta directamente la hora del evento de calendario).  
- `[SUPUESTO]` No hay límite máximo de tareas por usuario en el MVP (el NFR de rendimiento se enuncia "hasta 200").

**Escenarios faltantes (no en el PRD)**

- Confirmación antes de borrar y/o deshacer (undo) un borrado.  
- Reactivar una tarea archivada (transición `archived` → `pending`).  
- Duplicar tareas, edición/borrado masivo (no en alcance, pero esperables por el usuario).

**Dependencias / riesgos**

- `[RIESGO]` La semántica borrar vs archivar condiciona el comportamiento del evento en Google Calendar (ver HU-5.4); deben definirse juntas.  
- `[AMBIGÜEDAD]` Si la fecha límite incluye o no hora afecta a toda la Épica 5 y al orden "hoy" de la Épica 3\.

### Épica 3 — Organización y filtrado

**Edge cases**

- Filtro que no devuelve resultados (¿estado vacío específico de filtro vs cuenta sin tareas?).  
- Orden con fechas límite iguales, nulas o ausentes; cómo se ubican las tareas sin fecha en el orden "hoy".  
- Persistencia del filtro/orden elegido al recargar o entre sesiones.  
- Listado con muchas tareas (paginación o scroll).

**Supuestos implícitos**

- `[SUPUESTO]` El filtro por estado es de un único estado a la vez (no combinaciones múltiples).  
- `[SUPUESTO]` "Hoy" se calcula según la zona horaria del usuario.

**Escenarios faltantes (no en el PRD)**

- Búsqueda por texto, filtros combinados, ordenación manual o persistencia de preferencias.

**Dependencias / riesgos**

- `[AMBIGÜEDAD]` El criterio de ordenación "para hoy" no está definido (HU-3.2) y bloquea pruebas deterministas del orden.  
- `[RIESGO]` "Hoy" depende de zona horaria, conectando este orden con el riesgo de TZ de la Épica 5\.

### Épica 4 — Exportación

**Edge cases**

- Exportar sin tareas: ¿CSV vacío, solo cabeceras, o aviso?  
- Valores con comas, comillas dobles o saltos de línea (deben escaparse para no romper el CSV).  
- Encoding (UTF-8/BOM) para que tildes y emojis se vean bien en Excel; formato/locale de fechas.  
- Exportar cientos de tareas (rendimiento y posible timeout).

**Supuestos implícitos**

- `[SUPUESTO]` La exportación incluye **todas** las tareas, no solo las filtradas/visibles.  
- `[SUPUESTO]` Formato de fecha, separador de columnas y encoding fijos y documentados.

**Escenarios faltantes (no en el PRD)**

- Exportar solo lo filtrado o seleccionado; incluir o no las `archived`; nombre del archivo; otros formatos (fuera de alcance).

**Dependencias / riesgos**

- `[RIESGO]` Inyección CSV: celdas que empiezan por `=`, `+`, `-` o `@` pueden ejecutarse como fórmulas al abrir en Excel; debe mitigarse.

### Épica 5 — Sincronización con Google Calendar

**Edge cases**

- Tarea creada **sin** fecha y a la que luego se le añade una (¿se crea evento entonces?); quitar la fecha de una tarea ya sincronizada (¿se borra el evento?).  
- Archivar una tarea sincronizada: ¿qué ocurre con su evento?  
- Token OAuth expirado o revocado desde el lado de Google; rate limits de la API.  
- Usuario edita o borra **manualmente** el evento en Google (cruza con HU-5.7, sync inversa).  
- Reconectar con una cuenta de Google distinta a la original.  
- Eventos de todo el día vs con hora; eventos recurrentes (fuera de alcance, pero la API los expone).  
- Reintento que tiene éxito tarde, cuando el usuario ya desconectó Google; doble sincronización que duplica eventos.

**Supuestos implícitos**

- `[SUPUESTO]` Relación 1:1 entre tarea y evento; FlowSync "posee" los eventos que crea y persiste el mapeo tarea↔evento.  
- `[SUPUESTO]` Los eventos se crean en el calendario primario del usuario (no se elige calendario destino en el MVP).  
- `[SUPUESTO]` La fecha límite se traduce a una hora/duración de evento concreta según una regla aún por definir.

**Escenarios faltantes (no en el PRD)**

- Añadir/quitar fecha a una tarea existente; archivar tarea y su evento; selección de calendario destino; manejo de eventos borrados o editados manualmente en Google; flujo de renovación (refresh) de tokens OAuth.

**Dependencias / riesgos**

- `[RIESGO]` El PRD recomienda un **spike técnico** antes de decomponer esta épica (§7): rate limits, zonas horarias y casos all-day/recurrentes pueden inflar el alcance.  
- `[RIESGO]` OAuth requiere configuración previa en Google Cloud Console y revisión de permisos (§7); setup más largo de lo esperado.  
- `[RIESGO]` Los criterios de éxito (≥40% conexión de Google, \<5% fallos no recuperables, §6) condicionan el diseño de fiabilidad y telemetría.  
- `[AMBIGÜEDAD]` Sync inversa (HU-5.7) no comprometida; política de reintentos (HU-5.6) y comportamiento completar/borrar (HU-5.4) sin definir.

### Transversal (aplica a varias épicas)

- `[RIESGO]` **Zonas horarias**: riesgo recurrente entre fecha límite, orden "hoy" y hora del evento; debe definirse una única política de TZ para todo el producto.  
- `[RIESGO]` **Concurrencia multi-dispositivo**: el mismo usuario operando en dos sesiones puede generar estados inconsistentes; no hay estrategia definida.  
- `[SUPUESTO]` **Responsive móvil** (NFR §4): cada épica debería verificarse también en navegador móvil, no solo escritorio.  
- `[SUPUESTO]` **Idioma único** y formatos de fecha/hora consistentes en toda la app.  
- `[RIESGO]` **Seguridad transversal**: almacenamiento seguro de tokens (§4), rate limiting de autenticación y saneado de exportación CSV no están detallados en el PRD.

---

## Notas de trazabilidad

- Todas las historias derivan de los requisitos funcionales del PRD (§3.1–§3.5). Los requisitos no funcionales (§4) se incorporan como criterios de aceptación donde aplican (rendimiento en HU-2.2, privacidad en HU-1.6, errores claros en HU-1.1, observabilidad en HU-5.6).  
- No se incluyen historias para elementos fuera de alcance (PRD §1) ni se proponen arquitectura, estimaciones ni features no presentes en el PRD.

