# Documento de Requisitos del Sistema

## 1. Requisitos Funcionales

### Gabriel
- Login de usuario, administrador y superadministrador.
- Creación de clientes.
- El superadministrador proporciona permisos de administrador a *x* trabajadores.
- El administrador le da permisos al usuario para modificar, crear y gestionar clientes.
- Recibir información de facturas pendientes de forma automática (a los 3 días y a los 5 días).
- Recibir información de facturas que ya se han pagado y almacenarlas.
- El superadministrador debe ser capaz de modificar la información, formato o variables de las fichas de factura en cualquier momento.

### Hugo
- Cuando el cliente se da de baja, notificar los impagos y después, guardar sus datos durante el máximo tiempo permitido (borrado lógico).
- Las notificaciones se deben mandar por email.
- Funcionar desde el móvil.
- Dos roles: administrador y lector.

### Rubén
- La aplicación debe ser interna e independiente de Brain, los clientes no acceden ni se registran.
- El pago tiene que ser realizado completo, no fraccionado.
- Cuando el cliente solicita la baja, detener las facturas recurrentes pero seguir recordando los pagos pendientes.
- El superadministrador tiene que tener una configuración avanzada que le permita, por ejemplo, cambiar el plazo de pago.
- Se deben implementar filtros para las facturas (ubicación, temática, clientes, fecha de factura, etc.).
- Generar una factura automática, guardarla y enviarla al cliente.

### Aquiles
- El sistema debe permitir la creación de facturas.
- El sistema debe funcionar desde un dispositivo móvil.
- El sistema debe permitir que el administrador registre a un usuario.
- El sistema debe crear recordatorios de pagos.
- El sistema debe crear recordatorios de impagos.
- El sistema deberá realizar informes.

---

## 2. Requisitos No Funcionales

### Gabriel
- Facturas creadas en menos de 1 minuto.
- La web/aplicación debe ser lo suficientemente sencilla e intuitiva como para hacer una ficha de cliente desde cualquier dispositivo.
- Las fichas que se guardan en el sistema son las que no están activas, es decir, las facturas ya pagadas.
- El sistema debe soportar el login de todos los usuarios, administradores y del superadministrador.

### Hugo
- Privacidad de los datos.
- Facturar en menos de 1 minuto.

### Rubén
- Se puede utilizar desde cualquier dispositivo.
- Capacidad de escalar a miles de clientes.
- No requiere doble factor de autorización.
- Se prioriza la eficiencia.
- La aplicación no tiene nada que ver con Brain, están completamente separadas.

### Aquiles
- El proceso de facturar debe durar menos de un minuto.
- Un usuario no podrá darse de baja si tiene pagos pendientes.
- Los informes serán mensuales, trimestrales o semanales.
- Las notificaciones de impagos son perpetuas y solo paran al ser pagadas.
- Se debe verificar que el email del registro sea del dominio de TurbineH.
- Solo el administrador puede registrar a un usuario.
- Debe haber al menos un superadministrador en el sistema.

---

## 3. Requisitos de Negocio

### Gabriel
- Facilitar el seguimiento y creación de facturas de nuestros clientes.
- Controlar la información a la que tiene acceso cada usuario.

### Hugo
- Facturar en pocos clics, incluso desde el teléfono móvil.
- Capacidad de emitir factura puntual o recurrente, para clientes nuevos o existentes.
- Recordar los pagos pendientes.
- Confirmar al cliente las facturas cobradas.

### Rubén
- Los pagos dependen de la facturación del cliente y son de carácter mensual.

---

## 4. Requisitos de Usuario y Resumen General

- Que el usuario pueda registrarse y gestionar facturas.
- **Visión General:** Gestión rápida de facturas donde los clientes pueden chequearlas. Diseño minimalista e íntegro. Uso de roles, facturas recurrentes; los clientes se dan de baja sin impagos y pueden cambiar de plan.

### Notas adicionales (Hugo)

- **Flujo principal:** Registro del cliente -> Facturación -> Seguimiento de los cobros.
- **Dashboard:** Con información general, solo visible para el administrador.
- **Validación:** Verificar que el email pertenezca a la empresa (`<nombre>@turbine.<dominio>`).
- **Filtrado:** Añadir filtrado por empresa, temática, tipo de factura, estado y fecha.
- **Diseño:** Respetar la imagen corporativa de la empresa.
- **Plataforma:** Aplicación web adaptada a todos los dispositivos (Responsive).
- **Plazos:** Plazo de pago establecido en 5 días.
- **Permisos:** El rol "lector" puede hacer lo mismo que el administrador, pero solo en aquello a lo que se le dé permiso expreso.
- **Retención:** Mientras haya impagos, el cliente no se puede dar de baja.