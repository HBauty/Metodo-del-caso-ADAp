# Requisitos generalizados

## Requisitos Funcionales Generalizados:

### 1. Gestión de Accesos, Usuarios y Roles

#### FR1. Acceso multi-rol

Autenticación en el sistema mediante tres niveles de permisos: Superadministrador, Administrador, Usuario (lector).

#### FR2. Gestionar tres roles

- **Lector:** Se le permite, siguiendo unos permisos, crear (utilizando la plantilla), consultar, editar y borrar lógicamente las fichas de clientes asignados.
- **Administrador:** Debe poder registrar lectores, asignarles clientes y permisos. Tiene acceso a todas las funciones del lector y a la información de todos los lectores que haya registrado. Puede delegar temporal o indefinidamente clientes de un usuario a otros.
- **Superadministrador:** Tiene permiso para crear administradores, acceso global a toda la información, además de poder modificar plantillas, facturas y configuración del sistema.

> **Dependencias:** FR1 (la asignación de permisos se aplica a usuarios autenticados).

#### FR3. Uso exclusivo interno

Acceso restringido únicamente a empleados con correos de la empresa. Los clientes finales no tienen acceso ni registro en la plataforma.

> **Dependencias:** FR1 (el acceso interno se controla durante la autenticación).

#### FR4. Desactivación de cuentas

Permitir la desactivación de cuentas de usuarios internos que dejen la empresa, invalidando inmediatamente sus sesiones activas pero conservando de forma inmutable el rastro y el historial de todas las acciones que realizaron en el pasado.

> **Dependencias:** FR1 (requiere gestionar autenticación y sesiones); FR2 (requiere identificar la cuenta y su rol).

### 2. Gestión de Clientes y Bajas

#### FR5. Flujo de baja con borrado lógico

Al solicitar una baja de un cliente que no tenga facturas pendientes, el sistema detiene inmediatamente la generación de facturas recurrentes y posteriormente se realiza un borrado lógico de datos del cliente, los cuáles se eliminarán definitivamente pasado el plazo máximo legal permitido. En caso de tener deudas se detendrán las facturas pero no se realizará el borrado hasta su liquidación.

> **Dependencias:** FR9 (requiere identificar la ficha del cliente); FR12 (requiere detener la generación recurrente y consultar las facturas emitidas); FR25 (requiere consultar las facturas pendientes mediante el flujo de baja).

#### FR6. Pausa de suscripción

Permitir pausar temporalmente una suscripción activa registrando obligatoriamente el motivo, la fecha de inicio y la fecha prevista de reactivación si se conoce, bloqueando la generación de cuotas mensuales durante la pausa pero manteniendo vivas las deudas previas.

> **Dependencias:** FR9 (requiere una ficha de cliente y sus condiciones comerciales); FR12 (la pausa afecta a la generación recurrente).

#### FR7. Liquidación de bajas

Notificación automática por email de todos los impagos pendientes al tramitar la baja de un cliente.

> **Dependencias:** FR5 (se activa al tramitar la baja); FR12 (requiere localizar las facturas pendientes); FR18 (requiere registrar el resultado de la comunicación).

#### FR8. Reactivación y control de duplicados

Permitir la reactivación de una ficha histórica si un cliente antiguo regresa, implementando un sistema de alerta por identificador fiscal (NIF). Cualquier fusión de duplicados será autorizada y auditada.

> **Dependencias:** FR9 (requiere la ficha y el identificador fiscal para detectar duplicados);

#### FR9. Plantilla de clientes

La plantilla solo es modificable por el superadmin y debe contar obligatoriamente con el nombre de empresa, nombre comercial, razón social, país, moneda, identificador fiscal internacional o NIF en España, dirección, datos de los contactos, fecha de contratación, tarifa acordada, condiciones de pago particulares y capacidad de asociar documentos contractuales firmados externamente.

#### FR10. Filtrado

Buscar empresas clientes por sus datos guardados en la plantilla.

> **Dependencias:** FR9 (busca datos almacenados en la plantilla de clientes).

#### FR11. Organización por etiquetas

Habilitar un sistema de clasificación flexible mediante etiquetas manuales y administrables (ej. estratégico, beta, partner, riesgo de cobro) independientes de los estados financieros derivados del sistema.

> **Dependencias:** FR9 (las etiquetas se asignan a fichas de clientes).

### 3. Facturación y Pagos

#### FR12. Emisión automatizada

Generación automática de facturas (puntuales o recurrentes mensuales), almacenamiento en el sistema y envío directo al cliente por correo electrónico. Generar y guardar un PDF estándar por factura, asociado al cliente, con datos fiscales, IVA, serie, numeración e imagen corporativa.

