## Pseudocódigo de los requisitos de sistema más importantes (Requisitos funcionales mandatorios)

## 1) Ingreso de productos 

<pre> ```
Producto:
    nombre
    sku
    tipo          
    costo
    precio
    stock
    caracteristicas
    activo

Funcion RegistrarProducto(datos):
    Si datos.nombre está vacío o datos.sku está vacío:
        devolver "Error: faltan datos"
    Si existe un producto con ese sku:
        devolver "Error: SKU duplicado"
    Si datos.tipo no es "AUTOPARTE" ni "SERVICIO":
        devolver "Error: tipo inválido"
    Si datos.tipo = "SERVICIO":
        datos.stock = 0        
        Si datos.costo es vacío: datos.costo = 0
    Si datos.precio <= 0:
        devolver "Error: precio inválido"
    producto = nuevo Producto con datos
    producto.activo = verdadero
    guardar producto
    devolver "OK"
Fin
``` </pre>

## 2) Registro de movimientos 

Movimiento:
    tipo          
    fecha
    items         
    total
    estado         
    observaciones


Funcion RegistrarCompra(items):
    total = 0
    Para cada item en items:
        prod = buscar producto por sku
        Si no existe o no está activo: devolver "Error: SKU inválido"
        Si prod.tipo = "AUTOPARTE":
            Si item.cantidad <= 0 o item.costoUnit <= 0: devolver "Error: datos inválidos"
            prod.stock = prod.stock + item.cantidad
        total = total + (item.cantidad * valor(item.costoUnit, 0))
        actualizar prod
    Fin
    guardar movimiento "COMPRA" con items y total
    devolver "OK"
Fin

Funcion RegistrarVenta(items):
    total = 0
    Para cada item en items:
        prod = buscar producto por sku
        Si no existe o no está activo: devolver "Error: SKU inválido"
        Si item.cantidad <= 0 o item.precioUnit <= 0: devolver "Error: datos inválidos"
        Si prod.tipo = "AUTOPARTE":
            Si prod.stock < item.cantidad: devolver "Error: stock insuficiente"
            prod.stock = prod.stock - item.cantidad
            actualizar prod
        total = total + (item.cantidad * item.precioUnit)
    Fin
    guardar movimiento "VENTA" con items y total
    devolver "OK"
Fin

Funcion ModificarMovimiento(idMovimiento, nuevoTipo, nuevosItems):
    mov = buscar movimiento por id
    Si no existe o mov.estado != "ACTIVO": devolver "No modificable"
    Para cada item en mov.items:
        prod = buscar producto por id de item
        Si mov.tipo = "COMPRA" y prod.tipo = "AUTOPARTE":
            prod.stock = prod.stock - item.cantidad
            actualizar prod
        Si mov.tipo = "VENTA" y prod.tipo = "AUTOPARTE":
            prod.stock = prod.stock + item.cantidad
            actualizar prod
    Fin
    mov.estado = "ANULADO"
    actualizar mov
    Si nuevoTipo = "COMPRA":
        devolver RegistrarCompra(nuevosItems)
    Si nuevoTipo = "VENTA":
        devolver RegistrarVenta(nuevosItems)
    devolver "Error: tipo inválido"
Fin
