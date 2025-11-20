# Problemas con plantillas y carga de datos (CSV) en Odoo

## 1. Descripción

Al trabajar con Odoo para cargar productos de Tiendas Paco (lista de productos de supermercado), se presentaron:

- Errores al importar CSV.
- Problemas con rutas de imágenes dentro de los CSV.
- Campos obligatorios no cumplimentados.

---

## 2. Causas típicas

1. **Formato CSV incorrecto**:
   - Separador `;` en lugar de `,` (o viceversa).
   - Codificación no UTF-8.
2. **Columnas obligatorias faltantes**:
   - Nombre del producto.
   - Tipo.
   - Categoría, etc.
3. Para imágenes:
   - Rutas que apuntan a archivos que no existen en el servidor.
   - Intentar cargar URLs externas sin configuración adecuada.

---

## 3. Estrategia de solución aplicada en el proyecto

1. Definir primero **un CSV mínimo funcional**, solo con:
   - Nombre del producto.
   - Precio.
   - Tipo.
   - Unidad de medida.

2. Importar ese CSV hasta que no dé errores.

3. Una vez que los productos estén importados correctamente, añadir:

   - Categorías.
   - Referencias internas.
   - Imágenes (ya sea manualmente o por scripts separados).

4. Para las imágenes:
   - Se optó por subir las imágenes manualmente o por ZIP y asociarlas desde Odoo, ya que incrustarlas en el CSV generaba errores.

---

## 4. Buenas prácticas para plantillas CSV

- Exportar primero algunos productos de ejemplo desde Odoo y usar ese CSV como **plantilla base**.
- Evitar caracteres especiales en los nombres de archivo de imágenes.
- Trabajar siempre con UTF-8 y separador consistente (`,` o `;`).

---

## 5. Resumen

Las plantillas de importación de Odoo son sensibles a:

- Formato exacto de columnas.
- Campos obligatorios.
- Rutas de archivos.

La solución más efectiva es partir de una exportación de ejemplo, simplificar y luego ir añadiendo campos de forma incremental.
