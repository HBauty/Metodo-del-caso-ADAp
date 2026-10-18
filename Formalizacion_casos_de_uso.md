## CU-00: Registrarse en el sistema (Alta de empleado)

**Actor principal:** Empleado.

**Descripción:** Un nuevo empleado interno genera su perfil de usuario mediante la dirección corporativa para utilizar las funciones de la plataforma.

**Precondiciones:** El trabajador dispone de una cuenta de correo corporativo activa (@turbine...) sin registro previo en la plataforma.

**Garantía de éxito:** Se da de alta la cuenta del usuario, se guarda el registro en la auditoría y permanece a la espera de que la administración le asigne permisos y carteras.

### Escenario principal de éxito:

1. El usuario solicita la creación de un nuevo perfil en el sistema.
2. El usuario ingresa su nombre completo, email de la empresa y define su clave de acceso.
3. El sistema comprueba que la dirección corresponde al dominio institucional (@turbine...) y que no existe de antemano.
4. El sistema habilita el registro en estado pendiente de asignación de roles y notifica a la administración.
5. El sistema confirma la realización del proceso y guarda el evento en el log de auditoría.

Extensiones:
- **3a.** Correo no perteneciente a Turbine: El sistema deniega el procedimiento e indica que solo se admiten cuentas corporativas oficiales.
- **3b.** Dirección de correo ya registrada: El sistema detecta el registro previo y redirige al usuario a la pantalla de inicio de sesión (CU-01).

---

## CU-01: Iniciar sesión (Log-in)

**Actor principal:** Usuario.

**Descripción:** El empleado se autentica mediante correo corporativo y contraseña para acceder a las funciones e información permitidas por su rol.

**Precondiciones:** El empleado tiene una cuenta activa previamente registrada en el sistema.

**Garantía de éxito:** Se inicia la sesión y se carga el entorno restringido a sus permisos y clientes autorizados.

### Escenario principal de éxito:
1. El actor introduce su correo electrónico y contraseña.
2. El sistema verifica que el correo pertenece al dominio corporativo de Turbine (@turbine...).
3. El sistema valida las credenciales e identifica el rol del usuario (Superadministrador, Administrador o Lector).
4. El sistema concede el acceso mostrando únicamente los módulos y clientes asignados a su perfil.

### Extensiones:

- **2a.** Correo externo o personal: El sistema bloquea el acceso e informa que el uso es exclusivo para cuentas corporativas de Turbine.
- **3a.** Credenciales incorrectas o cuenta inactiva: El sistema deniega el acceso, registra el intento fallido en el log de auditoría y solicita reintentar.

---

## CU-02: Asignar roles y permisos

**Actor principal:** Superadministrador / Administrador.

**Descripción:** El actor registra empleados, define su rol (Administrador o Lector) y les asigna la cartera de clientes sobre la que pueden operar.

**Precondiciones:** El actor ha iniciado sesión con permisos de administración.

**Garantía de éxito:** Los permisos del empleado se actualizan y la acción queda registrada en el log de auditoría.

###Escenario principal de éxito:

1. El actor selecciona o registra un empleado con correo corporativo.
2. El actor asigna el rol correspondiente (Administrador si es SuperAdmin; Lector si es Admin).
3. El actor selecciona los clientes específicos asignados a ese usuario.
4. El sistema valida los privilegios, guarda la configuración y registra el cambio en el log inmutable.

### Extensiones:

- **1a.** Correo no perteneciente a Turbine: El sistema rechaza el alta e informa del error.

---

## CU-03: Pausar suscripción

**Actor principal:** Administrador / Lector asignado.

**Descripción:** El usuario detiene temporalmente la facturación recurrente de un cliente activo sin cancelar su contrato.

**Precondiciones:** El cliente tiene una suscripción activa en el sistema.

**Garantía de éxito:** Se bloquea la emisión de nuevas facturas recurrentes hasta la fecha de reactivación, manteniendo activas las alertas de deudas previas.

### Escenario principal de éxito:

1. El actor selecciona la suscripción activa del cliente y solicita pausarla.
2. El sistema solicita el motivo, la fecha de inicio y la fecha prevista de reactivación.
3. El actor introduce los datos requeridos.
4. El sistema cambia el estado de la suscripción a "Pausada" y detiene la programación de próximas facturas mensuales.

### Extensiones:

- **3a.** Faltan datos obligatorios (motivo o fechas): El sistema impide continuar y solicita completar la información.

---

