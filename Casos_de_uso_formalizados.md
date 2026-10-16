# Especificación de casos de uso

## Criterio de elaboración

Este documento formaliza los diez casos de uso proporcionados utilizando la plantilla de referencia y los requisitos de `Requisitos.md`. No se dan por decididas las cuestiones que no aparecen en los requisitos: se señalan **entre paréntesis** con la información que hace falta concretar. Los identificadores CU-01 a CU-10 son provisionales.

**Criterio sobre el cliente:** el término *cliente* se refiere a la empresa cliente, que no accede ni se registra en la plataforma. Puede ser solicitante o destinatario externo, pero no ejecuta operaciones dentro del sistema. La solicitud de baja o de una excepción, si procede, llega por un canal externo y un empleado la registra o tramita en la plataforma. Los permisos concretos por rol se remiten al caso de uso de gestión de accesos, usuarios y roles.

## Interpretación de los esbozos recibidos

- **Diagramas 1 y 2 (cobrar/pagar):** se reconoce un sistema de pago externo, coherente con la conciliación de pagos mediante fuentes externas prevista en FR17. No se presupone que la empresa cliente inicie sesión en la plataforma. Los diagramas nombran los casos «Cobrar una factura» y «Pagar una factura», pero no estaban en la lista inicial de diez casos formalizados; por ello se usan como contexto y no se añaden como casos independientes en esta versión. *(Confirmar si deben incorporarse posteriormente como casos de uso propios.)*
- **Diagrama 3 (impago/baja):** la empresa puede solicitar la baja desde fuera de la plataforma; la comprobación de facturas pendientes y la tramitación dentro del sistema corresponden al flujo interno. El envío de los impagos por correo está respaldado por FR7.
- **Diagrama 4 (consulta/filtros):** se descarta la asociación directa del cliente con la consulta dentro de la plataforma, ya que no tiene acceso. La consulta y el filtrado los realiza personal interno autorizado; el cliente recibe los documentos por los canales de comunicación previstos. Consultar una factura no debe implicar generar una factura nueva: se consulta una factura ya existente y se obtiene su PDF.
- **Diagrama 5 (edición):** se mantiene al Administrador como actor del caso según el esbozo, y la edición se limita a facturas en borrador. FR13 impide alterar una factura emitida; las modificaciones posteriores deben tramitarse mediante factura rectificativa. Los permisos detallados se remiten al caso específico de roles.
- **Diagrama 6 (excepciones):** una petición del cliente, si la hay, se produce fuera de la plataforma y debe registrarla un empleado. La aprobación debe preceder a la emisión de la factura excepcional, conforme a FR23; se corrige así el orden sugerido por el esbozo, que parece situar la generación antes de la aprobación.
- **Uso de relaciones UML:** los esbozos pegados no conservan de forma inequívoca la dirección de todas las flechas ni sus etiquetas `<<include>>`/`<<extend>>`. Por eso se han utilizado como indicios del proceso, no como especificación formal de todas las dependencias.

---

# CU-01. Emitir y enviar una factura o factura rectificativa

**ID / Código:** CU-01  
**Nombre del caso de uso:** Emitir y enviar una factura o factura rectificativa  
**Descripción:** Permite emitir una factura, guardar su PDF estándar y enviarla por correo electrónico al cliente. Cuando se trate de una factura rectificativa, deberá quedar vinculada a la factura original. Antes de la emisión definitiva se validarán los datos fiscales y se mostrará una vista previa.

**Actor principal:** Empleado interno con permiso para facturar en las emisiones manuales. Para la facturación recurrente, el iniciador es el proceso programado del sistema; la autorización por rol y la gestión de clientes se mantienen dentro de los casos correspondientes.

**Ámbito (Scope):** Sistema de gestión de clientes y facturación.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Empresa:** emitir documentos fiscales coherentes, conservarlos y registrar el resultado del envío.
- **Cliente destinatario:** recibir la factura o factura rectificativa correspondiente.
- **Administrador:** revisar y aprobar las excepciones financieras que lo requieran.
- **Responsable de la factura:** conocer si el correo se ha entregado o si se ha producido una incidencia.

## Precondiciones

- El actor está autenticado como empleado interno y dispone del permiso de facturación correspondiente.
- Existe una ficha de cliente seleccionada en el sistema.
- Para emitir una factura rectificativa, existe una factura original identificada a la que vincularla.

## Disparador

El empleado solicita emitir y enviar una factura puntual o una factura rectificativa, o llega el momento programado de generar una factura recurrente. FR12 contempla facturas puntuales y recurrentes mensuales; FR15 determina la primera cuota proporcional y las siguientes el día 1.

## Garantía de éxito

La factura queda emitida y almacenada con su PDF estándar asociado al cliente, se envía por correo electrónico y se registra el resultado de la comunicación. La factura rectificativa queda vinculada al documento original.

## Garantía mínima

