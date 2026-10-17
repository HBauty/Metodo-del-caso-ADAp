@startuml
left to right direction

actor "Lector" as lector
actor "Administrador" as admin
actor "Superadministrador" as superadmin

actor "Usuario" as user

superadmin --|> admin
admin --|> user
lector --|> user

(CU-00\n Registrarse en el sistema) as CU00
(CU-01\n Iniciar Sesión) as CU01
(CU-02\n Asignar roles y permisos) as CU02
(CU-03\n Pausar Suscripción) as CU03
(CU-04\n Tramitar baja de cliente) as CU04
(CU-05\n Notificar avisos e impagos) as CU05
(CU-06\n Consultar y gestionar fichas de clientes e historial) as CU06
(CU-07\n Registrar ficha de suscripción - Acuerdo Comercial) as CU07
(CU-08\n Emitir orden de cobro / Envío de factura automática) as CU08
(CU-09\n Verificar y conciliar cobros) as CU09
(CU-10\n Generar factura manual - puntual) as CU10
(CU-11\n Modificar Borrador de factura / Aprobar excepción) as CU11
(CU-12\n Emitir y enviar una factura o factura rectificativa) as CU12
(CU-13\n Actualizar la tarifa según los cobros anuales) as CU13
(CU-14\n Aprobar una excepción financiera) as CU14
(CU-15\n Generar un comprobante de una operación) as CU15
(CU-16\n Consultar una factura y obtener su PDF) as CU16
(CU-17\n Gestionar incidencias de entregta de correo) as CU17

user -- CU00
CU00 <.. CU01 : <<dependency>>

CU01 <.. CU02 : <<include>>
CU01 <.. CU06 : <<include>>
CU01 <.. CU17 : <<include>>
CU01 <.. CU13 : <<include>>
CU01 <.. CU03 : <<include>>
CU01 <.. CU07 : <<include>>
CU01 <.. CU14 : <<include>>
CU01 <.. CU04 : <<include>>
CU01 <.. CU11 : <<include>>
CU01 <.. CU16 : <<include>>

CU02 ..> CU00 : <<dependency>>

admin -- CU06
admin -- CU17
admin -- CU13
CU13 ..> CU07 : <<dependency>>
admin -- CU02
admin -- CU03

lector -- CU03
lector -- CU11
lector -- CU04
lector -- CU16

CU03 ..> CU07 : <<dependency>>

lector -- CU07
CU07 <.. CU08 : <<dependency>>
CU08 ..> CU12 : <<include>>
CU12 <.. CU15 : <<dependency>>
CU12 <.. CU09 : <<dependency>>

lector -- CU14
CU14 ..> CU10 : <<extends>>
CU10 <.. CU11 : <<dependency>>
CU10 ..> CU12 : <<include>>

CU04 <.. CU05 : <<extends>>

CU16 ..> CU12 : <<dependency>>

CU05 ..> CU12 : <<dependency>>

CU13 ..> CU09 : <<dependency>>

CU15 ..> CU09 : <<dependency>>
CU15 ..> CU05 : <<dependency>>

@enduml
