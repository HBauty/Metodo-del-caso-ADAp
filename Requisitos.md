# Documento de Requisitos del Sistema

## 1. Requisitos Funcionales

Requisitos Funcionales Generalizados:
1. Gestión de Accesos, Usuarios y Roles  
Acceso multi-rol: Autenticación en el sistema mediante cuatro niveles de permisos: Superadministrador, Administrador, Usuario (lector). 
Uso interno Aislado
Gestión de permisos jerárquica:
El Superadministrador gestiona y otorga permisos a los Administradores.
El Administrador registra usuarios y asigna permisos para crear, modificar y gestionar clientes.
Uso exclusivo interno: Acceso restringido únicamente a empleados de la empresa. Los clientes finales no tienen acceso ni registro en la plataforma.
Diseño responsivo: Interfaz completamente adaptada para funcionar y ser operativa desde dispositivos móviles.
2. Gestión de Clientes y Bajas
Fichas de clientes: Creación, modificación y almacenamiento de la información de los clientes.
Flujo de baja con borrado lógico: Al solicitar una baja, el sistema detiene inmediatamente la generación de facturas recurrentes, pero mantiene los datos del cliente bloqueados (borrado lógico) durante el tiempo máximo legal permitido.
Liquidación de bajas: Notificación automática por email de todos los impagos pendientes al tramitar la baja de un cliente.
3. Facturación y Pagos
Emisión automatizada: Generación automática de facturas recurrentes, almacenamiento en el sistema y envío directo al cliente por correo electrónico.
Creación manual: Permiso para que los usuarios autorizados creen facturas de forma manual.
Política de pago único: El sistema solo procesará y registrará pagos completos; no se admiten pagos fraccionados.
Configuración avanzada: Panel exclusivo para el Superadministrador que permite modificar en cualquier momento el formato, variables de las facturas y el plazo límite de pago.
4. Notificaciones, Filtros e Informes
Alertas automáticas de impago: Envío de recordatorios automáticos por email ante facturas pendientes (por ejemplo, a los 3 y a los 5 días del vencimiento).
Persistencia de reclamación: Continuar enviando recordatorios de impagos pendientes incluso si el cliente ya ha iniciado el proceso de baja.
Histórico de pagados: Registro, confirmación y almacenamiento de las facturas que ya han sido abonadas.
Buscador y filtros avanzados: Herramienta de filtrado de facturas por criterios como ubicación, temática del cliente, estado del pago y fecha de emisión.
Módulo de informes: Generación de reportes periódicos y estadísticos sobre el estado financiero y de facturación.




Requisitos no Funcionales Generalizados:
1. Rendimiento y Eficiencia
Velocidad de facturación: El proceso completo de generación, guardado y envío de una factura debe realizarse en menos de 1 minuto.
Eficiencia del sistema: El procesamiento de datos y la carga de pantallas deben estar optimizados para evitar demoras, priorizando la agilidad en la gestión diaria.
2. Usabilidad y Accesibilidad
Diseño intuitivo: La interfaz debe ser lo suficientemente sencilla y fácil de usar como para permitir la creación de una ficha de cliente o factura de forma rápida y sin necesidad de formación compleja.
Compatibilidad multidispositivo: La aplicación debe ser 100% responsiva y accesible desde cualquier dispositivo (ordenadores, tablets y móviles).
3. Escalabilidad y Concurrencia
Capacidad de crecimiento: La arquitectura del sistema debe estar preparada para escalar a miles de clientes y facturas sin perder rendimiento.
Soporte de concurrencia: El sistema debe soportar el inicio de sesión simultáneo de todos los usuarios, administradores y el superadministrador sin caídas ni ralentizaciones.
4. Seguridad, Privacidad y Arquitectura
Privacidad por diseño: El sistema debe garantizar la protección y confidencialidad de los datos almacenados, cumpliendo con las normativas vigentes (como RGPD para el borrado lógico).
Autenticación simplificada: No se requerirá doble factor de autorización (2FA) para el acceso, priorizando una entrada rápida al sistema.
Independencia tecnológica: La aplicación es completamente independiente y aislada del sistema Brain, operando en servidores o bases de datos separadas.
Eficiencia en modelos de IA: Las tareas de Inteligencia Artificial (como la predicción de impagos o la lectura de extractos bancarios) deben ejecutarse en segundo plano (background) para no penalizar el rendimiento ni la velocidad de la interfaz de usuario. 



Requisitos de negocio
Necesidades, objetivos y reglas de Turbine que motivan y delimitan el desarrollo del sistema.

RN1. Reducir el trabajo administrativo manual automatizando la creación de facturas, el seguimiento de pagos y la comunicación de incidencias.
RN2. Centralizar el control de clientes, facturación y cobros para conocer lo facturado, cobrado y pendiente y facilitar la supervisión financiera.
RN3. Delegar la gestión de clientes entre empleados, manteniendo permisos por cliente, supervisión global y avisos al responsable correspondiente.
RN4. Facilitar la gestión diaria en pocos clics, con poco aprendizaje y desde cualquier lugar, incluido el teléfono móvil.
RN5. Sostener el crecimiento hasta miles de clientes y conservar su histórico de facturas y cobros.
RN6. Cobrar suscripciones mensuales por tramos de facturación anual del cliente, sin permanencia anual ni pagos fraccionados, con precio ordinario estable durante el año y revisión anual, además de servicios puntuales con importe propio.
RN7. Proteger los datos personales y fiscales y mantener facturas válidas, coherentes y conservadas conforme a la categoría de información y finalidad aplicables.
RN8. Disponer de una aplicación administrativa interna independiente de Brain, con una solución técnica asequible y sin imponer una tecnología concreta.
Requisitos de usuario
Tareas y resultados que los actores necesitan obtener; el lector opera sobre sus clientes asignados, el administrador sobre todos y el superadministrador añade configuración avanzada y acceso a todos los administradores y lectores.

RU1. El personal interno necesita iniciar sesión con correo corporativo y contraseña y acceder únicamente a los clientes autorizados para crearlos, consultarlos, modificarlos y darlos de baja lógicamente. Además poder importar sus datos desde CSV.
RU2. El administrador necesita registrar empleados y asignarles clientes y permisos; el superadministrador necesita gestionar los privilegios administrativos y acceder a todos los administradores y lectores.
RU3. El personal autorizado necesita crear y modificar facturas puntuales o configurar suscripciones mensuales dentro de las resctricciones acordadas. Además enviar sus PDF, consultar y descargar el histórico del cliente.
RU4. El personal autorizado necesita modificar facturas dentro de las restricciones de edición acordadas, enviar sus PDF y consultar y descargar el histórico del cliente.
RU5. El lector asignado necesita conocer qué facturas están pagadas, pendientes o vencidas, revisar cobros y recibir avisos de pago e impago de sus clientes.
RU6. El cliente externo necesita recibir facturas y recordatorios amigables, disponer del correo y teléfono de soporte, mediante comunicaciones externas y sin acceder a la aplicación.
RU7. El personal autorizado necesita registrar la comunicación de baja, consultar, notificar deudas y completar la baja administrativa tras su liquidación.
RU8. El administrador y el superadministrador necesitan consultar un panel global de facturado, cobrado y pendiente por períodos trimestrales y anuales.
RU9. El superadministrador necesita poder crear nuevos administradores, modificar la plantilla y tener acceso para modificar las facturas.
RU10. El lector necesita visualizar las próximas facturas en un calendario, limitado a sus clientes asignados o global según su rol.
RU11. El equipo de Turbine necesita realizar sus tareas desde móvil, tableta u ordenador y disponer de documentación de uso y configuración.


