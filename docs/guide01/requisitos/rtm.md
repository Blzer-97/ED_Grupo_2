# Especificación de requisitos de software

## Requisitos funcionales
### RF001 - Gestión de Productos
#### 1. Función registrarProducto:
- Entrada de datos: nombre, SKU (clave única), categoría, stock inicial, precio costo, precio venta.
- Validación de campos obligatorios y esquema (SKU único, precios positivos).
#### 2. Función modificarProducto:
- Modificación de cualquier campo editable (nombre, categoría, precios).
- Control para evitar incosistencias en ventas como cotización.
#### 3. Función eliminarProducto:
- Eliminación lógica (desactivación) permitida solo si no existen ventas o movimientos de inventario relacionados por confirmar.
- Confirmar nuevamente al usuario antes de ejecutar la eliminación.
#### 4. Función consultarProducto:
- Visualización de información del producto, su stock actual.

```
FUNCIÓN registrarProducto(nombre, sku, categoria, stock, costo, venta)
    SI sku ya existe ENTONCES error
    producto = crear(nombre, sku, categoria, stock, costo, venta)
    guardar(producto)
FIN

FUNCIÓN modificarProducto(id, nuevosDatos)
    producto = buscar(id)
    actualizar(producto, nuevosDatos)
FIN

FUNCIÓN eliminarProducto(id)
    SI confirmar() ENTONCES
        producto = buscar(id)
        producto.activo = false
    FIN SI
FIN

FUNCIÓN consultarProductos()
    lista = obtenerProductos()
    RETORNAR lista
FIN
```

### RF002 - Gestión de Inventario
#### 1. Función registrarEntrada:
- Registrar una entrada de inventario (compra u otro tipo de ingreso).
- Actualiza el stock sumando la cantidad.
#### 2. Función registrarSalida:
- Registrar una salida de inventario (venta o retiro).
- Validar que stock actual sea suficiente para la salida.
- Restar cantidad al stock.
#### 3. Función actualizarStock:
- Función interna par ajustar stock según operacion (entrada o salida).
#### 4. Función consultarHistorial:
- Mostrar movimientos de inventario guardados para un producto, con filtros opcionales.
```
FUNCIÓN registrarEntrada(producto, cantidad)
    stockAnterior = producto.stock
    producto.stock = producto.stock + cantidad
    guardarMovimiento("ENTRADA", producto, cantidad)
FIN

FUNCIÓN registrarSalida(producto, cantidad)
    SI producto.stock >= cantidad ENTONCES
        producto.stock = producto.stock - cantidad
        guardarMovimiento("SALIDA", producto, cantidad)
    SINO
        error("Stock insuficiente")
    FIN SI
FIN

FUNCIÓN consultarHistorial(producto)
    movimientos = obtenerMovimientos(producto)
    RETORNAR movimientos
FIN
```
### RF003 - Gestión de Ventas
#### 1. Función registrarVenta:
- Registrar venta con lista de productos, cantidades, precio unitario, etc.
- Para producto: Editar el precio de venta unitario predeterminado de las líneas de orden de venta.
- Calcular subtotal, aplicar descuentos/impuestos y calcular total, considerar el IGV configurable.
#### 2. Función generarBoleta:
- Crear documento PDF con los datos completos de la venta (productos, cantidad, subtotal, IGV, total).
- Opciones para guardar boleta en la computadora.
#### 3. Función verRentabilidad:
- Calcular margen de ganancia para cada producto vendido: (precioVenta - precioCosto) * cantidad.
- Sumar margen total de la venta para análisis.
- Periodo configurable para ver la rentabilidad
```
FUNCIÓN registrarVenta(items, cliente)
    total = 0
    PARA CADA item EN items
        SI item.esProducto ENTONCES
            registrarSalida(item.producto, item.cantidad)
        FIN SI
        subtotal = item.cantidad * item.precio
        total = total + subtotal
    FIN PARA
    
    venta = crearVenta(items, total, cliente)
    guardar(venta)
FIN

FUNCIÓN generarBoleta(venta)
    pdf = crearPDF(venta)
    RETORNAR pdf
FIN

FUNCION verRentabilidad(fechaInicio, fechaFin)
    listaVentas = obtenerVentasPorFechas(fechaInicio, fechaFin)    
    margenTotalAcumulado = 0
    listaDetalles = []

    PARA CADA item EN listaVentas HACER
        margenIndividual = (item.precioVenta - item.precioCosto) * item.cantidad
        margenTotalAcumulado = margenTotalAcumulado + margenIndividual
        AGREGAR margenIndividual A listaDetalles
    FIN PARA

    RETORNAR { 
        rentabilidadTotal: margenTotalAcumulado, 
        detalle: listaDetalles 
    }
FIN
```
### RF004 - Gestión de Servicios
#### 1. Función registrarServicio:
- Añadir nuevo servicio al catálogo local (nombre, precio predeterminado).
#### 2. Función modificarServicio:
- Modificar el nombre y precio predeterminado de un servicio existente.
#### 3. Función eliminarServicio:
- Borrar un servicio existente.
#### 4. Función consultarServicio:
- Listar todos los servicios activos con precios.
```
FUNCIÓN registrarServicio(nombre, precio)
    servicio = crear(nombre, precio)
    guardar(servicio)
FIN

FUNCIÓN modificarServicio(id, nuevoPrecio)
    servicio = buscar(id)
    servicio.precio = nuevoPrecio
FIN

FUNCIÓN eliminarServicio(id)
    servicio = buscar(id)
    servicio.activo = false
FIN

FUNCIÓN consultarServicios()
    lista = obtenerServicios()
    RETORNAR lista
FIN
```

### RF005 - Creación de Reportes
#### 1. Función obtenerProductoVendidos:
- Calcular y mostrar cantidad total vendida.
- Permitir comparación entre varios productos relevantes.
#### 2. Función compararGanancias:
- Extraer ganancias netas.
- Mostrar tabla comparativa.

```
FUNCION obtenerProductoVendidos()
    cantidadTotal = sumarTodasLasVentas()
    listaComparativa = obtenerProductosRelevantes()
    RETORNAR { 
        total: cantidadTotal, 
        productos: listaComparativa 
    }
FIN
FUNCION compararGanancias()
    ingresos = obtenerTotalIngresos()
    gastos = obtenerTotalGastos()
    gananciaNeta = ingresos - gastos
    datosTabla = crearTablaDesglose(ingresos, gastos, gananciaNeta)

    RETORNAR { 
        neto: gananciaNeta, 
        tabla: datosTabla 
    }
FIN
```
## Requisitos no funcionales
### Portabilidad del software:
El sistema debe ser compatible con Windows para permitir su ejecución local en PCs diversas, sin dependencias externas difíciles de manejar.
### Facilidad de mantenimiento:
El software debe estar desarrollado con un código claro y modular, principalmente se busca comentarios para que sea escalable a futuro.
### Usabilidad del software:
La interfaz de usuario debe ser intuitiva y simple, requisito mencionado por el cliente, debe contar con flujos de trabajo simples y consistentes. Además, no debe contar con inicio de sesión a petición del cliente (única persona en usar el sistema).
### Velocidad de procesamiento de datos:
El sistema debe procesar y actualizar la información en tiempos mínimos aceptables, por la naturaleza de un único taller mecánico, no existen cantidades masivas de datos.
### Restricciones técnicas del software:
- El software debe funcionar completamente en almacenamiento local, sin depender de Internet.
- No deben utilizarse tecnologías o librerías no compatibles con las plataformas objetivo.
