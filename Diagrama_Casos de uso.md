#Esbozo previo a la definición de los casos de uso

@startuml

:Super Administrador: as superadmin
:Administrador: as admin
:Usuario: as user
:Lector: as lector

(Autenticicación) as auth
(Registrar Usuario) as register
(Dar de baja) as darDeBaja

(Crear rol) as crearRol
(Asignar rol) as asignarRol

superadmin-->asignarRol
superadmin-->crearRol

(Busqueda) as busqueda

(Cobrar) as cobrar
(Consultar factura) as consultarFact
(Generar factura) as generarFactura
(Editar factura) as editarFactura
(Enviar factura) as enviarFactura

(Notificar) as notificar


@enduml
