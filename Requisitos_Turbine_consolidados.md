# Requisitos

Base consolidada para revisión del equipo, elaborada a partir de la transcripción y los documentos de requisitos; las mejoras opcionales y las propuestas del equipo se identifican expresamente.

## Requisitos de negocio

Necesidades, objetivos y reglas de Turbine que motivan y delimitan el desarrollo del sistema.

- RN1. Reducir el trabajo administrativo manual automatizando la creación de facturas, el seguimiento de pagos y la comunicación de incidencias.
- RN2. Centralizar el control de clientes, facturación y cobros para conocer lo facturado, cobrado y pendiente y facilitar la supervisión financiera.
- RN3. Delegar la gestión de clientes entre empleados, manteniendo permisos por cliente, supervisión global y avisos al responsable correspondiente.
- RN4. Facilitar la gestión diaria en pocos clics, con poco aprendizaje y desde cualquier lugar, incluido el teléfono móvil.
- RN5. Sostener el crecimiento hasta miles de clientes y conservar su histórico de facturas y cobros.
- RN6. Cobrar suscripciones mensuales por tramos de facturación anual del cliente, sin permanencia anual ni pagos fraccionados, con precio ordinario estable durante el año y revisión anual, además de servicios puntuales con importe propio.
- RN7. Proteger los datos personales y fiscales y mantener facturas válidas, coherentes y conservadas conforme a la categoría de información y finalidad aplicables.
- RN8. Disponer de una aplicación administrativa interna independiente de Brain, con una solución técnica asequible y sin imponer una tecnología concreta.

## Requisitos de usuario

Tareas y resultados que los actores necesitan obtener; el lector opera sobre sus clientes asignados, el administrador sobre todos y el superadministrador añade configuración avanzada y acceso a todos los administradores y lectores.

- RU1. El personal interno necesita iniciar sesión con correo corporativo y contraseña y acceder únicamente a los clientes autorizados para crearlos, consultarlos, modificarlos y darlos de baja lógicamente. Además poder importar sus datos desde CSV.
- RU2. El administrador necesita registrar empleados y asignarles clientes y permisos; el superadministrador necesita gestionar los privilegios administrativos y acceder a todos los administradores y lectores.
- RU3. El personal autorizado necesita crear y modificar facturas puntuales o configurar suscripciones mensuales dentro de las resctricciones acordadas. Además enviar sus PDF, consultar y descargar el histórico del cliente.
- RU4. El personal autorizado necesita modificar facturas dentro de las restricciones de edición acordadas, enviar sus PDF y consultar y descargar el histórico del cliente.
- RU5. El lector asignado necesita conocer qué facturas están pagadas, pendientes o vencidas, revisar cobros y recibir avisos de pago e impago de sus clientes.
- RU6. El cliente externo necesita recibir facturas y recordatorios amigables, disponer del correo y teléfono de soporte, mediante comunicaciones externas y sin acceder a la aplicación.
- RU7. El personal autorizado necesita registrar la comunicación de baja, consultar, notificar deudas y completar la baja administrativa tras su liquidación.
- RU8. El administrador y el superadministrador necesitan consultar un panel global de facturado, cobrado y pendiente por períodos trimestrales y anuales.
- RU9. El superadministrador necesita poder crear nuevos administradores, modificar la plantilla y tener acceso para modificar las facturas.
- RU10. El lector necesita visualizar las próximas facturas en un calendario, limitado a sus clientes asignados o global según su rol.
- RU11. El equipo de Turbine necesita realizar sus tareas desde móvil, tableta u ordenador y disponer de documentación de uso y configuración.

## Requisitos del sistema

### Requisitos Funcionales

