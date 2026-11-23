# Práctica: Préstamo de equipo en Odoo

- **Estudiante:** David Alejandro Cardona Sanchez
- **Número de cuenta:** 0801200304464

## Respuestas

### Paso 1. Catálogo de equipos

**1. ¿Qué pasa con un equipo cuando se desmarca el campo Activo? ¿Por qué es mejor que borrarlo?**

Se archiva, por lo que deja de aparecer en las listas habituales, pero sigue guardado en la base de datos. Es mejor que borrarlo porque conserva el historial de préstamos y se puede reactivar.

**2. ¿Para qué sirve copy=False en el campo codigo?**

Evita que el código se copie al duplicar un equipo, para que cada equipo tenga un código único.

### Paso 2. Registro de préstamos

**1. Este modelo usa _name y _inherit al mismo tiempo. ¿Qué hace cada uno aquí?**

_name crea un modelo nuevo llamado prestamo.prestamo, mientras que _inherit le agrega las funcionalidades de mail.thread, como el chatter y el seguimiento de cambios.

**2. ¿Qué hace tracking=True en el campo state?**

Registra automáticamente en el chatter los cambios de estado, indicando quién los hizo y cuándo.

### Paso 3. Equipos del préstamo

**1. En fields.One2many("prestamo.linea", "prestamo_id"), ¿qué indica cada parámetro?**

El primero indica el modelo relacionado (prestamo.linea) y el segundo el campo Many2one que conecta las líneas con el préstamo (prestamo_id).

**2. ¿Qué pasa con las líneas cuando se borra un préstamo? ¿Qué parte del código lo define?**

Las líneas se eliminan automáticamente junto con el préstamo. Esto lo define ondelete="cascade" en el campo prestamo_id.

### Paso 4. Flujo de estados

**1. Los botones ya se ocultan según el estado. ¿Por qué los métodos vuelven a validar el estado?**

Porque ocultar botones solo afecta la interfaz, mientras que la validación en Python protege los datos desde el servidor y evita acciones inválidas. El método podría ejecutarse desde la API, desde otro código o desde otra pestaña del navegador que todavía muestra el botón.

**2. ¿Qué hace prestamo.linea_ids.equipo_id.write(...)? ¿Por qué no hace falta un for para cada equipo?**

Actualiza todos los equipos relacionados en una sola operación. Odoo trabaja con grupos de registros (recordsets), por lo que no hace falta recorrerlos individualmente.

### Paso 5. Reglas del préstamo

**1. El campo equipo_id ya filtra los equipos disponibles. ¿Por qué hace falta el TODO 5?**

Porque el domain filtra al seleccionar, pero el TODO 5 verifica que el equipo siga disponible justo al momento de prestar. Entre ambos momentos el equipo pudo pasar a mantenimiento o prestarse en otro préstamo.

**2. ¿En qué momento se ejecuta un método con @api.constrains? ¿Y la validación de action_prestar?**

@api.constrains se ejecuta al guardar o modificar los campos indicados, mientras que action_prestar valida cuando se pulsa el botón Prestar.

### Paso 6. Contacto, kanban y búsqueda

**1. ¿Qué pasa si arrastra una tarjeta del kanban a otra columna? ¿Por qué?**

El equipo cambia de estado al de la columna donde se suelta, porque el kanban está agrupado por el campo estado. Esto permite cambiar el estado sin pasar por las reglas del préstamo.

**2. El campo prestamo_ids se agregó a res.partner. ¿Se crea una columna nueva en la tabla de contactos?**

No. El One2many no crea una columna; utiliza el Many2one partner_id que ya existe en los préstamos para mostrar los registros relacionados.