> **Dependencias:** FR9 (requiere los datos fiscales, comerciales y de contacto del cliente); FR13 (la emisión debe respetar las validaciones y controles previos).

#### FR13. Vista previa y control de errores fiscales

Disponer de una pantalla de vista previa del PDF antes de su emisión definitiva, bloqueando el proceso si faltan datos fiscales críticos del cliente. Bloquear la edición o eliminación de cualquier factura que ya haya sido emitida. Cualquier modificación o devolución posterior deberá realizarse mediante un flujo controlado que genere una Factura Rectificativa vinculada al documento original.

> ** Dependencias:** FR9 (requiere los datos fiscales del cliente para validarlos).

#### FR14. Aviso de anomalía

Emitir una alerta visual cuando el importe total de una factura redactada se desvíe desproporcionadamente del patrón histórico de consumo de ese cliente. También cuando un pago no coincida con el importe acordado, el sistema asociará el movimiento pero mantendrá la factura en un estado de "Diferencia pendiente" a la espera de revisión por el lector.

> ** Dependencias:** FR12 (requiere facturas y patrones históricos); FR17 (requiere movimientos de pago para detectar diferencias).

#### FR15. Nuevos clientes

Generar la primera cuota en la fecha de alta de forma proporcional hasta final de mes y las siguientes el día 1.

> **Dependencias:** FR9 (requiere la fecha de alta y tarifa acordada); FR12 (genera la cuota inicial y las siguientes facturas).

#### FR16. Creación manual

Permiso (concedido por los administradores) para que los lectores creen facturas de forma manual.

> **Dependencias:** FR1 (requiere autenticación); FR2 (requiere identificar el rol y sus permisos); FR12 (habilita la creación manual de facturas).

#### FR17. Conciliación Externa

Consulta automática de fuentes externas, principalmente Stripe. Las deudas no se cancelan en cascada (un pago posterior no liquida deudas anteriores).

> **Dependencias:** FR12 (requiere facturas emitidas para asociar y conciliar movimientos).

### 4. Notificaciones, Filtros e Informes

#### FR18. Histórico de pagados

Registro del resultado de los envíos de comunicaciones, confirmación automática de cobro al cliente y conservación del histórico tras la baja.

> **Dependencias:** FR12 (requiere registrar los envíos de facturas); FR17 (requiere registrar la confirmación de cobros).

#### FR19. Recordatorios

Enviar recordatorios amigables por defecto los días 3 y 5 desde la emisión y después cada dos o tres días (esto es totalmente configurable por el superadministrador), incluso tras comunicar una baja, deteniéndose al detectar, confirmar el pago de esa factura o por el administrador.

> **Dependencias:** FR12 (los recordatorios se programan a partir de la emisión); FR17 (el flujo se detiene al detectar el pago); FR21 (debe respetar la pausa de seguimiento); FR9 (debe considerar las condiciones de pago del cliente).

#### FR20. Notificación fallida

Si por correo se notifica un fallo de entrega, el sistema abrirá una incidencia automática asociada a la factura, no la marcará como notificada y alertará al responsable asignado para su corrección y reenvío.

> **Dependencias:** FR12 (requiere el envío de la factura); FR18 (registra el resultado del envío).

#### FR21. Gestión de facturas en disputa

Permitir suspender temporalmente el envío de recordatorios automáticos de una factura concreta cambiándola al estado "Seguimiento pausado", exigiendo un motivo interno y una fecha límite de revisión administrativa.

> **Dependencias:** FR19 (suspende temporalmente los recordatorios).

#### FR22. Bandeja de entrada del lector

Crear una pantalla de trabajo diario que muestre a los usuarios de forma priorizada sus tareas activas: aprobaciones pendientes, deudas que vencen pronto, incidencias de entrega de correos, revisiones anuales y pagos por conciliar. Añadir notas aclaratorias dentro de las fichas de clientes o facturas, además de menciones a otros usuarios para que reciban notificaciones.

> **Dependencias:** FR17 (pagos por conciliar); FR19 (deudas y vencimientos); FR20 (incidencias de entrega); FR21 (revisiones de seguimiento); FR23 (aprobaciones); FR24 (revisiones anuales).

#### FR23. Aprobación de administrador

Retener en estado "Borrador" y requerir la aprobación digital de un Administrador para cualquier factura puntual con importes inusuales, modificaciones manuales de precios de suscripción, descuentos extraordinarios o alteraciones de vencimientos fuera de la política general.

> **Dependencias:** FR2 (requiere identificar solicitantes y administradores autorizados); FR12 (la aprobación controla la emisión); FR16 (incluye facturas puntuales creadas manualmente).

