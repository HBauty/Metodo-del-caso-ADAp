@startuml
actor Administrador as Admin
Admin -> Administrador: revisionAnual(cliente: Cliente)
alt SE APRUEBA LA REVISION
Admin -> Administrador: aprobarRevision(cliente: Cliente, nuevaTarifa: Decimal)
Administrador -> Cliente: actualizarTarifa()
@enduml
