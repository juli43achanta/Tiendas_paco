# Error al cargar módulos personalizados en Odoo (addons_path)

## 1. Descripción

En tu proyecto se trabajó con un módulo personalizado relacionado con Supabase (`supabase_conector`). Aunque finalmente se decidió **no usarlo**, sí fue necesario depurar errores asociados al **`addons_path`** y a la carga de módulos.

Errores típicos en los logs:

```text
FileNotFoundError: No such file or directory: '/odoo/odoo-server/addons'
```

o el módulo no aparecía en la lista de aplicaciones.

---

## 2. Causa

Configuración incorrecta del parámetro `addons_path` en `odoo.conf`. Se encontraron casos con dos líneas:

```ini
addons_path = /odoo/odoo-server/addons,/odoo/custom/addons
addons_path = /opt/odoo/odoo/addons
```

Odoo solo tiene en cuenta **la última línea**, por lo que la ruta a `/odoo/custom/addons` quedaba ignorada o se referenciaban rutas que no existían.

---

## 3. Comandos útiles

Ver rutas de addons:

```bash
sudo cat /etc/odoo/odoo.conf | grep addons_path
```

Listar directorios:

```bash
sudo ls -l /opt/odoo/odoo/addons
sudo ls -l /odoo/custom/addons
```

---

## 4. Solución paso a paso

1. **Eliminar líneas duplicadas de `addons_path`**:

Editar:

```bash
sudo nano /etc/odoo/odoo.conf
```

Dejar **solo una** línea, por ejemplo:

```ini
addons_path = /opt/odoo/odoo/addons,/odoo/custom/addons
```

2. **Verificar que las rutas existan de verdad**:

```bash
sudo ls -l /opt/odoo/odoo/addons
sudo ls -l /odoo/custom/addons
```

3. **Reiniciar Odoo**:

```bash
sudo systemctl restart odoo
sudo systemctl status odoo
```

4. **Actualizar lista de módulos** desde la interfaz:

- Activar **Modo desarrollador**.
- Ir a **Aplicaciones → Actualizar lista de aplicaciones**.
- Buscar el módulo deseado.

---

## 5. Recomendaciones

- Mantener una única línea `addons_path` con todas las rutas separadas por comas.
- No dejar rutas antiguas o inexistentes (`/odoo/odoo-server/addons` si ya no existe).
- Documentar la ruta estándar donde se dejan los módulos personalizados (por ejemplo `/odoo/custom/addons`).

---

## 6. Resumen

- Problemas al cargar módulos casi siempre = rutas de `addons_path`.
- Solución: unificar y corregir rutas en `/etc/odoo/odoo.conf` y reiniciar.