- FR1. Autenticar empleados con correo corporativo de Turbine y contraseña, verificar su pertenencia a la empresa y rechazar correos personales o ajenos.
- FR2. Gestionar tres roles
    -  Lector: Se le permite crear, consultar, editar y borrar lógicamente los clientes asignados. Almacenando como mínimo nombre de empresa, NIF, dirección, nombre del contacto y datos de contacto.
    - Administrador: Debe poder registrar lectores, asignarles clientes y permisos.Tiene acceso a todas las funciones del lector y a la información de todos los lectores que haya registrado.
    - Superadministrador: Tiene permiso para crear administradores, acceso global a toda la información, además de poder modificar plantillas, facturas y configuración del sistema.

- FR5. Buscar empresas y contactos por nombre, ubicación o sector y filtrar facturas por cliente, fecha, tipo puntual o recurrente y estado pagado, pendiente o vencido, respetando los permisos.
- FR6. Crear facturas puntuales para clientes nuevos o existentes y configurar facturación recurrente mensual, generando una factura nueva por período sin intervención manual ni planes de pago fraccionado.
- FR7. Generar la primera cuota en la fecha de alta por el resto del mes y las siguientes el día 1; fórmula de prorrateo, redondeo e impuestos por concretar.
- FR8. Permitir establecer importes propios para facturas puntuales y cuotas por tramo de facturación anual para suscripciones, mantener el precio ordinario durante el año y actualizarlo tras la revisión anual.
- FR9. Generar y guardar un PDF estándar por factura, asociado al cliente, con datos fiscales, IVA, numeración correlativa e imagen corporativa; conservar facturas pagadas y pendientes.
- FR10. Enviar facturas y comunicaciones al cliente por correo electrónico o WhatsApp, incluyendo correo y teléfono de soporte; canal definitivo por acordar, sin chat propio.
- FR11. Consultar automáticamente una fuente externa de cobros, asociar cada pago a su factura y actualizar su estado; prever Stripe y admitir simulación en la entrega, sin implementar una pasarela propia.
- FR12. Calcular el vencimiento desde la fecha de factura y el plazo estándar de cinco días; el pago de una factura posterior no debe liquidar deudas anteriores.
- FR13. Enviar recordatorios amigables los días 3 y 5 desde la emisión y después cada dos o tres días, incluso tras comunicar una baja, deteniéndolos únicamente al detectar o confirmar el pago de esa factura.
- FR14. Notificar cobros y seguimientos de impago al usuario interno asignado al cliente y permitir revisar los cobros detectados y cambiar el estado del cliente dentro de las reglas de baja; el impago no cancela automáticamente el servicio.
- FR15. Registrar la baja comunicada por personal autorizado o el proceso externo, detener nuevas facturas recurrentes, informar de todas las deudas pendientes y completar la baja administrativa solo tras liquidarlas, incluidas las puntuales.
- FR16. Mostrar un panel global exclusivo para administrador y superadministrador con indicadores de facturado, cobrado y pendiente agrupados por trimestre y año.
- FR17. Permitir ajustes ordinarios al personal autorizado y reservar al superadministrador la configuración profunda, incluido el cambio del plazo estándar de pago.
- FR18. Consultar el histórico completo conservado del cliente y descargar sus PDF según permisos, manteniéndolo después de la baja o el borrado lógico cuando corresponda.
- FR19. Exponer una API con acceso controlado para que el proceso externo de baja consulte las facturas pendientes de un cliente.
- FR20. Permitir editar facturas conforme a su ciclo de vida y las restricciones acordadas con Turbine y su gestor; concretar los límites antes y después de la emisión.
- FR21. Mostrar un calendario visual de la programación de facturas, con visión de clientes asignados para el lector y global para administrador y superadministrador.
- FR22. Mejora opcional: importar empresas cliente y contactos desde CSV, acordando columnas, tratamiento de duplicados y asignación de responsables.
- FR23. Mejora opcional: consultar una fuente externa de facturación anual y avisar de posibles cambios de tramo o plan; fuente de datos por concretar.
- FR24. Mejora opcional: generar copias periódicas de datos y movimientos y permitir al superadministrador consultar actividad de clientes, cobros y permisos; periodicidad, alcance y conservación por acordar.
- FR25. Propuesta del equipo: enviar al cliente una confirmación automática cuando se confirme el cobro de su factura, sin generar un PDF adicional de justificante.
- FR26. Propuesta del equipo: permitir al superadministrador configurar plantillas, campos y variables de futuras facturas, preservando el formato fiscal y las restricciones de edición de las ya emitidas.
- FR27. Propuesta del equipo: registrar el resultado del envío de facturas y comunicaciones para facilitar su seguimiento.
- FR28. Propuesta del equipo: permitir corregir manualmente el estado de pago de una factura, con permisos específicos y registro de autor y actuación, si Turbine aprueba este procedimiento.
- FR29. Mejora futura: ampliar la API para consultas externas adicionales, definiendo operaciones y permisos sin exigir integración con Brain en la entrega actual.
- FR30. Propuesta del equipo: incorporar las siguientes ayudas de IA como mejoras adicionales al flujo básico de facturación y cobros.
- FR30.1. Analizar el histórico de pagos para identificar clientes con riesgo de retraso o impago, respetando el ámbito de clientes autorizado.
- FR30.2. Procesar extractos bancarios mediante texto o visión artificial para proponer asociaciones entre ingresos y facturas; fuente y validación por acordar.
- FR30.3. Generar textos de recordatorio adaptados al retraso y perfil del cliente, manteniendo el tono amigable y la cadencia acordada.
- FR30.4. Sugerir días y horas de envío que puedan mejorar la apertura de comunicaciones y el cobro, respetando las fechas de emisión y recordatorio establecidas.
- FR30.5. Estimar ingresos y flujo de caja futuro a partir de las suscripciones, facturas y tendencias históricas de cobro, para usuarios con acceso al panel global.