#### FR24. Módulo de informes

Mostrar un panel global exclusivo para administrador y superadministrador con indicadores de facturado, cobrado y pendiente agrupados por semana, mes, trimestre y año. Además se generarán alertas al año de darse de alta para listar los clientes que requieren una revisión de su rango de facturación anual. El sistema calculará la propuesta basada en una fuente simulada y el administrador decidirá su aplicación para futuros pagos.

> **Dependencias:** FR12 (requiere datos de facturación); FR17 (requiere datos de cobro y conciliación); FR9 (requiere la fecha de alta y la tarifa del cliente para la revisión anual).

#### FR25. Revisión de baja

Exponer una API con acceso controlado para que el proceso de baja consulte las facturas pendientes de un cliente.

> **Dependencias:** FR9 (requiere identificar al cliente); FR12 (requiere consultar sus facturas); FR17 (requiere conocer su estado de pago).

#### FR26. Calendario

Mostrar un calendario visual de la programación de facturas, con visión de clientes asignados para el lector y global para administrador y superadministrador.

> **Dependencias:** FR9 (requiere los datos del cliente y las asignaciones de acceso); FR12 (muestra la programación de facturas); FR15 (usa la programación de cuotas recurrentes).

#### FR27. Redacción automatizada de reclamaciones

Generar correos electrónicos de recordatorio de pago con un tono adaptado (amistoso, formal, urgente) según los días de retraso y el perfil del cliente.

> **Dependencias:** FR19 (genera el contenido de los recordatorios); FR9 (usa el perfil del cliente para adaptar el tono).

#### FR28. Importación y exportación avanzada

Permitir la importación masiva de clientes mediante archivos CSV ,que incluya una pantalla de previsualización para el mapeo de columnas y el filtrado de archivos corruptos, y facturas antiguas. Permitir la generación y descarga trimestral o mensual de un paquete ZIP estructurado con los listados CSV y PDFs de las facturas emitidas y rectificativas del periodo.

> **Dependencias:** FR9 (importa y exporta datos de clientes); FR12 (exporta facturas emitidas); FR13 (incluye facturas rectificativas vinculadas).

---

## Requisitos no Funcionales Generalizados:

### 1. Rendimiento y Eficiencia

#### NFR1. Velocidad de facturación

Descripción: El proceso completo de generación, guardado y envío de una factura debe realizarse en menos de 1 minuto.

- **Métrica:** Tiempo total inferior a 60 segundos en cada ejecución, medido desde que se solicita generar la factura hasta que el PDF queda guardado y el proveedor del canal confirma la aceptación del envío
- **Método de verificación:** Ejecutar pruebas de extremo a extremo con facturas puntuales y recurrentes, registrar las marcas temporales de inicio, almacenamiento y aceptación del envío y comprobar que cada ejecución cumple el límite.

#### NFR2. Eficiencia del sistema

- **Descripción:** El procesamiento de datos y la carga de pantallas deben estar optimizados para evitar demoras, priorizando la agilidad en la gestión diaria.
- **Métrica:** El percentil 95 del tiempo hasta que una pantalla sea utilizable ≤ 2 segundos y de las operaciones habituales de consulta o modificación ≤ 3 segundos.
- **Método de verificación:** Medir los tiempos de navegación y operaciones habituales con herramientas del navegador y pruebas de carga mediante k6 o JMeter; calcular el percentil 95 y contrastarlo con los umbrales en el entorno acordado.

#### NFR3. Procesamiento asíncrono en segundo plano

- **Descripción:** Las tareas masivas de generación de facturas recurrentes o procesamiento de informes de gran volumen programadas para el día 1 de cada mes deben ejecutarse de forma asíncrona mediante colas de trabajo en segundo plano para no congelar la interfaz de usuario web.
- **Métrica:** 0% de impacto en la navegación web (carga ≤ 2 segundos) y 0 caídas del lote masivo, garantizando que el 100% de los errores individuales queden aislados y registrados en el log sin detener el resto del proceso.
- **Método de validación:** Disparar la generación masiva del Día 1 con un lote que incluyan facturas con datos rotos deliberadamente y verificar con herramientas de carga que las pantallas web responden con fluidez, las facturas válidas se emiten con éxito y las erróneas quedan correctamente identificadas en el registro de salud.

### 2. Usabilidad y Accesibilidad

#### NFR4. Diseño intuitivo e idiomas