## CU-04: Tramitar baja de cliente

**Actor principal:** Lector asignado.

**Descripción:** El usuario registra la solicitud de baja de un cliente, deteniendo su suscripción y aplicando el borrado lógico tras liquidar deudas.

**Precondiciones:** El cliente existe y está activo en el sistema.

**Garantía de éxito:** Se detienen las facturas recurrentes y los datos del cliente pasan a estado bloqueado (borrado lógico) por el tiempo legal permitido.

### Escenario principal de éxito:

1. El actor solicita dar de baja a un cliente.
2. El sistema detiene inmediatamente la generación de futuras facturas recurrentes.
3. El sistema verifica que el cliente no tiene facturas pendientes de pago ni vencidas.
4. El sistema aplica el borrado lógico al cliente, conservando su histórico bloqueado por motivos legales.

### Extensiones:

- **3a.** El cliente tiene impagos pendientes: El sistema bloquea la baja definitiva, notifica automáticamente por email al cliente sobre sus deudas pendientes y mantiene el flujo de reclamación hasta el cobro total.

---

## CU-05: Notificar avisos e impagos 

**Actor principal:** Lector asignado

**Descripción:** El sistema envía recordatorios de pago o avisos a los clientes.

**Precondiciones:** Existen facturas emitidas pendientes de pago o vencidas en el sistema.

**Garantía de éxito:** Las comunicaciones se envían en el idioma del cliente y quedan registradas en el historial.

### Escenario principal de éxito:

1. El sistema revisa diariamente los vencimientos de las facturas no pagadas.
2. El sistema genera el mensaje adaptando el idioma y el tono (amistoso en días 3 y 5; formal/urgente cada 2-3 días tras vencer), incluyendo datos de soporte.
3. El sistema envía la notificación al contacto de facturación/cobros del cliente y avisa al empleado interno asignado.
4. El sistema registra el resultado del envío.

### Extensiones:

- **1a.** Factura marcada "En Disputa": El sistema omite el envío de recordatorios hasta la fecha máxima de revisión.
- **3a.** Fallo de entrega (rebote de correo): El sistema no marca la factura como notificada, crea una incidencia automática y alerta al responsable para corregir el contacto.

---

## CU-06: Consultar y gestionar fichas de clientes e historial

**Actor principal:** Administrador / Lector asignado.

**Descripción:** El actor busca, filtra, consulta y edita la información fiscal, contactos múltiples e historial de facturación de los clientes.

**Precondiciones:** El actor ha iniciado sesión.

**Garantía de éxito:** Se muestran o actualizan los datos respetando los permisos de visibilidad del rol y el control de concurrencia.

### Escenario principal de éxito:

1. El actor aplica filtros de búsqueda (por nombre, ubicación, sector o estado/fecha de factura).
2. El sistema muestra las fichas e historiales únicamente de los clientes autorizados para ese rol.
3. El actor selecciona un cliente y modifica sus datos fiscales o gestiona sus contactos asociados (añadir o marcar como "Inactivo").
4. El sistema valida los datos, guarda los cambios y registra la modificación en el log de auditoría.

### Extensiones:

- **4a.** Conflicto de concurrencia (dos usuarios editando a la vez): El sistema bloquea el sobrescrito accidental y avisa al usuario para recargar los datos actuales.

---

## CU-07: Registrar ficha de suscripción (Acuerdo Comercial)

**Actor principal:** Administrador / Lector asignado.

**Descripción:** El actor registra las condiciones comerciales y el plan contratado por un cliente para programar su facturación.

**Precondiciones:** El cliente ya está registrado en el sistema con sus datos fiscales completos.

**Garantía de éxito:** Queda creada la ficha de suscripción y programada la facturación automática.

### Escenario principal de éxito:

1. El actor selecciona un cliente y solicita crear un nuevo acuerdo comercial.
2. El actor introduce la fecha de inicio, selecciona el plan/tarifa según el rango de facturación anual, asigna el comercial responsable y las condiciones de pago.
3. El actor adjunta el documento de contrato firmado externamente.
4. El sistema guarda la ficha y programa la emisión automática (prorrateando los días restantes si el alta no es el día 1).

### Extensiones:

- **2a.** Condiciones de pago o descuentos fuera de política: El sistema retiene la activación y envía el acuerdo al flujo de aprobación de un Administrador/SuperAdmin.

---

## CU-08: Emitir orden de cobro / Envío de factura automática

**Actor principal:** Lector asignado