No se emite una factura si faltan datos fiscales críticos o si sigue pendiente una aprobación obligatoria. Si falla la entrega del correo, la factura no se marca como notificada y se genera una incidencia para su corrección y reenvío.

## Escenario principal de éxito

1. El empleado selecciona el cliente y solicita emitir una factura o una factura rectificativa.
2. El sistema prepara el documento con los datos fiscales y comerciales del cliente y los datos de facturación aplicables.
3. Si es una factura rectificativa, el sistema la vincula a la factura original seleccionada.
4. El sistema valida que estén presentes los datos fiscales críticos necesarios para emitir el documento.
5. El sistema muestra una vista previa del PDF antes de la emisión definitiva.
6. El sistema comprueba si la factura requiere aprobación por un importe inusual, una modificación manual del precio de suscripción, un descuento extraordinario o un vencimiento fuera de la política general.
7. Si no se requiere aprobación, o la aprobación obligatoria ya se ha concedido mediante CU-03, el sistema confirma la emisión.
8. El sistema guarda la factura y su PDF estándar, asociado a la ficha del cliente.
9. El sistema envía la factura al cliente por correo electrónico.
10. El sistema registra el resultado del envío y comunica el resultado al empleado.

## Extensiones / Flujos alternativos

- **4a. Faltan datos fiscales críticos.** El sistema bloquea la emisión e indica que deben completarse los datos necesarios.
- **6a. La factura necesita aprobación.** El documento permanece en estado **Borrador** hasta que se resuelva la aprobación mediante CU-03.
- **6b. La aprobación es denegada.** *(Falta definir si el borrador se mantiene, se devuelve al solicitante para su modificación o se cancela, y qué estado debe adoptar.)*
- **9a. Falla la entrega del correo.** El sistema no marca la factura como notificada, crea una incidencia asociada a la factura y alerta al responsable asignado. Se continúa mediante CU-10.
- **R1. Se ejecuta una generación recurrente programada.** El sistema genera las facturas que correspondan de forma asíncrona en segundo plano; los errores individuales quedan registrados sin detener el resto del lote, conforme a NFR3. Para clientes nuevos, la primera cuota se calcula proporcionalmente hasta final de mes y las siguientes se programan para el día 1, conforme a FR15.
- **3a. No se identifica la factura original para una rectificativa.** El sistema bloquea la emisión de la rectificativa e informa de que debe seleccionarse una factura original válida.

---

# CU-02. Actualizar el rango de facturación anual

**ID / Código:** CU-02  
**Nombre del caso de uso:** Actualizar el rango de facturación anual  
**Descripción:** Permite revisar la facturación de un cliente cuando se cumple un año desde su alta. El sistema genera una alerta y calcula una propuesta a partir de una fuente simulada; el administrador decide si la aplica a pagos futuros.

**Actor principal:** Administrador. El acceso al módulo y las operaciones permitidas a cada rol se rigen por el caso de uso de gestión de accesos, usuarios y roles.

**Ámbito (Scope):** Sistema de gestión de clientes y facturación.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Empresa:** revisar los rangos de facturación y aplicar los cambios aprobados a futuros pagos.
- **Administrador:** disponer de una propuesta para tomar la decisión.
- **Cliente:** que las condiciones futuras de facturación reflejen la decisión tomada.

## Precondiciones

- El actor está autenticado y tiene permiso para consultar el módulo de informes y decidir sobre la revisión.
- Existe una ficha de cliente con fecha de alta y tarifa acordada.
- La fuente simulada necesaria para calcular la propuesta está disponible.

*(Confirmar si la revisión se aplica solo a clientes activos o también a clientes pausados, históricos o en proceso de baja.)*

## Disparador

El sistema genera una alerta cuando se cumple un año desde el alta del cliente y lo incluye entre los clientes que requieren revisión.

## Garantía de éxito

El administrador toma una decisión sobre la propuesta y, si decide aplicarla, el sistema actualiza el rango de facturación para pagos futuros.

## Garantía mínima

Si no se toma una decisión, el sistema no aplica el cambio por su cuenta. Las facturas ya emitidas no se modifican mediante esta operación.

## Escenario principal de éxito

1. El sistema detecta que se ha cumplido un año desde el alta del cliente.
2. El sistema genera una alerta de revisión y muestra el cliente en el listado correspondiente.
3. El administrador abre la revisión de ese cliente.
4. El sistema calcula y presenta una propuesta de actualización basada en la fuente simulada.
5. El administrador revisa la propuesta y decide aplicarla.
6. El sistema actualiza el rango de facturación para los pagos futuros.
7. El sistema confirma que la actualización se ha aplicado.

## Extensiones / Flujos alternativos

- **4a. La fuente simulada no está disponible o no permite calcular la propuesta.** *(Falta definir si la revisión queda pendiente, si se reintenta automáticamente y qué aviso se muestra.)*
- **5a. El administrador no acepta la propuesta.** *(Definir si se descarta, se pospone o permanece pendiente, y si debe introducirse un motivo.)*
- **6a. Se registra la decisión y la actualización en la auditoría inmutable conforme a NFR13.** *(Los requisitos no detallan si también se conservarán explícitamente el valor anterior y el nuevo como campos separados.)*

