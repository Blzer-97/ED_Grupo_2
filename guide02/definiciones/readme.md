# Definiciones y Abreviaturas
A continuación se presentan las definiciones y respectivas abreviaturas utilizadas en el desarrollo del software de *“Sistema de Gestión para una Mecánica Automotriz”*. Estas definiciones tienen como objetivo unificar el lenguaje entre el equipo de desarrollo y el usuario final, asegurando una interpretación correcta de los procesos del software.

## 1. Perspectiva de lógica de negocio
Términos relacionados con el funcionamiento operativo de la mecánica y la gestión del negocio.
- **SKU (Stock Keeping Unit / Unidad de Mantenimiento de Stock)**: Código único alfanumérico asignado a cada producto (ej. "ACE-001"). En este sistema, funciona como el identificador principal para buscar, editar o realizar transacciones sobre un producto en el inventario.
- **Servicio**: Actividad intangible ofrecida por el taller mecánico que implica mano de obra especializada, por ejemplo: "Cambio de Aceite", "Afinamiento". A diferencia de los productos, los servicios no controlan stock físico.
- **Producto**: Bien tangible comercializado por la mecánica, por ejemplo: "Aceite", "Neumáticos". Estos ítems están sujetos a control de inventario y su cantidad disminuye con cada venta.
- **Reabastecimiento**: Acción de ingresar nuevas unidades de un producto existente al inventario, aumentando su stock disponible sin modificar sus datos maestros (nombre, categoría).
- **IGV (Impuesto General a las Ventas)**: Tributo aplicado al precio de venta. En el sistema, es un parámetro configurable (actualmente por defecto al 18%) que se calcula sobre el subtotal de la transacción.
- **Item de Venta**: Cada línea individual dentro de una boleta que representa un producto o servicio específico, detallando su cantidad, precio unitario y subtotal.
- **Boleta de Venta**: Documento final generado en formato PDF que valida la transacción comercial entre la mecánica y el cliente, detallando los ítems adquiridos y los montos totales.

## 2. Perspectiva tecnológica
Términos técnicos, herramientas y métodos utilizados durante la etapa de implementación y pruebas del prototipo.
- **JSON (JavaScript Object Notation)**: Formato ligero de intercambio de datos. En este prototipo, se utiliza como mecanismo de Persistencia (base de datos basada en archivos) para almacenar la información de productos, ventas y configuraciones de forma permanente.
- **Maven**: Herramienta de gestión de proyectos y comprensión de software. Se utiliza para gestionar las dependencias del proyecto (librerías externas) y el proceso de construcción (build).
- **Jackson**: Librería de Java utilizada para procesar JSON. Permite la serialización (convertir objetos Java a texto JSON para guardar) y la deserialización (convertir texto JSON a objetos Java para leer).
- **Swing**: Biblioteca gráfica (GUI) de Java utilizada para construir la interfaz de usuario del sistema (Ventanas, Botones, Tablas).
- **PDFBox**: Librería de código abierto de Apache utilizada en el módulo de reportes para la generación dinámica de los documentos PDF (boletas).
- **CardLayout**: Gestor de diseño de Swing utilizado en la ventana principal (MainForm) que permite intercambiar entre diferentes paneles (Inventario, Ventas, Configuración) como si fueran cartas de una baraja, manteniendo una única ventana activa.
- **Inyección de Dependencias (Manual)**: Patrón de diseño utilizado en la clase App.java donde los servicios (InventarioService, VentaService) reciben sus dependencias (como el GestorDatosJSON) a través del constructor, en lugar de crearlas internamente, facilitando las pruebas y la modularidad.