- **Descripción:** La interfaz debe ser lo suficientemente fácil de usar como para permitir la creación de una ficha de cliente o factura de forma rápida y sin necesidad de formación compleja. Debe estar integrada perfectamente en inglés y español.
- **Métrica:** Al menos el 90 % de las tareas de crear un cliente y crear una factura se completan correctamente y en poco tiempo, por usuarios nuevos tanto en español como en inglés tras una introducción de como máximo cinco minutos.
- **Método de verificación:** Realizar una prueba de usabilidad con usuarios de distintos idiomas representativos sin experiencia previa, proporcionar la misma introducción y registrar tareas completadas, solicitudes de ayuda, errores y tiempos de ejecución.

#### NFR5. Compatibilidad multidispositivo

- **Descripción:** La aplicación debe ser accesible mediante etiquetas, contraste, teclado y mensajes de error comprensibles desde cualquier dispositivo (ordenadores, tablets y móviles) y a través de un navegador web.
- **Métrica:** El 100 % de los flujos esenciales de acceso, gestión de clientes y generación, envío y consulta de facturas funciona en ordenadores, tablets y móviles, sin controles inaccesibles ni contenido esencial oculto; matriz de sistemas, navegadores y tamaños de pantalla por acordar.
- **Método de verificación:** Ejecutar los mismos flujos en dispositivos reales y emuladores de las tres categorías, utilizando distintos tamaños y orientaciones, y comprobar visualmente la interfaz y funcionalmente sus controles en cada combinación de la matriz.

### 3. Escalabilidad y Concurrencia

#### NFR6. Capacidad de crecimiento

- **Descripción:** La arquitectura del sistema debe estar preparada para escalar a miles de clientes y facturas sin perder rendimiento.
- **Métrica:** Volumen de referencia propuesto para aprobación del equipo: 2.000 clientes y 15.000 facturas; con esa carga de datos, las tareas deben seguir cumpliendo los límites de NFR1 y NFR2, sin errores de procesamiento ni pérdida de información. Con esa carga de datos, las tareas deben seguir cumpliendo los límites de NFR1 y NFR2, sin errores de procesamiento ni pérdida de información.
- **Método de verificación:** Llenar el sistema con datos sintéticos hasta el volumen acordado y repetir las pruebas de facturación, búsquedas, filtros e histórico en el mismo entorno; comparar los resultados con una carga de datos menor y comprobar tiempos, errores e integridad.

#### NFR7. Soporte de concurrencia

- **Descripción:** El sistema debe soportar el inicio de sesión simultáneo de todos los usuarios, administradores y el superadministrador sin caídas ni ralentizaciones.
- **Métrica:** El 100 % de los inicios de sesión con credenciales válidas se completa correctamente, con cero caídas o errores del servidor, cuando acceden simultáneamente N cuentas, siendo N el total de cuentas internas activas; límite propuesto de tiempo de autenticación: percentil 95 ≤ 2 segundos.
- **Método de verificación:** Lanzar una prueba de acceso simultáneo con k6 o JMeter usando cuentas distintas de todos los roles, verificar la sesión y el rol resultante de cada cuenta y registrar tiempos, errores y disponibilidad del servidor.

#### NFR8. Control de colisiones concurrentes y bloqueo

- **Descripción:** Para evitar que la edición simultánea de una misma factura o cliente por dos empleados destruya información, el backend implementará un mecanismo de control de concurrencia, rechazando el segundo guardado e informando al usuario del conflicto.
- **Métrica:** Procesos de edición correctos a pesar de la interacción de varios usuarios a la vez mostrando una alerta clara sobre el conflicto al usuario bloqueado.
- **Método de verificación:** Simular con una herramienta de automatización el guardado concurrente de la misma factura por dos usuarios con milisegundos de diferencia, comprobando que el primer cambio se almacena con éxito y el segundo es rechazado de forma segura notificando la colisión.

#### NFR9. Precisión Matemática Financiera

- **Descripción:** Garantizar que los cálculos y totales del sistema sean exactos y deterministas, prohibiendo estrictamente el uso de tipos de coma (double o float) para el dinero. Se emplearán tipos numéricos de precisión fija (Decimal).
- **Métrica:** Desviación monetaria exactamente igual a 0.00 euros en el 100% de los cálculos del sistema, garantizando que la suma de las líneas de detalle coincida de forma precisa con el total del PDF, el almacenamiento y los paneles financieros agregados.
- **Método de verificación:** Ejecutar pruebas unitarias automatizadas inyectando líneas de factura con múltiples decimales e IVA combinados, verificar en el código y en el esquema de la base de datos que se usan tipos de precisión fija (Decimal / enteros), y comprobar que las agregaciones del dashboard no sufren pérdidas de céntimos.

### 4. Seguridad, Privacidad y Arquitectura