---

# CU-03. Aprobar una excepción financiera

**ID / Código:** CU-03  
**Nombre del caso de uso:** Aprobar una excepción financiera  
**Descripción:** Permite al administrador revisar y conceder una aprobación digital antes de emitir una factura puntual con un importe inusual, aplicar una modificación manual del precio de una suscripción, conceder un descuento extraordinario o alterar un vencimiento fuera de la política general. Una excepción puede originarse en un proceso interno o en una petición externa del cliente; este último no accede a la plataforma y, si su petición se admite, un empleado debe registrarla.

**Actor principal:** Administrador, según el esbozo y FR23. Los permisos de acceso se remiten al caso de gestión de roles. *(Falta decidir si un administrador puede aprobar una solicitud que él mismo haya creado.)*

**Ámbito (Scope):** Sistema de gestión de facturación.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Empleado solicitante:** obtener una decisión sobre la operación excepcional.
- **Administrador:** controlar las excepciones y conceder una aprobación explícita.
- **Empresa:** evitar que las operaciones sujetas a aprobación se emitan sin autorización.
- **Cliente:** recibir facturas con condiciones autorizadas.

## Precondiciones

- El administrador está autenticado y tiene permiso para aprobar excepciones.
- Existe una operación identificada como pendiente de aprobación.
- La factura puntual o la modificación permanece sin emitir/aplicar mientras la aprobación sea obligatoria.

## Disparador

El sistema detecta que una factura u operación financiera requiere aprobación y la mantiene en estado **Borrador**; también puede registrarse una petición externa de excepción a través de un empleado.

## Garantía de éxito

La decisión de aprobación queda aplicada a la operación correspondiente y, si se aprueba, esta puede continuar hacia la emisión o aplicación según su tipo.

## Garantía mínima

La operación no se emite ni se aplica sin la aprobación requerida. La decisión y su responsable deben quedar trazables conforme a los requisitos de auditoría corporativa.

## Escenario principal de éxito

1. El administrador consulta la lista de aprobaciones pendientes.
2. El sistema muestra la operación, el cliente y los datos disponibles para evaluarla.
3. El administrador revisa el motivo de la excepción y sus condiciones económicas.
4. El administrador concede la aprobación digital.
5. El sistema registra de forma auditable la decisión y permite que la operación continúe.
6. Si se trata de una factura excepcional, el sistema solo después de la aprobación permite completar su emisión y envío mediante CU-01.

## Extensiones / Flujos alternativos

- **2a. La información de la solicitud es insuficiente.** *(Definir si el administrador puede devolverla al solicitante para completar datos y qué estado se utiliza.)*
- **4a. El administrador no aprueba la solicitud.** *(Falta definir la acción de rechazo, el estado resultante y si es obligatorio registrar un motivo.)*
- **4b. La solicitud ya ha sido resuelta por otro usuario.** *(Confirmar el comportamiento ante una aprobación concurrente o una solicitud que ya no esté pendiente.)*

---

# CU-04. Generar un comprobante de factura, cobro o notificación

**ID / Código:** CU-04  
**Nombre del caso de uso:** Generar un comprobante de factura, cobro o notificación  
**Descripción:** Permite generar un comprobante nuevo asociado a una factura, un cobro o una notificación. El comprobante será un documento independiente construido a partir de la información registrada en el sistema; no consiste simplemente en recuperar el PDF fiscal de la factura ni en crear una copia de seguridad técnica. Los requisitos aportados no especifican todavía el contenido ni el formato de este nuevo documento.

**Actor principal:** Empleado interno autorizado, conforme al caso de uso de gestión de accesos, usuarios y roles.

**Ámbito (Scope):** Sistema de facturación, cobros y comunicaciones.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Empleado:** disponer de evidencia de una operación concreta.
- **Empresa:** conservar la trazabilidad de facturas, cobros y comunicaciones.
- **Cliente destinatario:** poder recibir el comprobante cuando proceda.

## Precondiciones

- El actor está autenticado y autorizado para consultar el registro seleccionado.
- Existe una factura, un cobro o un registro de notificación al que asociar el comprobante.

## Disparador

El empleado solicita generar un comprobante de una factura, un cobro o una notificación. El diagrama 1 relaciona el comprobante con el flujo de facturación, pero no permite determinar si debe generarse automáticamente tras el envío o bajo petición.

## Garantía de éxito

El sistema proporciona un comprobante asociado al registro seleccionado, con información suficiente para identificar la operación.

*(Falta concretar el formato —PDF, otro formato o ambos—, los campos obligatorios para cada tipo de comprobante, si se genera automáticamente o bajo petición, y si se almacena, descarga o envía al cliente.)*

## Garantía mínima

El sistema no genera un comprobante de una operación inexistente o a la que el usuario no tenga acceso. Si no hay datos suficientes, informa de la limitación sin presentar como acreditada una operación que no consta en el sistema.

