## Pruebas de caja negra
Nota: Hacer clic en las imágenes para una mejor visualización. 
| Requisito Funcional | ID Caso | Descripción breve del caso | Entrada(s) | Resultado Esperado | Evidencia | Resultado |
|---------------------|---------|----------------------------|------------|--------------------|-----------|-----------|
| RF01 - Gestión de Productos | CU01 | No permitir el registro de productos sin SKU | SKU: Vacío / Los demás campos fueron llenados | El producto no se registra | <img width="1019" height="519" alt="image" src="https://github.com/user-attachments/assets/891988ad-73f4-4fb8-aea3-5b3c24947ade" /> | Ok |
| RF02 - Gestión de Inventario | CU02 | Disminuir el stock al realizar una venta | Agregar a la venta el producto con SKU ACE001 y RN001 (donde se vende 5 cantidades de cada uno) | El stock de los dos productos disminuyen acorde a la venta | <img width="612" height="504" alt="image" src="https://github.com/user-attachments/assets/2f5fb495-68b3-46de-8802-a82772da309b" /> | Ok |
| RF003 - Gestión de Ventas | CU03 | No permitir la venta si no hay stock | F3C4 | F3C5 | F3C6 | F3C7 |
| RF004 - Gestión de Servicios | CU05 | Crear un servicio nuevo desde venta | F5C4 | F5C5 | F5C6 | F5C7 |
| RF005 - Creación de Reportes | CU04 | Generación de boleta de forma local | F4C4 | F4C5 | F4C6 | F4C7 |