#### NFR10. Privacidad por diseño

- **Descripción:** El sistema debe garantizar la protección y confidencialidad de los datos almacenados, cumpliendo con las normativas vigentes (como RGPD para el borrado lógico).
- **Métrica:** Cero accesos o exposiciones de datos personales y fiscales a usuarios no autorizados en los casos de prueba y cumplimiento del 100 % de los controles aplicables de la lista de revisión normativa, incluida la conservación, el borrado lógico y la eliminación definitiva que correspondan; lista validada por el responsable competente.
- **Método de verificación:** Ejecutar pruebas de acceso no autorizado y de separación de permisos sobre pantallas, API y documentos; inspeccionar código, configuración, registros y tratamiento de datos; contrastar los resultados con la lista normativa validada, sin asumir que el borrado lógico por sí solo acredita el cumplimiento.

#### NFR11. Independencia tecnológica

- **Descripción:** El software operará con plena autonomía respecto a la plataforma Brain, desplegándose sobre bases de datos e infraestructura de servidores totalmente desvinculados.
- **Métrica:** Ausencia total (0%) de acoplamiento operativo con Brain para la facturación, manteniendo un entorno persistente separado y garantizando que el 100 % de los procesos clave se ejecuten sin interrupciones aun con dicho sistema inactivo.
- **Método de verificación:** Auditar la topología de red, esquemas de datos y endpoints; simular una caída completa de Brain y ejecutar el flujo completo de emisión para constatar que no se efectúan peticiones ni dependencias críticas hacia esa infraestructura.

#### NFR12. Consistencia y fiabilidad

- **Descripción:** mantener el mismo PDF en almacenamiento, envío y descarga, dirigir los avisos al destinatario correcto y prevenir emisiones o avisos duplicados involuntarios.
- **Métrica:** Igualdad del hash SHA-256 del PDF almacenado, enviado y descargado en el 100 % de las facturas probadas; destinatario correcto en el 100 % de los avisos y cero duplicados involuntarios de una misma emisión o aviso programado, incluso ante reintentos.
- **Método de verificación:** Ejecutar pruebas de integración con captura de comunicaciones, comparar los hashes de los PDF y los destinatarios esperados e introducir reintentos, fallos y ejecuciones simultáneas para comprobar que cada emisión o aviso se produce una sola vez; distinguir los recordatorios legítimos de días diferentes.

#### NFR13. Inmutabilidad de la Auditoría Corporativa

- **Descripción:** Los registros de auditoría que guardan quién creó un cliente, modificó datos fiscales o aprobó descuentos se almacenarán en estructuras de "sólo inserción" . Ningún usuario, incluido el Superadministrador, podrá editar o eliminar este histórico.
- **Métrica:** Todos los eventos críticos registrados con marcas de tiempo e identificadores inmutables, y no hay registros de auditoría borrados o modificados por cualquier rol del sistema, incluido el Superadministrador.
- **Método de verificación:** Intentar forzar la edición o eliminación de un registro de auditoría mediante scripts directos a la base de datos o desde el panel de control con el rol de Superadministrador, verificando que el backend rechaza la operación de forma segura y bloquea cualquier intento de alteración histórica.

#### NFR14. Protección de Secretos y Credenciales

- **Descripción:** Queda estrictamente prohibido almacenar claves API, contraseñas o secretos de la simulación de Stripe dentro del repositorio de código fuente. Se gestionan obligatoriamente de forma cifrada.
- **Métrica:** Las credenciales, tokens de API o contraseñas guardadas de forma fija en el repositorio de código fuente, y los secretos se mantienen cifrados.
- **Método de verificación:** Ejecutar un script de análisis estático de código o una inspección visual completa del repositorio buscando patrones de texto confidenciales, y verificar que los archivos de configuración utilizan algún tipo de cifrado.

#### NFR15. Gestión de Backups Probados

- **Descripción:** Implementar copias de seguridad automáticas y periódicas de los datos y documentos almacenados. La política de la infraestructura exigirá realizar pruebas periódicas documentadas de restauración completa de los backups en entornos aislados para validar su integridad y éxito.
- **Métrica:** Todas las copias de seguridad automáticas generadas según el calendario previsto y la cantidad de errores críticos de integridad detectados durante el proceso obligatorio de restauración periódica en el entorno de pruebas.
- **Método de verificación:** Programar la ejecución automática del backup del sistema, extraer el archivo resultante y ejecutar un script de restauración completa sobre un entorno de base de datos aislado, comprobando mediante consultas de prueba que el 100% de los datos y documentos maestros históricos se recuperan sin corrupción ni pérdida de información.