## Escenario principal de éxito

1. El empleado selecciona el tipo de registro: factura, cobro o notificación.
2. El empleado identifica el registro concreto del que necesita el comprobante.
3. El sistema comprueba que el actor puede consultar ese registro.
4. El sistema obtiene los datos registrados de la operación seleccionada.
5. El sistema genera un documento de comprobante nuevo utilizando los datos de la operación seleccionada.
6. El sistema pone el comprobante a disposición del empleado mediante el mecanismo que se defina.

## Extensiones / Flujos alternativos

- **3a. El usuario no tiene permiso para consultar el registro.** El sistema deniega el acceso y no genera el comprobante.
- **4a. No existe evidencia del envío o del cobro.** El sistema informa de que no puede acreditar ese evento con los datos disponibles. *(Decidir si, en ese caso, se admite un comprobante parcial que indique expresamente que el resultado no consta.)*
- **5a. No se puede generar el documento.** *(Definir los mensajes, el registro del error y si se permite reintentar.)*

---

# CU-05. Consultar una factura y obtener su PDF

**ID / Código:** CU-05  
**Nombre del caso de uso:** Consultar una factura y obtener su PDF  
**Descripción:** Permite localizar y consultar una factura y acceder al PDF estándar asociado a ella.

**Actor principal:** Lector/empleado interno autorizado.  
**Ámbito (Scope):** Sistema de facturación.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Lector/empleado:** consultar la información de las facturas de los clientes que tiene asignados y obtener el documento correspondiente.
- **Empresa cliente:** recibir la factura por los canales previstos, sin acceder a la plataforma; es un interesado externo, no un usuario de esta consulta.
- **Administrador:** consultar la información comprendida en sus permisos.
- **Superadministrador:** disponer de acceso global.

## Precondiciones

- El usuario ha iniciado sesión y tiene permiso para consultar la factura.
- La factura existe en el sistema o se ha importado como factura histórica.

El cliente mencionado en la lista inicial se interpreta como la empresa destinataria de la factura, no como usuario de la plataforma. La consulta y descarga desde la plataforma las realiza un empleado interno autorizado.

## Disparador

El empleado necesita consultar una factura o recuperar su PDF.

## Garantía de éxito

El usuario consulta los datos de la factura y obtiene el PDF correspondiente, respetando su ámbito de permisos.

## Garantía mínima

El sistema no revela facturas de clientes fuera del ámbito autorizado. Si no hay PDF disponible, informa de ello sin mostrar un documento distinto del asociado a la factura.

## Escenario principal de éxito

1. El usuario accede a la consulta de facturas.
2. El usuario localiza y selecciona una factura.
3. El sistema verifica los permisos del usuario sobre la factura y el cliente asociado.
4. El sistema muestra la información disponible de la factura.
5. El usuario solicita obtener el PDF.
6. El sistema proporciona al empleado el PDF estándar asociado a esa factura. Si el documento debe enviarse a la empresa cliente, se utiliza el flujo de envío correspondiente a CU-01; la empresa cliente no lo descarga desde la plataforma.

## Extensiones / Flujos alternativos

- **3a. El usuario no tiene permiso.** El sistema deniega la consulta.
- **2a. No se encuentra la factura.** El sistema informa de que no hay resultados para los criterios utilizados.
- **5a. La factura no tiene un PDF almacenado.** *(Definir si se permite generar el PDF bajo demanda para facturas antiguas/importadas o si debe informarse de que no está disponible. La solución debe respetar NFR12, que exige consistencia entre el PDF almacenado, enviado y descargado.)*
- **4a. El usuario necesita obtener varias facturas a la vez.** Esta necesidad se encauza mediante la exportación prevista en FR28 (paquete ZIP con listados y PDF del periodo), no mediante la consulta individual de este caso.

---

# CU-06. Editar una factura en borrador

**ID / Código:** CU-06  
**Nombre del caso de uso:** Editar una factura en borrador  
**Descripción:** Permite modificar una factura que todavía no se ha emitido, siempre que el usuario disponga de permisos y que se cumplan las reglas de aprobación de excepciones financieras.

**Actor principal:** Administrador, según la lista de casos proporcionada y sujeto al caso de uso de gestión de accesos, usuarios y roles.  
**Ámbito (Scope):** Sistema de facturación.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Administrador/empleado autorizado:** corregir los datos de una factura antes de emitirla.
- **Empresa:** evitar cambios no autorizados o inconsistentes.
- **Cliente:** recibir un documento correcto.

## Precondiciones

- El actor está autenticado y dispone de permisos de edición.
- La factura existe y todavía no ha sido emitida.

**Restricción:** FR13 prohíbe editar o eliminar facturas ya emitidas y exige que las modificaciones posteriores se tramiten mediante una factura rectificativa. Este caso cubre únicamente la edición de borradores. El Administrador figura como actor en el esbozo; el alcance concreto de los permisos se rige por el caso de uso de gestión de roles, que también contempla las capacidades globales del Superadministrador. No se vuelven a definir aquí los permisos generales.

