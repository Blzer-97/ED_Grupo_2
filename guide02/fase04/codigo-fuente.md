## Código fuente
Las clases service contienen la lógica de negocio y muestran cómo operan las funciones principales del programa.

### Clase ServicioService
La clase ServicioService gestiona la lógica de negocio relacionada con los objetos Servicio. Obtiene la lista de servicios desde un gestor de datos, permite registrar uno nuevo asignándole un id incremental, actualizar un servicio existente reemplazándolo en la lista y eliminar servicios por su id. Cada operación persiste los cambios mediante GestorDatosJSON, que actúa como capa de almacenamiento.

```
package grupo2.mecanica_ed_02.Service;

import grupo2.mecanica_ed_02.Modelos.Servicio;
import grupo2.mecanica_ed_02.Persistence.GestorDatosJSON;
import java.util.List;

public class ServicioService {

    private final GestorDatosJSON gestorDatos;

    public ServicioService(GestorDatosJSON gestorDatos) {
        this.gestorDatos = gestorDatos;
    }

    public List<Servicio> getServicios() {
        return gestorDatos.leerServicios();
    }

    public Servicio registrarServicio(Servicio servicio) {
        List<Servicio> servicios = getServicios();
        
        // Simular autoincremento
        int maxId = servicios.stream().mapToInt(Servicio::getId).max().orElse(0);
        servicio.setId(maxId + 1);
        
        servicios.add(servicio);
        gestorDatos.guardarServicios(servicios);
        return servicio;
    }

    public Servicio actualizarServicio(Servicio servicioActualizado) {
        List<Servicio> servicios = getServicios();
        for (int i = 0; i < servicios.size(); i++) {
            if (servicios.get(i).getId() == servicioActualizado.getId()) {
                servicios.set(i, servicioActualizado);
                gestorDatos.guardarServicios(servicios);
                return servicioActualizado;
            }
        }
        throw new RuntimeException("Servicio con ID " + servicioActualizado.getId() + " no encontrado.");
    }

    public void eliminarServicio(int id) {
        List<Servicio> servicios = getServicios();
        servicios.removeIf(s -> s.getId() == id);
        gestorDatos.guardarServicios(servicios);
    }
}
```

### Clase InventarioService
La clase InventarioService administra la lógica de negocio relacionada con productos y movimientos de inventario. Permite obtener, registrar, actualizar y eliminar productos almacenados en JSON, validando condiciones como la unicidad del SKU y la existencia del producto antes de modificarlo. También gestiona el registro de movimientos de stock, ajustando el inventario del producto según entradas o salidas y registrando cada movimiento en el historial.

```
package grupo2.mecanica_ed_02.Service;

import grupo2.mecanica_ed_02.Modelos.MovimientoInventario;
import grupo2.mecanica_ed_02.Modelos.Producto;
import grupo2.mecanica_ed_02.Persistence.GestorDatosJSON;
import java.util.List;
import java.util.stream.Collectors;

public class InventarioService {

    private final GestorDatosJSON gestorDatos;

    public InventarioService(GestorDatosJSON gestorDatos) {
        this.gestorDatos = gestorDatos;
    }
    
    public List<Producto> getProductos() {
        return gestorDatos.leerProductos();
    }

    public Producto findProductoBySku(String sku) {
        return getProductos().stream()
                .filter(p -> p.getSku().equalsIgnoreCase(sku))
                .findFirst()
                .orElse(null);
    }

    public Producto registrarProducto(Producto producto) {
        List<Producto> productos = getProductos();

        if (findProductoBySku(producto.getSku()) != null) {
            throw new RuntimeException("Error: El SKU '" + producto.getSku() + "' ya existe.");
        }
        
        productos.add(producto);
        gestorDatos.guardarProductos(productos);
        return producto;
    }

    public Producto actualizarProducto(Producto productoActualizado) {
        List<Producto> productos = getProductos();
        boolean encontrado = false;

        for (int i = 0; i < productos.size(); i++) {
            if (productos.get(i).getSku().equalsIgnoreCase(productoActualizado.getSku())) {
                productos.set(i, productoActualizado);
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new RuntimeException("Error: No se pudo actualizar. Producto con SKU '" + productoActualizado.getSku() + "' no encontrado.");
        }
        
        gestorDatos.guardarProductos(productos);
        return productoActualizado;
    }

    public void eliminarProducto(String sku) {
        List<Producto> productos = getProductos();
        boolean eliminado = productos.removeIf(p -> p.getSku().equalsIgnoreCase(sku));
        
        if (!eliminado) {
             throw new RuntimeException("Error: No se pudo eliminar. Producto con SKU '" + sku + "' no encontrado.");
        }
        
        gestorDatos.guardarProductos(productos);
    }

    public void registrarMovimiento(MovimientoInventario mov) {
        Producto producto = findProductoBySku(mov.getSkuProducto());
        if (producto == null) {
            throw new RuntimeException("Error: Producto con SKU '" + mov.getSkuProducto() + "' no existe.");
        }

        int nuevoStock = producto.getStock() + mov.getCantidad(); // cantidad ya es + o -

        if (nuevoStock < 0) {
            throw new RuntimeException("Stock insuficiente para " + producto.getNombre() + ". Stock actual: " + producto.getStock() + ", se intentó sacar: " + (-mov.getCantidad()));
        }

        producto.setStock(nuevoStock);
        actualizarProducto(producto);

        List<MovimientoInventario> movimientos = gestorDatos.leerMovimientos();
        movimientos.add(mov);
        gestorDatos.guardarMovimientos(movimientos);
    }

    public List<MovimientoInventario> getHistorialPorProducto(String sku) {
        List<MovimientoInventario> todos = gestorDatos.leerMovimientos();
        return todos.stream()
                .filter(m -> m.getSkuProducto().equalsIgnoreCase(sku))
                .collect(Collectors.toList());
    }
}
```

