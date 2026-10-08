# Documento de Requisitos del Sistema

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
## Requisitos funcionales 
1. Gestión de Accesos, Usuarios y Roles  
- FR1. Acceso multi-rol: Autenticación en el sistema mediante cuatro niveles de permisos: Superadministrador, Administrador, Usuario (lector). 
- FR2. Gestionar tres roles
Lector: Se le permite crear, consultar, editar y borrar lógicamente las fichas de clientes asignados. Almacenando como mínimo nombre de empresa, NIF, dirección, nombre del contacto y datos de contacto.
Administrador: Debe poder registrar lectores, asignarles clientes y permisos.Tiene acceso a todas las funciones del lector y a la información de todos los lectores que haya registrado.
Superadministrador: Tiene permiso para crear administradores, acceso global a toda la información, además de poder modificar plantillas, facturas y configuración del sistema.
- FR3.Uso exclusivo interno: Acceso restringido únicamente a empleados con correos de la empresa. Los clientes finales no tienen acceso ni registro en la plataforma.
2. Gestión de Clientes y Bajas
- FR4.Flujo de baja con borrado lógico: Al solicitar una baja, el sistema detiene inmediatamente la generación de facturas recurrentes, pero esta no se procesa mientras el cliente tenga facturas pendientes , posteriormente mantiene los datos del cliente bloqueados (borrado lógico) durante el tiempo máximo legal permitido.
- FR5.Liquidación de bajas: Notificación automática por email de todos los impagos pendientes al tramitar la baja de un cliente.
- FR6.Filtrado: Buscar empresas y contactos por nombre, ubicación o sector y filtrar facturas por cliente, fecha, tipo puntual o recurrente y estado pagado, pendiente o vencido, respetando los permisos.
3. Facturación y Pagos
- FR7.Emisión automatizada: Generación automática de facturas (puntuales o recurrentes mensuales), almacenamiento en el sistema y envío directo al cliente por correo electrónico. Generar y guardar un PDF estándar por factura, asociado al cliente, con datos fiscales, IVA, numeración correlativa e imagen corporativa además de conservar facturas pagadas. 
- FR8.Nuevos clientes: Generar la primera cuota en la fecha de alta por el resto del mes y las siguientes el día 1.
- FR9.Creación manual: Permiso para que los lectores creen facturas de forma manual.
- FR10.Política de pago único: El sistema solo procesará y registrará pagos completos; no se admiten pagos fraccionados.
- FR11.Conciliación Externa: Consulta automática de fuentes externas. Las deudas no se cancelan en cascada (un pago posterior no liquida deudas anteriores).
4. Notificaciones, Filtros e Informes
- FR12.Histórico de pagados: Registro del resultado de los envíos de comunicaciones, confirmación automática de cobro al cliente y conservación del histórico tras la baja. 
- FR13.Recordatorios:Enviar recordatorios amigables los días 3 y 5 desde la emisión y después cada dos o tres días, incluso tras comunicar una baja, deteniéndolos únicamente al detectar o confirmar el pago de esa factura.
- FR14.Módulo de informes: Mostrar un panel global exclusivo para administrador y superadministrador con indicadores de facturado, cobrado y pendiente agrupados por trimestre y año. 
- FR15.Revisión de baja: Exponer una API con acceso controlado para que el proceso externo de baja consulte las facturas pendientes de un cliente.
- FR16.Calendario: Mostrar un calendario visual de la programación de facturas, con visión de clientes asignados para el lector y global para administrador y superadministrador.
- FR17.Redacción automatizada de reclamaciones:Generar correos electrónicos de recordatorio de pago con un tono adaptado (amistoso, formal, urgente) según los días de retraso y el perfil del cliente.

## Requisitos no Funcionales
1. Rendimiento y Eficiencia
- NFR1. Velocidad de facturación.
Descripción: El proceso completo de generación, guardado y envío de una factura debe realizarse en menos de 1 minuto.
Métrica: Tiempo total inferior a 60 segundos en cada ejecución, medido desde que se solicita generar la factura hasta que el PDF queda guardado y el proveedor del canal confirma la aceptación del envío
Método de verificación: Ejecutar pruebas de extremo a extremo con facturas puntuales y recurrentes, registrar las marcas temporales de inicio, almacenamiento y aceptación del envío y comprobar que cada ejecución cumple el límite.
- NFR2.Eficiencia del sistema. 
Descripción: El procesamiento de datos y la carga de pantallas deben estar optimizados para evitar demoras, priorizando la agilidad en la gestión diaria.
Métrica: El percentil 95 del tiempo hasta que una pantalla sea utilizable ≤ 2 segundos y de las operaciones habituales de consulta o modificación ≤ 3 segundos.
 Método de verificación: Medir los tiempos de navegación y operaciones habituales con herramientas del navegador y pruebas de carga mediante k6 o JMeter; calcular el percentil 95 y contrastarlo con los umbrales en el entorno acordado.