## Disparador

El administrador selecciona una factura en borrador y solicita modificarla.

## Garantía de éxito

Se guardan los cambios autorizados en la factura que permanece sin emitir hasta que se cumplan las validaciones y aprobaciones aplicables.

## Garantía mínima

Una factura emitida no se modifica ni se elimina mediante este caso. Si hay un conflicto de edición concurrente, se rechaza el segundo guardado y se informa al usuario, conforme a NFR8.

## Escenario principal de éxito

1. El administrador localiza y selecciona una factura en borrador.
2. El sistema verifica que la factura no ha sido emitida y que el usuario puede editarla.
3. El administrador modifica los campos permitidos.
4. El sistema valida los datos modificados.
5. Si los cambios constituyen una excepción financiera sujeta a aprobación, el sistema mantiene el documento en borrador hasta resolver CU-03.
6. El administrador guarda los cambios.
7. El sistema comprueba que no existe una edición concurrente incompatible, guarda la versión actualizada y confirma el resultado.

## Extensiones / Flujos alternativos

- **2a. La factura ya ha sido emitida.** El sistema bloquea la edición. La modificación o devolución deberá tramitarse mediante una factura rectificativa vinculada a la original, conforme a CU-01.
- **2b. El usuario no tiene permiso de edición.** El sistema deniega la operación.
- **5a. La modificación requiere aprobación.** La factura permanece en borrador hasta que se resuelva CU-03. *(La regla sobre la autoaprobación de solicitudes creadas por un administrador queda pendiente de concretar.)*
- **6a. Otro usuario ha guardado cambios sobre la misma factura.** El sistema rechaza el guardado en conflicto y avisa al usuario, de acuerdo con NFR8.
- **3a. El usuario intenta eliminar una factura emitida.** El sistema bloquea la operación, conforme a FR13.

---

# CU-07. Filtrar facturas y fichas de clientes

**ID / Código:** CU-07  
**Nombre del caso de uso:** Filtrar facturas y fichas de clientes  
**Descripción:** Permite localizar fichas de clientes y facturas mediante criterios de búsqueda, limitando los resultados a la información que el usuario puede consultar.

**Actor principal:** Empleado interno autorizado. Según su ámbito de acceso, un lector consulta clientes asignados y los administradores disponen del acceso previsto en sus permisos. La asociación directa del cliente con el filtrado que aparece en el esbozo no se aplica, porque la empresa no accede a la plataforma.  
**Ámbito (Scope):** Sistema de gestión de clientes y facturación.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Lector/empleado:** encontrar rápidamente clientes y facturas dentro de su ámbito de acceso.
- **Administrador y Superadministrador:** localizar información dentro de sus respectivos permisos.
- **Empresa:** evitar accesos a registros no autorizados y agilizar la gestión.

## Precondiciones

- El usuario está autenticado.
- Existen fichas de clientes o facturas que se puedan consultar.
- El usuario dispone de permisos sobre los registros que se mostrarán.

## Disparador

El usuario necesita localizar una o varias fichas de clientes o facturas mediante criterios de filtrado.

## Garantía de éxito

El sistema muestra los registros coincidentes que el usuario está autorizado a consultar.

## Garantía mínima

El sistema no muestra registros fuera del ámbito autorizado. Si no hay coincidencias, muestra un resultado vacío o un mensaje equivalente.

## Escenario principal de éxito

1. El usuario elige si desea filtrar fichas de clientes o facturas.
2. El usuario introduce los criterios de búsqueda disponibles.
3. El sistema comprueba los permisos aplicables al usuario.
4. El sistema busca coincidencias entre los registros autorizados.
5. El sistema muestra los resultados coincidentes.
6. El usuario selecciona un resultado para consultar su ficha o factura mediante el caso de uso correspondiente.

## Extensiones / Flujos alternativos

- **2a. El usuario no introduce criterios suficientes.** *(Definir si se permite una consulta sin filtros y cuál es el límite de resultados.)*
- **4a. No hay coincidencias.** El sistema informa de que no se han encontrado registros.
- **2b. Se desea filtrar por campos de factura no definidos.** *(Especificar los campos y operadores admitidos para facturas. FR10 indica que las empresas se buscan por los datos guardados en la plantilla de clientes, pero no enumera criterios específicos de filtrado de facturas.)*
- **2c. Se desea filtrar por etiquetas.** *(Confirmar si las etiquetas manuales de FR11 también deben estar disponibles como criterio de filtrado.)*

---

# CU-08. Tramitar la baja de un cliente

**ID / Código:** CU-08  
**Nombre del caso de uso:** Tramitar la baja de un cliente  
**Descripción:** Permite procesar una solicitud de baja deteniendo la generación de facturas recurrentes y comprobando si existen facturas pendientes. Si hay deuda, se conserva la ficha sin borrado hasta su liquidación; si no hay deuda, se realiza el borrado lógico y la eliminación definitiva se pospone hasta el plazo legal aplicable.

