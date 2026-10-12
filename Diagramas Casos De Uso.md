#Esbozos previos a la definición de los casos de uso

#---------------------------------------------------------------

#Diagrama 1: Cobrar una factura
@startuml

:Super Administrador: as superadmin
:Administrador: as admin
:Cliente: as client
superadmin <|-- admin

(Autenticación) as login 
(Gestionar Cliente) as gestionarCliente


gestionarCliente ..> login : <<include>>
admin -- gestionarCliente
gestionarCliente -- client


(Dar de Alta) as registerClient
admin -- registerClient
registerClient ..> login : <<include>>
registerClient -- client


(Solicitar Cobro) as cobrar
:Sistema de Pago: as sistemaDePago
sistemaDePago -- cobrar
(Generar Factura) as generarFactura
cobrar ..> generarFactura
cobrar ..> login : <<include>>
(Enviar Factura) as enviarFactura
generarFactura ..> enviarFactura
generarFactura ..> (Generar Respaldo)
enviarFactura -- client


(Modificar Factura) as modFactura
admin -- modFactura
modFactura ..> login : <<include>>
@enduml

#---------------------------------------------------------------

#Diagrama 2: Pagar una factura
@startuml

:Super Administrador: as superadmin
:Administrador: as admin
:Cliente: as client
superadmin <|-- admin

(Autenticación) as login 
(Gestionar Cliente) as gestionarCliente


gestionarCliente ..> login : <<include>>
admin -- gestionarCliente
gestionarCliente -- client


(Dar de Alta) as registerClient
admin -- registerClient
registerClient ..> login : <<include>>
registerClient -- client



(Emitir Pago) as pagar
:Sistema de Pago: as sistemaDePago
(Generar Factura) as generarFactura
client ..> pagar
pagar ..> generarFactura
pagar ..> login : <<include>>
(Enviar Factura) as enviarFactura
generarFactura ..> enviarFactura
enviarFactura -- sistemaDePago

@enduml

#---------------------------------------------------------------

#Diagrama 3: Notificar Impago / Dar de baja

@startuml

:Super Administrador: as superadmin
:Administrador: as admin
:Cliente: as client
superadmin <|-- admin

(Autenticación) as login 
(Gestionar Cliente) as gestionarCliente


gestionarCliente ..> login : <<include>>
admin -- gestionarCliente
gestionarCliente -- client


(Dar de Alta) as registerClient
admin -- registerClient
registerClient ..> login : <<include>>
registerClient -- client


(Comprobar impagos) as comprobarImpagos
(Notificar impagos) as notificarImpagos
(Solicitar darse de baja) as borrarCuenta
borrarCuenta ..> login : <<include>>
client -- borrarCuenta
borrarCuenta ..> comprobarImpagos : <<dependency>>
comprobarImpagos ..> notificarImpagos : <<include>>
notificarImpagos -- client

@enduml

#---------------------------------------------------------------

#Diagrama 4: Consultar una factura
@startuml

:Super Administrador: as superadmin
:Administrador: as admin
:Cliente: as client
superadmin <|-- admin

(Autenticación) as login 
(Gestionar Cliente) as gestionarCliente


gestionarCliente ..> login : <<include>>
admin -- gestionarCliente
gestionarCliente -- client


(Dar de Alta) as registerClient
admin -- registerClient
registerClient ..> login : <<include>>
registerClient -- client


(Consultar una Factura) as consultar
consultar ..> login : <<include>>
client -- consultar
consultar ..> (Generar una Factura) : <<include>>


(Filtrar Facturas) as filtrar
admin -- filtrar
filtrar ..> login : <<include>>
filtrar ..> (Generar una Factura) : <<include>>
@enduml

#---------------------------------------------------------------

#Diagrama 5: Editar Factura

@startuml

:Super Administrador: as superadmin
:Administrador: as admin
:Cliente: as client
superadmin <|-- admin

(Autenticación) as login 
(Gestionar Cliente) as gestionarCliente


gestionarCliente ..> login : <<include>>
admin -- gestionarCliente
gestionarCliente -- client


(Dar de Alta) as registerClient
admin -- registerClient
registerClient ..> login : <<include>>
registerClient -- client


(Editar una Factura) as edit
edit ..> login : <<include>>
admin -- edit
edit ..> (Generar una Factura) : <<include>>

@enduml

#---------------------------------------------------------------

#Diagrama 6: Gestionar Excepciones
@startuml

:Super Administrador: as superadmin
:Administrador: as admin
:Cliente: as client
superadmin <|-- admin

(Autenticación) as login 
(Gestionar Cliente) as gestionarCliente


gestionarCliente ..> login : <<include>>
admin -- gestionarCliente
gestionarCliente -- client


(Dar de Alta) as registerClient
admin -- registerClient
registerClient ..> login : <<include>>
registerClient -- client


(Solicitar excepción financiera) as solicitarExcepcion
solicitarExcepcion ..> login : <<include>>
client -- solicitarExcepcion
(Gestionar Excepción) as gestionarExcepcion
gestionarExcepcion --> login : <<include>>
admin -- gestionarExcepcion
solicitarExcepcion ..> gestionarExcepcion : <<dependency>
(Aplicar Excepción) as aplicarExcepcion
gestionarExcepcion ..> aplicarExcepcion
(Generar Factura Excepcional) as generarFacturaExcepcional
aplicarExcepcion ..> generarFacturaExcepcional
(Aprobar Excepción) as aprobarExcepcion
generarFacturaExcepcional ..> aprobarExcepcion : <<dependency>>
admin -- aprobarExcepcion
(Enviar Factura) as enviarFactura
aprobarExcepcion ..> enviarFactura
enviarFactura -- client

@enduml
