# Negociación y discusión de requisitos
Durante la recopilación y análisis de requisitos del usuario, se identificaron los principales puntos a discutir entre el cliente y el equipo de desarrollo. A continuación se detallan los principales conflictos y acuerdos alcanzados:
1. Generación de boleta y validación con SUNAT
   - Cliente: Desea que el sistema genere boletas y facturas listas, si es posible conectadas a SUNAT.
   - Equipo de desarrollo: Advirtió que la integración directa del sistema con SUNAT representa una carga de trabajo que excede a su capacidad.
   - Acuerdo: En una primera versión, el sistema generará boletas y facturas en un formato listas para imprimir sin integración directa con SUNAT.
2. Gestión de stock y modificaciones en la boleta de venta
   - Cliente: Quiere registrar compras, ventas y poder modificar campos al momento de generar la boleta como descripción, precio, cantidad, etc.
   - Equipo de desarrollo: Señaló que es viable poder modificar algunos campos de los productos y servicios en la boleta de venta.
   - Acuerdo: Se podrá modificar campos como descripción, precio y cantidad que estén definidos por defecto en el sistema al momento de agregarlos a la boleta de venta, a excepción del tipo de producto o servicio.
4. Ingreso de productos y servicios
   - Cliente: Desea registrar productos y servicios con múltiples características detalladas y con posibilidad de editarlas, además solicitó que el sistema ya cuente con los productos y servicios que ofrece.
   - Equipo de desarrollo: Señaló que es necesario conocer la cantidad de productos y servicios que el negocio ofrece para registrarlos en la versión inicial del sistema, además de otros detalles.
   - Acuerdo: El cliente brindará al equipo de desarrollo un documento donde estén detallados todos los productos y servicios para su registro en el sistema.


# Tips para mayor claridad
## Propósito
Definir los requisitos de usuario que han generado que el ED haya tenido que negociar, consensuar con el cliente para llegar a una resolución o acuerdo.
En el caso de los ED que no cuenten con cliente real entonces describiran los requisitos que internamente para el ED han tenido un costo significativo de consenso.

## Qué se espera
- Que al finalizar la implementación se haga una revaluación de estos requisitos que fueron inicialmente conflictivos.