**Actor principal:** Empleado interno autorizado para tramitar bajas. El rol concreto autorizado se rige por el caso de uso de gestión de accesos, usuarios y roles.

**Ámbito (Scope):** Sistema de gestión de clientes y facturación.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Cliente:** que se tramite su solicitud de baja y se le informe de las deudas pendientes.
- **Empresa:** detener nuevas cuotas, cobrar las deudas pendientes y conservar los registros que deban mantenerse.
- **Empleado responsable:** conocer el estado de la solicitud y las obligaciones pendientes.

## Precondiciones

- Existe una ficha de cliente.
- El empleado está autenticado y tiene permiso para tramitar la baja.
- El sistema puede consultar las facturas pendientes y su estado de pago mediante el proceso definido en FR25.

La empresa cliente puede solicitar la baja, pero no accede a la plataforma. El empleado autorizado registra y tramita la solicitud dentro del sistema. *(El canal externo de recepción —correo, teléfono u otro— no está especificado en los requisitos ni en los esbozos.)*

## Disparador

La empresa cliente comunica una solicitud de baja por un canal externo y un empleado autorizado inicia su tramitación.

## Garantía de éxito

Se detiene la generación de nuevas facturas recurrentes. Si existen deudas, la ficha no se borra hasta que se liquiden; si no existen facturas pendientes, se efectúa el borrado lógico de acuerdo con FR5.

## Garantía mínima

El sistema no elimina definitivamente la ficha antes del plazo legal aplicable ni realiza el borrado lógico mientras existan deudas pendientes. La tramitación de la baja no cancela las deudas anteriores.

## Escenario principal de éxito

1. El empleado localiza la ficha del cliente y registra o selecciona la solicitud de baja.
2. El sistema detiene inmediatamente la generación de facturas recurrentes del cliente, conforme a FR5.
3. El sistema consulta las facturas pendientes y sus estados de pago mediante el flujo de revisión de baja.
4. El sistema determina si existen facturas pendientes.
5. Si existen impagos, el sistema envía automáticamente por correo electrónico la relación de impagos pendientes y registra el resultado de la comunicación.
6. Si existen impagos, el sistema conserva la ficha sin realizar el borrado hasta la liquidación de la deuda.
7. Si no existen facturas pendientes, el sistema realiza el borrado lógico de los datos del cliente.
8. El sistema conserva los registros e históricos que correspondan y deja la eliminación definitiva sujeta al plazo legal máximo aplicable.
9. El sistema informa al empleado del resultado del trámite.

## Extensiones / Flujos alternativos

- **3a. No se puede consultar el estado de las facturas pendientes.** *(Definir si se bloquea el trámite, se reintenta la consulta o se deriva la incidencia a un responsable.)*
- **5a. Falla el envío del correo con los impagos.** El sistema no registra la comunicación como notificada y abre una incidencia asociada al envío, conforme a FR20; se continúa mediante CU-10.
- **4a. Existen deudas, pero se solicita eliminar la ficha igualmente.** El sistema no realiza el borrado lógico mientras no se liquide la deuda, conforme a FR5.
- **6a. Se liquida posteriormente la deuda.** *(Definir si el sistema ejecuta el borrado lógico automáticamente al detectar la liquidación o si debe intervenir un empleado.)*

---

# CU-09. Gestionar la ficha de un cliente

**ID / Código:** CU-09  
**Nombre del caso de uso:** Gestionar la ficha de un cliente  
**Descripción:** Permite crear y mantener la ficha de un cliente utilizando la plantilla definida, consultar y modificar los datos autorizados, asociar documentos contractuales y administrar la información complementaria contemplada en los requisitos.

**Actor principal:** Empleado interno autorizado; las operaciones disponibles dependen del rol. El esbozo agrupa el alta/registro dentro de la gestión de clientes, por lo que se mantiene aquí como caso general y sus variantes se describen en los flujos alternativos.

**Ámbito (Scope):** Sistema de gestión de clientes.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Empleado/Lector:** crear, consultar y mantener las fichas de los clientes que tiene asignados, dentro de sus permisos.
- **Administrador:** gestionar lectores, asignar clientes y permisos, y delegar clientes temporal o indefinidamente.
- **Superadministrador:** administrar la plantilla de clientes y disponer de acceso global.
- **Empresa:** conservar datos fiscales y comerciales completos y evitar duplicados.
- **Cliente:** mantener correctos sus datos y condiciones comerciales.

## Precondiciones

- El empleado está autenticado.
- El rol dispone de permisos para la operación solicitada.
- Para modificar una ficha existente, esta se encuentra registrada en el sistema.

## Disparador

El usuario necesita dar de alta un cliente o mantener la información de una ficha existente.

## Garantía de éxito

La ficha queda creada o actualizada con los datos autorizados, y las operaciones adicionales solicitadas se realizan solo si están permitidas por el rol y las reglas correspondientes.