**Descripción:** El sistema ejecuta el ciclo de facturación programado (día 1 de cada mes o día de alta) generando y enviando el documento de cobro al cliente.

**Precondiciones:** El cliente tiene una suscripción activa.

**Garantía de éxito:** La factura se emite, se almacena el PDF íntegro y se envía al cliente en menos de 1 minuto.

### Escenario principal de éxito:

1. Al llegar la fecha programada, el sistema calcula el importe exacto (cuota mensual o prorrateo inicial) más impuestos.
2. El sistema genera el PDF estándar con numeración correlativa, estética corporativa e idioma configurado.
3. El sistema almacena el PDF y lo envía por correo electrónico al cliente.
4. El sistema cambia el estado de la factura a "Enviada" e inicia el contador de 5 días para el vencimiento.

### Extensiones:

- **3a.** Caída del servidor durante facturación masiva: Al reiniciarse, el sistema reanuda la cola desde el punto de fallo garantizando cero facturas duplicadas.

---

## CU-09: Verificar y conciliar cobros

**Actor principal:** Administrador / Lector asignado.

**Descripción:** El sistema consulta fuentes externas para confirmar pagos y liquidar facturas pendientes.

**Precondiciones:** Existen facturas en estado "Enviada" o "Vencida".

**Garantía de éxito:** El estado de la factura se actualiza, se detienen los recordatorios y se notifica el cobro.

### Escenario principal de éxito:

1. El sistema consulta periódicamente los movimientos en las pasarelas y cuentas bancarias conectadas.
2. El sistema detecta un ingreso, lo asocia a una factura específica y verifica que el importe coincide al céntimo (pago único completo).
3. El sistema marca la factura como "Pagada" y detiene automáticamente el flujo de recordatorios de esa factura.
4. El sistema envía confirmación de cobro al cliente y notifica al empleado responsable en Turbine.

### Extensiones:

- **2a.** Discrepancia en el importe (pago parcial o de más): El sistema no liquida la factura automáticamente, la marca como "Parcialmente Conciliada" y alerta al Administrador para supervisión manual.

---

## CU-10: Generar factura manual (Puntual)

**Actor principal:** Administrador / Lector asignado.

**Descripción:** El empleado crea manualmente una factura puntual para servicios adicionales o configuraciones especiales.

**Precondiciones:** El cliente está dado de alta en el sistema.

**Garantía de éxito:** La factura se crea, valida y queda lista para su emisión y envío en PDF.

### Escenario principal de éxito:

1. El actor selecciona al cliente y solicita crear una factura manual.
2. El actor introduce el concepto, importe exacto y fecha de vencimiento.
3. El sistema verifica que los parámetros están dentro de la política estándar.
4. El sistema genera el PDF correlativo, lo guarda en la base de datos y lo envía al cliente.

### Extensiones:

- **3a.** Importe inusual, descuento extraordinario o vencimiento fuera de política: El sistema retiene la factura en estado "Pendiente de aprobación", bloquea el envío y solicita validación a un Administrador/SuperAdmin.

---

## CU-11: Modificar factura en borrador

> (Nota: Por el requisito NFR12, está prohibido editar facturas ya emitidas; solo se editan borradores o se aprueban excepciones).

**Actor principal:** Administrador / Superadministrador.

**Descripción:** El actor edita los datos de una factura no emitida o aprueba/rechaza una factura retenida por excepción financiera.

**Precondiciones:** La factura se encuentra en estado "Borrador" o "Pendiente de Aprobación".

**Garantía de éxito:** La factura se actualiza o pasa a estado "Aprobada/Emitida", registrando la auditoría.

### Escenario principal de éxito:

1. El actor selecciona una factura en estado Borrador o Pendiente de aprobación.
2. El actor modifica los importes/condiciones o valida la excepción financiera introduciendo una justificación.
3. El sistema guarda los cambios, registra la resolución en el log de auditoría y procede a emitir la factura.

### Extensiones:

- **1a.** Intento de editar una factura ya emitida: El sistema bloquea la acción e indica que debe generarse una factura rectificativa (CU-12).

## CU-12. Emitir y enviar una factura o factura rectificativa

**Actor principal:** Administrador o Lector con permiso para facturar.  

**Descripción:** Generar una factura, guardar su PDF y enviarla al cliente. Incluye facturas puntuales, recurrentes y rectificativas.  

**Precondiciones:** El usuario está autenticado y autorizado; existe la ficha del cliente. Si es una rectificativa, debe existir la factura original.