2. Usabilidad y Accesibilidad
- NFR3.Diseño intuitivo.
Descripción: La interfaz debe ser lo suficientemente sencilla y fácil de usar como para permitir la creación de una ficha de cliente o factura de forma rápida y sin necesidad de formación compleja. 
Métrica: Al menos el 90 % de las tareas de crear un cliente y crear una factura se completan correctamente y en poco tiempo, por usuarios nuevos tras una introducción de como máximo cinco minutos.
Método de verificación: Realizar una prueba de usabilidad con usuarios representativos sin experiencia previa, proporcionar la misma introducción y registrar tareas completadas, solicitudes de ayuda, errores y tiempos de ejecución.
- NFR4.Compatibilidad multidispositivo.
Descripción: La aplicación debe ser accesible desde cualquier dispositivo (ordenadores, tablets y  móviles).
Métrica: El 100 % de los flujos esenciales de acceso, gestión de clientes y generación, envío y consulta de facturas funciona en ordenadores, tablets y móviles, sin controles inaccesibles ni contenido esencial oculto; matriz de sistemas, navegadores y tamaños de pantalla por acordar.
Método de verificación: Ejecutar los mismos flujos en dispositivos reales y emuladores de las tres categorías, utilizando distintos tamaños y orientaciones, y comprobar visualmente la interfaz y funcionalmente sus controles en cada combinación de la matriz.
3. Escalabilidad y Concurrencia
- NFR5.Capacidad de .
Descripción: La arquitectura del sistema debe estar preparada para escalar a miles de clientes y facturas sin perder rendimiento.
Métrica: Volumen de referencia propuesto para aprobación del equipo: 2.000 clientes y 15.000 facturas; con esa carga de datos, las tareas deben seguir cumpliendo los límites de NFR1 y NFR2, sin errores de procesamiento ni pérdida de información. 
; con esa carga de datos, las tareas deben seguir cumpliendo los límites de NFR1 y NFR2, sin errores de procesamiento ni pérdida de información.
Método de verificación: Llenar el sistema con datos sintéticos hasta el volumen acordado y repetir las pruebas de facturación, búsquedas, filtros e histórico en el mismo entorno; comparar los resultados con una carga de datos menor y comprobar tiempos, errores e integridad.
- NFR6.Soporte de concurrencia.
Descripción: El sistema debe soportar el inicio de sesión simultáneo de todos los usuarios, administradores y el superadministrador sin caídas ni ralentizaciones.
Métrica: El 100 % de los inicios de sesión con credenciales válidas se completa correctamente, con cero caídas o errores del servidor, cuando acceden simultáneamente N cuentas, siendo N el total de cuentas internas activas; límite propuesto de tiempo de autenticación: percentil 95 ≤ 2 segundos.  
Método de verificación: Lanzar una prueba de acceso simultáneo con k6 o JMeter usando cuentas distintas de todos los roles, verificar la sesión y el rol resultante de cada cuenta y registrar tiempos, errores y disponibilidad del servidor.
4. Seguridad, Privacidad y Arquitectura
NFR7.Privacidad por diseño.
Descripción: El sistema debe garantizar la protección y confidencialidad de los datos almacenados, cumpliendo con las normativas vigentes (como RGPD para el borrado lógico).
Métrica: Cero accesos o exposiciones de datos personales y fiscales a usuarios no autorizados en los casos de prueba y cumplimiento del 100 % de los controles aplicables de la lista de revisión normativa, incluida la conservación, el borrado lógico y la eliminación definitiva que correspondan; lista validada por el responsable competente.  
Método de verificación: Ejecutar pruebas de acceso no autorizado y de separación de permisos sobre pantallas, API y documentos; inspeccionar código, configuración, registros y tratamiento de datos; contrastar los resultados con la lista normativa validada, sin asumir que el borrado lógico por sí solo acredita el cumplimiento.
- NFR8.Independencia tecnológica: La aplicación es completamente independiente y aislada del sistema Brain, operando en servidores o bases de datos separadas.
- NFR9.Consistencia y fiabilidad.
Descripción: mantener el mismo PDF en almacenamiento, envío y descarga, dirigir los avisos al destinatario correcto y prevenir emisiones o avisos duplicados involuntarios.
Métrica: Igualdad del hash SHA-256 del PDF almacenado, enviado y descargado en el 100 % de las facturas probadas; destinatario correcto en el 100 % de los avisos y cero duplicados involuntarios de una misma emisión o aviso programado, incluso ante reintentos.  
Método de verificación: Ejecutar pruebas de integración con captura de comunicaciones, comparar los hashes de los PDF y los destinatarios esperados e introducir reintentos, fallos y ejecuciones simultáneas para comprobar que cada emisión o aviso se produce una sola vez; distinguir los recordatorios legítimos de días diferentes.