### Requisitos No-Funcionales

- NFR1. Usabilidad: ofrecer una interfaz sencilla, pocos clics y poca curva de aprendizaje, con el objetivo de completar alta, facturación y chequeo en menos de un minuto por tarea; escenarios por definir.
- NFR2. Movilidad: ofrecer una aplicación web responsive y operativa en móvil, tableta y ordenador, permitiendo emitir, enviar y consultar facturas desde el teléfono.
- NFR3. Seguridad y privacidad: proteger datos personales y fiscales desde el diseño y aplicar los permisos por cliente a consultas, cambios, API y descargas; no se exige doble factor para el alcance básico.
- NFR4. Consistencia y fiabilidad: mantener el mismo PDF en almacenamiento, envío y descarga, dirigir los avisos al destinatario correcto y prevenir emisiones o avisos duplicados involuntarios.
- NFR5. Escalabilidad: admitir miles de clientes y su histórico; la propuesta de sesiones simultáneas del equipo se evaluará con un volumen de usuarios y condiciones de carga por concretar.
- NFR6. Conformidad y conservación: respetar el formato fiscal estándar y conservar datos y documentos tras la baja conforme a su categoría y finalidad, con borrado lógico cuando proceda y eliminación definitiva acordada, incluidas las copias si se implementan.
- NFR7. Documentación: entregar instrucciones de uso, configuración y API comprensibles para el equipo de Turbine y utilizables como referencia por sus agentes de IA.
- NFR8. Independencia: funcionar como aplicación administrativa interna independiente de Brain, sin acceso ni autorregistro de clientes externos ni dependencia de modificar el producto comercial.
- NFR9. Adecuación económica y técnica: priorizar una base de datos asequible, considerar alternativas libres y elegir tecnologías sin imponer las usadas en Brain; presupuesto por concretar.
- NFR10. Entrega verificable: el flujo de generación, almacenamiento y envío de facturas debe funcionar con datos de prueba aunque los cobros se simulen.
- NFR11. Propuesta del equipo: ejecutar las tareas de IA en segundo plano para preservar la agilidad de la interfaz y las tareas habituales, si se incorporan esas mejoras.