### Clase VentaService
La clase VentaService gestiona el proceso completo de registrar una venta. Asigna un identificador y fecha si no están definidos, calcula los importes de la venta (subtotal, IGV, total y margen de ganancia) usando CalculadoraVentas, descuenta del inventario el stock de los productos incluidos mediante movimientos de salida, y finalmente guarda la venta actualizada en el almacenamiento JSON.

```
package grupo2.mecanica_ed_02.Service;

import grupo2.mecanica_ed_02.Modelos.ItemVenta;
import grupo2.mecanica_ed_02.Modelos.MovimientoInventario;
import grupo2.mecanica_ed_02.Modelos.Venta;
import grupo2.mecanica_ed_02.Persistence.GestorDatosJSON;
import grupo2.mecanica_ed_02.Util.CalculadoraVentas;

import java.util.Date;
import java.util.List;
import java.util.UUID;

public class VentaService {

    private final GestorDatosJSON gestorDatos;
    private final InventarioService inventarioService;
    private final CalculadoraVentas calculadoraVentas;

    public VentaService(GestorDatosJSON gestorDatos, InventarioService inventarioService, CalculadoraVentas calculadoraVentas) {
        this.gestorDatos = gestorDatos;
        this.inventarioService = inventarioService;
        this.calculadoraVentas = calculadoraVentas;
    }

    public Venta registrarVenta(Venta venta) {

        if (venta.getId() == null) {
            venta.setId(UUID.randomUUID().toString());
        }
        if (venta.getFecha() == null) {
            venta.setFecha(new Date());
        }

        double subtotal = calculadoraVentas.calcularSubtotal(venta.getItems());
        double igv = calculadoraVentas.calcularMontoIGV(subtotal);
        double total = calculadoraVentas.calcularTotal(subtotal, igv, venta.getDescuento());
        double ganancia = calculadoraVentas.calcularMargenGanancia(venta);

        venta.setSubtotal(subtotal);
        venta.setIgv(igv);
        venta.setTotal(total);
        venta.setMargenGanancia(ganancia);

        for (ItemVenta item : venta.getItems()) {
            if (item.isEsProducto()) {

                int cantidadSalida = -item.getCantidad(); 
                
                MovimientoInventario mov = new MovimientoInventario(
                    new Date(),
                    item.getSkuOId(),
                    cantidadSalida, // Ej. -2
                    "Salida-Venta",
                    "Venta ID: " + venta.getId()
                );
                
                inventarioService.registrarMovimiento(mov);
            }
        }

        List<Venta> ventas = gestorDatos.leerVentas();
        ventas.add(venta);
        gestorDatos.guardarVentas(ventas);

        return venta;
    }
}
```

### Clase ReporteService
La clase ReporteService centraliza la lógica de negocio necesaria para la generación de reportes financieros, actuando como el componente encargado de procesar los datos de las ventas almacenadas en el sistema.
```
package grupo2.mecanica_ed_02.Service;

import grupo2.mecanica_ed_02.Modelos.Venta;
import grupo2.mecanica_ed_02.Persistence.GestorDatosJSON;
import java.util.List;


public class ReporteService {

    private final GestorDatosJSON gestorDatos;

    public ReporteService(GestorDatosJSON gestorDatos) {
        this.gestorDatos = gestorDatos;
    }

    public double calcularTotalVendido(List<Venta> ventas) {
        return ventas.stream()
                .mapToDouble(Venta::getTotal)
                .sum();
    }


    public double calcularGananciaTotal(List<Venta> ventas) {
        return ventas.stream()
                .mapToDouble(Venta::getMargenGanancia)
                .sum();
    }
}
```

### Clase ConfigService
Mantiene en una memoria temporal o caché los datos que se usan todo el tiempo, como el valor del IGV o el símbolo de la moneda, para que el sistema no pierda tiempo leyéndolos del disco duro cada vez que los necesita.
```
package grupo2.mecanica_ed_02.Service;

import grupo2.mecanica_ed_02.Modelos.Configuracion;
import grupo2.mecanica_ed_02.Persistence.GestorDatosJSON;

public class ConfigService {

    private final GestorDatosJSON gestorDatos;
    private Configuracion configuracionCache;

    public ConfigService(GestorDatosJSON gestorDatos) {
        this.gestorDatos = gestorDatos;

        this.configuracionCache = gestorDatos.leerConfiguracion();
    }

    public Configuracion getConfiguracion() {

        return this.configuracionCache;
    }

    public void guardarConfiguracion(Configuracion nuevaConfig) {
        gestorDatos.guardarConfiguracion(nuevaConfig);

        this.configuracionCache = nuevaConfig;
        System.out.println("Configuración guardada y caché actualizada.");
    }

    public double getPorcentajeIGV() {
        return this.configuracionCache.getPorcentajeIGV();
    }

    public String getSimboloMoneda() {
        return this.configuracionCache.getSimboloMoneda();
    }
}
```