## Garantía mínima

El sistema no permite realizar operaciones fuera de los permisos del usuario. Las alertas de posibles duplicados se muestran antes de realizar una fusión y toda fusión autorizada debe quedar auditada.

## Escenario principal de éxito

1. El usuario selecciona la opción de crear una ficha o modificar una ficha existente.
2. El sistema comprueba que el usuario tiene permisos para la operación y, si corresponde, acceso a ese cliente asignado.
3. Para una nueva ficha, el usuario completa los campos obligatorios de la plantilla: nombre de empresa, nombre comercial, razón social, país, moneda, identificador fiscal, dirección, contactos, fecha de contratación, tarifa acordada y condiciones particulares de pago.
4. El usuario asocia, cuando proceda, los documentos contractuales firmados externamente.
5. El sistema comprueba el identificador fiscal para detectar coincidencias con fichas existentes o históricas.
6. Si no hay un duplicado que requiera intervención, el sistema guarda la ficha o los cambios autorizados.
7. El sistema confirma el resultado al usuario.

## Extensiones / Flujos alternativos

- **3a. Faltan campos obligatorios.** *(Definir los mensajes y si se permite guardar un borrador incompleto.)*
- **5a. El identificador fiscal coincide con una ficha existente o histórica.** El sistema muestra una alerta de posible duplicado. La reactivación de una ficha histórica está contemplada por FR8; cualquier fusión de duplicados requiere autorización y auditoría.
- **6a. El usuario intenta modificar la plantilla de clientes.** Solo el Superadministrador puede hacerlo, conforme a FR9.
- **6b. El usuario desea asignar o delegar clientes a lectores.** La operación corresponde al Administrador y debe respetar los permisos y reglas definidos para las asignaciones.
- **6c. El usuario desea aplicar etiquetas.** El sistema permite utilizar las etiquetas manuales administrables contempladas por FR11, de acuerdo con los permisos definidos en el caso de gestión de roles.
- **6d. El usuario desea pausar una suscripción.** Debe registrar obligatoriamente el motivo y la fecha de inicio, así como la fecha prevista de reactivación si se conoce. El sistema bloquea la generación de cuotas mensuales durante la pausa y mantiene las deudas anteriores, conforme a FR6.
- **6e. El usuario desea reactivar una ficha histórica.** El sistema activa el flujo de reactivación de FR8 y comprueba las coincidencias por identificador fiscal.
- **6f. Dos empleados modifican la misma ficha a la vez.** El sistema debe detectar la colisión y rechazar el segundo guardado incompatible, informando al usuario, conforme a NFR8.

---

# CU-10. Gestionar incidencias

**ID / Código:** CU-10  
**Nombre del caso de uso:** Gestionar incidencias  
**Descripción:** Permite revisar y tratar las incidencias asociadas a fallos de entrega de correos. Cuando el sistema detecta una entrega fallida, crea automáticamente una incidencia relacionada con la factura y avisa al responsable asignado para que la corrija y vuelva a enviar la comunicación.

**Actor principal:** Empleado responsable de la incidencia. El alcance definido por los requisitos es la gestión de incidencias por fallos de entrega de correo; otros tipos no se incorporan porque no están descritos en los requisitos.

**Ámbito (Scope):** Sistema de facturación y comunicaciones.  
**Nivel (Level):** Objetivo de usuario.

## Interesados e intereses

- **Empleado responsable:** detectar y corregir fallos en los envíos asignados.
- **Empresa:** evitar que los envíos fallidos aparezcan como notificados y conservar el histórico.
- **Cliente:** recibir la factura o comunicación que le corresponde.

## Precondiciones

- El empleado está autenticado y tiene permiso para consultar la incidencia y la factura asociada.
- Para el flujo automático descrito en FR20, el sistema ha detectado un fallo de entrega o ha creado una incidencia por ese motivo.

## Disparador

El sistema detecta un fallo de entrega de correo y crea una incidencia asociada a la factura, o el empleado accede a la bandeja de incidencias para tratar una incidencia ya registrada.

## Garantía de éxito

El empleado puede identificar la causa, corregirla y volver a enviar la comunicación. El resultado queda registrado y la factura solo se marca como notificada cuando el envío se confirma según las reglas del sistema.

## Garantía mínima

Una comunicación fallida no se marca como notificada. La incidencia queda asociada a la factura y el responsable asignado recibe una alerta para su corrección y reenvío.

## Escenario principal de éxito

1. El empleado accede a su bandeja de trabajo o a la incidencia asignada.
2. El sistema muestra la incidencia y la factura relacionada.
3. El empleado revisa el resultado del envío y los datos disponibles para identificar la causa del fallo.
4. El empleado corrige el problema que impide la entrega. *(Precisar qué datos puede modificar el responsable, por ejemplo, el destinatario del correo.)*
5. El empleado solicita reenviar la comunicación.
6. El sistema intenta realizar el envío y registra el resultado de la comunicación.
7. Si el envío se confirma, el sistema actualiza el estado de la comunicación conforme a las reglas de notificación.
8. El sistema informa al empleado del resultado.