**Garantía de éxito:** La factura y su PDF quedan guardados y asociados al cliente; se realiza el envío y se registra su resultado.

### Escenario principal de éxito

1. El usuario selecciona un cliente e inicia una factura.
2. El sistema prepara el borrador con los datos fiscales y los importes.
3. El sistema valida los datos y muestra una vista previa del PDF.
4. El sistema comprueba si hay importes anómalos o condiciones que requieran aprobación.
5. El usuario confirma la emisión cuando la factura está autorizada.
6. El sistema emite la factura y guarda su PDF.
7. El sistema envía la factura al cliente y registra el resultado.

### Extensiones / Flujos alternativos

- **1a. Factura rectificativa:** Se selecciona la factura original y se crea una rectificativa vinculada a ella.
- **1b. Facturación recurrente:** El sistema genera y envía automáticamente las cuotas programadas. Prorratea la primera cuota hasta final de mes y emite las siguientes el día 1. Procesa los lotes en segundo plano.
- **3a. Datos fiscales incompletos:** Se bloquea la emisión y se indican los errores.
- **4a. Se requiere aprobación:** La factura permanece en borrador hasta que se apruebe mediante CU-14.
- **7a. Falla el envío:** La factura se conserva y el sistema abre una incidencia para CU-17.

---

## CU-13. Actualizar la tarifa según los cobros anuales

**Actor principal:** Administrador.  

**Descripción:** Revisar anualmente la tarifa de una empresa cliente según el dinero que ha cobrado, de acuerdo con su plan. El sistema propone una tarifa y el administrador decide si aplicarla.  

**Precondiciones:** El administrador está autenticado; el cliente tiene fecha de alta y una tarifa registrada.

**Disparador:** El sistema avisa cuando se cumple un año desde el alta del cliente.

**Garantía de éxito:** La decisión queda registrada y, si se aprueba el cambio, la nueva tarifa se aplica a pagos futuros.

**Garantía mínima:** No se cambia la tarifa sin aprobación ni se modifican facturas ya emitidas.

### Escenario principal de éxito

1. El administrador abre la revisión anual del cliente.
2. El sistema consulta los datos de cobros disponibles y propone una tarifa según el plan.
3. El administrador revisa y acepta la propuesta.
4. El sistema actualiza la tarifa para pagos futuros y registra el cambio.
5. El sistema confirma la actualización.

### Extensiones / Flujos alternativos

- **2a. No se pueden consultar los datos:** Se informa del error y se mantiene la tarifa anterior.
- **3a. El administrador rechaza la propuesta:** Se registra la decisión sin modificar la tarifa.

---

## CU-14. Aprobar una excepción financiera

**Nombre:** Aprobar una excepción financiera  
**Descripción:** Autorizar operaciones excepcionales, como importes inusuales, descuentos extraordinarios, cambios manuales de precio o vencimientos fuera de la política habitual.  
**Actor principal:** Administrador.  
**Ámbito:** Sistema de facturación.  
**Nivel:** Objetivo de usuario.

**Interesados e intereses:** El administrador controla las excepciones; la empresa evita operaciones económicas no autorizadas.

**Precondiciones:** El administrador está autenticado y existe una operación pendiente de aprobación que todavía no se ha aplicado o emitido.

**Disparador:** El administrador consulta una solicitud de aprobación. Puede ser una solicitud creada por él mismo.

**Garantía de éxito:** La aprobación queda registrada y la operación puede continuar.

**Garantía mínima:** Ninguna operación sujeta a aprobación se aplica sin autorización.

### Escenario principal de éxito

1. El administrador abre las solicitudes pendientes.
2. El sistema muestra la operación y sus condiciones.
3. El administrador revisa la información y aprueba la solicitud, aunque la haya creado él mismo.
4. El sistema registra la aprobación y permite continuar la operación.
5. Si se trata de una factura, su emisión continúa mediante CU-12.

### Extensiones / Flujos alternativos

- **3a. Información insuficiente:** La solicitud sigue pendiente hasta que se complete la información.
- **3b. Solicitud rechazada:** Se registra el rechazo y la operación no se ejecuta.
- **3c. Solicitud ya resuelta:** El sistema impide aprobarla o rechazarla de nuevo.

---

## CU-15. Generar un comprobante de una operación
 
