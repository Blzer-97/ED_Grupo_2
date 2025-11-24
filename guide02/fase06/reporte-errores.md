## Reporte de errores
El detalle de los errores encontrados serán indicados en el Anexo 2 - “Informe de errores encontrados”:

#### NOTA: Hacer clic en las imágenes para verlas con mayor claridad.

## Anexo 2: “Informe de errores encontrados”

| ID Defecto | Descripción del defecto | Pasos | Fecha | Detectado por | Estado | Corregido por | Fecha de cierre | Prioridad |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| DE01 | Se pueden añadir valores negativos a los campos de producto <img width="866" height="260" alt="image" src="https://github.com/user-attachments/assets/70c274f2-1563-46c3-9f10-d471ab57ac26" /> | 1. Ingresar al módulo de Inventario / 2. Hacer clic en "Nuevo Producto" / 3. Escribir valores negativos en precios y stock inicial / 4. Registrar producto | 18-11-2025 | Matias Vincula | Corregido | Fabio Tohalino | 20-11-2025 | Media |
| DE02 | El botón “Historial” muestra una ventana vacía <img width="672" height="330" alt="image" src="https://github.com/user-attachments/assets/b09b335e-81d2-4bf7-9299-3d68fa8e3d99" /> | 1. Abrir el módulo Inventario / 2. Elegir un producto con múltiples movimientos / 3. Presionar “Historial” / 4. Se abre una ventana sin datos | 18-11-2025 | Luis Machaca | Corregido | Fabio Tohalino | 20-11-2025 | Alta |
| DE03 | El botón “Eliminar” no solicita confirmación antes de borrar un producto <img width="661" height="427" alt="image" src="https://github.com/user-attachments/assets/4044664c-aa66-401d-ba05-6f8b9d90a2d5" /> | 1. Acceder a Inventario / 2. Seleccionar un producto activo / 3. Presionar “Eliminar” / 4. El producto se elimina inmediatamente sin mostrar advertencia ni confirmación | 19-11-2025 | Matias Vincula | Corregido | Fabio Tohalino | 20-11-2025 | Baja |
| DE04 |Al renovar stock o ingresar uno nuevo, este no se "refrecaba" en el módulo de Ventas <img width="1567" height="894" alt="Screenshot 2025-11-24 010041" src="https://github.com/user-attachments/assets/2b64a8a5-44fb-4be9-91b2-04313dd6de71" /> Stock del primer producto: 45 <img width="1572" height="906" alt="Screenshot 2025-11-24 010047" src="https://github.com/user-attachments/assets/a85b30d6-bc3e-4fab-be34-45b243260845" /> Stock del primer producto: 39 | 1. Ir al módulo Inventario/ 2. Modificar el stock de un producto / 3. Ir al módulo Ventas / 4. El cambio en el stock no se ve reflejado en la tabla | F4C4 | F4C5 | F4C6 | F4C7 | F4C8 | F4C9 |