## Extensiones / Flujos alternativos

- **3a. No hay información suficiente para identificar la causa.** *(Definir si se puede asignar la incidencia a otro responsable, añadir notas o escalarla.)*
- **6a. El reenvío vuelve a fallar.** La factura no se marca como notificada y el responsable debe ser alertado para continuar la corrección, conforme a FR20. *(Definir si se conserva la misma incidencia, se crea otra o se incrementa un contador de reintentos.)*
- **7a. El envío se ha aceptado, pero no se confirma la entrega final.** *(Precisar qué respuesta del proveedor se considera suficiente para actualizar el estado; NFR1 se refiere a la aceptación del envío por el proveedor.)*
- **8a. Se necesita gestionar una categoría distinta de fallo de correo.** No se define un flujo para este caso, porque los requisitos actuales solo especifican incidencias de entrega de correo. *(Si se quieren incluir otras categorías, será necesario definirlas.)*

---

# Cuestiones que siguen abiertas

Las siguientes decisiones no quedan resueltas por los requisitos ni por los seis esbozos. Las anotaciones correspondientes también aparecen entre paréntesis en el caso afectado:

1. **Comprobantes:** definir formato, contenido y campos obligatorios; decidir si se generan automáticamente al emitir/enviar una factura, registrar un cobro o enviar una notificación, o si se solicitan manualmente; y determinar si se almacenan, descargan o envían a la empresa cliente. También falta decidir si puede emitirse un comprobante parcial cuando no existe evidencia de entrega/cobro y cómo se registran los errores de generación.
2. **Rango anual de facturación:** concretar qué significa exactamente «rango», cómo calcula la fuente simulada la propuesta y qué hacer si la fuente falla. También falta decidir si se revisan clientes pausados, históricos o en baja, qué ocurre si se rechaza o pospone la propuesta y si la auditoría conserva por separado los valores anterior y nuevo.
3. **Filtros:** especificar los campos y operadores de búsqueda de facturas, el comportamiento de una búsqueda sin criterios y si las etiquetas de FR11 deben estar disponibles como filtro. FR10 sí indica que las fichas de clientes se buscan usando los datos guardados en su plantilla.
4. **Consulta de facturas:** definir el comportamiento cuando una factura histórica/importada no dispone de PDF almacenado. NFR12 obliga a mantener la consistencia entre el PDF almacenado, enviado y descargado.
5. **Tramitación de bajas:** definir el canal externo por el que la empresa comunica la solicitud, qué hacer si no se pueden consultar las facturas pendientes y si, una vez liquidada la deuda, el borrado lógico se ejecuta automáticamente o requiere intervención de un empleado.
6. **Aprobaciones financieras:** concretar el estado y el tratamiento de una solicitud denegada, qué ocurre si la información es insuficiente y si un administrador puede aprobar su propia solicitud. También debe definirse el comportamiento si otro usuario resuelve la solicitud concurrentemente.
7. **Incidencias:** los requisitos especifican expresamente incidencias por fallo de entrega de correo. Si se desea ampliar el alcance, faltan por definir otras categorías. Para las incidencias de correo, falta precisar qué campos puede corregir el responsable, si puede reasignar/escalar la incidencia, cómo se gestionan reintentos repetidos y qué respuesta del proveedor basta para cambiar el estado.
8. **Datos de cliente incompletos:** decidir si puede guardarse como borrador una ficha a la que todavía le falten campos obligatorios; FR9 sí establece cuáles son los campos exigidos para la plantilla completa.
9. **Casos de cobro/pago:** los diagramas 1 y 2 los muestran como casos separados, pero no forman parte de la lista inicial de diez. *(Confirmar si deben añadirse como casos de uso independientes en una iteración posterior.)*
## Aspectos que ya quedan resueltos con la información recibida

- **Acceso del cliente:** la empresa cliente no inicia sesión ni consulta directamente el sistema. Sus peticiones se comunican externamente y un empleado autorizado opera en la plataforma; el cliente puede recibir documentos y notificaciones.
- **Permisos por rol:** no se vuelven a definir en estos casos, ya que están descritos en el caso de uso específico. El caso de edición sigue el esbozo y se limita a borradores; FR13 impide modificar facturas emitidas.
- **Comprobantes:** se trata de generar un documento nuevo que acredite una factura, un cobro o una notificación, no de una copia de seguridad técnica.
- **Facturación recurrente:** forma parte del flujo de emisión según FR12 y FR15; las ejecuciones masivas deben realizarse en segundo plano de acuerdo con NFR3. Ya no se deja pendiente decidir si el sistema debe contemplarlas.
- **Orden de aprobación:** las excepciones que la requieren se aprueban antes de emitir la factura excepcional, conforme a FR23; una petición de la empresa cliente se tramita fuera de la plataforma.