**Nombre:** Generar un comprobante de una operación  
**Descripción:** Crear manualmente un documento PDF que refleje los datos registrados de una factura, un cobro o una notificación, cuando la empresa cliente solicite un comprobante. Es un documento distinto de la factura original.  
**Actor principal:** Administrador.  
**Ámbito:** Facturación y comunicaciones.  
**Nivel:** Objetivo de usuario.

**Interesados e intereses:** La empresa cliente quiere recibir una constancia documental; el administrador necesita generarla con datos fiables.

**Precondiciones:** El administrador está autenticado; la empresa cliente está registrada y sus datos identificativos son válidos. La factura, el cobro o la notificación correspondiente existe en el sistema.

**Disparador:** La empresa cliente pide un comprobante y el administrador solicita generarlo manualmente.

**Garantía de éxito:** El comprobante PDF se guarda, queda asociado al cliente y a la operación, y se pone a disposición para su entrega.

**Garantía mínima:** El comprobante no presenta como realizado un cobro o una entrega que no conste confirmada.

### Escenario principal de éxito

1. El administrador selecciona el cliente y la operación de la que necesita el comprobante.
2. El sistema verifica que la operación existe y consulta sus datos registrados.
3. El sistema prepara un comprobante con el cliente, la referencia, la fecha y el estado de la operación.
4. El administrador solicita su generación.
5. El sistema genera y guarda el PDF asociado a la operación.
6. El administrador obtiene el documento para entregarlo al cliente.

### Extensiones / Flujos alternativos

- **2a. Operación inexistente:** El sistema informa del problema y no genera el comprobante.
- **3a. Cobro o entrega sin confirmar:** El sistema no permite emitir un comprobante que afirme que se ha cobrado o entregado.
- **5a. Error de generación:** Se informa del error y no se guarda un documento incompleto.

---

## CU-16. Consultar una factura y obtener su PDF

**Nombre:** Consultar una factura y obtener su PDF  
**Descripción:** Consultar una factura existente y obtener el PDF que tiene guardado.  
**Actor principal:** Lector.  
**Ámbito:** Sistema de facturación.  
**Nivel:** Objetivo de usuario.

**Interesados e intereses:** El lector necesita consultar facturas de sus clientes asignados; la empresa debe proteger la información.

**Precondiciones:** El lector está autenticado y tiene acceso a la factura, que ya dispone de su PDF.

**Disparador:** El lector necesita consultar una factura o descargar su PDF.

**Garantía de éxito:** Se muestra la factura y se obtiene su PDF correspondiente.

**Garantía mínima:** No se muestran facturas fuera del ámbito autorizado ni se genera una factura nueva.

### Escenario principal de éxito

1. El lector accede a la consulta de facturas.
2. Localiza y selecciona una factura.
3. El sistema comprueba que puede consultarla.
4. El sistema muestra sus datos.
5. El lector solicita el PDF y el sistema le proporciona el documento guardado.

### Extensiones / Flujos alternativos

- **2a. Factura no encontrada:** El sistema informa de que no hay resultados.
- **3a. Sin permiso de acceso:** El sistema deniega la consulta.

---

## CU-17. Gestionar incidencias de entrega de correo

**Descripción:** Revisar incidencias por fallos de correo y gestionar sus reenvíos periódicos.  
**Actor principal:** Administrador.  

**Precondiciones:** El administrador está autenticado y existe una incidencia de correo registrada.

**Disparador:** El sistema detecta un fallo de entrega, crea una incidencia y avisa al administrador.

**Garantía de éxito:** La comunicación se reenvía correctamente, se registra el resultado y se resuelve la incidencia.

**Garantía mínima:** Una entrega fallida no se marca como realizada. Los reintentos no deben generar envíos duplicados involuntarios.

### Escenario principal de éxito

1. El administrador abre la incidencia de correo.
2. El sistema muestra la factura o comunicación afectada y el error registrado.
3. El administrador revisa la causa y corrige los datos cuando corresponde.
4. El sistema vuelve a intentar el envío periódicamente mientras exista un fallo confirmado, registrando cada intento.
5. Cuando se confirma la entrega, el sistema actualiza el estado de la comunicación y cierra la incidencia.
6. El sistema informa al administrador del resultado.

### Extensiones / Flujos alternativos

- **3a. El error no puede corregirse todavía:** La incidencia permanece abierta.
- **4a. El reenvío falla:** Se conserva la incidencia y se programa otro intento periódico.
- **4b. El proveedor acepta el correo, pero no confirma la entrega:** Se registra el envío aceptado sin darlo por entregado ni reenviarlo por falta de confirmación.

