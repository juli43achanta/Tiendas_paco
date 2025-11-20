# Error de `Access Denied` (Contraseña maestra de Odoo)

## 1. Descripción

Al intentar crear una nueva base de datos desde la interfaz web de Odoo, aparece el error:

```text
Database creation error: Access Denied
```

o la pantalla simplemente indica que la **master password** es incorrecta.

---

## 2. Causa

Odoo utiliza una **contraseña maestra** (master password) definida en `odoo.conf` mediante la opción:

```ini
admin_passwd = ...
```

Si lo que escribes en el navegador no coincide con la contraseña almacenada en ese archivo, Odoo deniega la creación de bases de datos.

---

## 3. Verificación

Ver contenido de `odoo.conf`:

```bash
sudo nano /etc/odoo/odoo.conf
```

Buscar la línea:

```ini
admin_passwd = MiContraseñaSegura123
```

Comprobar permisos del archivo:

```bash
ls -l /etc/odoo/odoo.conf
```

Debe ser algo parecido a:

```text
-rw-r----- 1 odoo odoo ...
```

---

## 4. Solución paso a paso

1. **Definir o corregir `admin_passwd`**:

Editar:

```bash
sudo nano /etc/odoo/odoo.conf
```

Añadir o modificar:

```ini
admin_passwd = CambiaEstaContraseña123
list_db = True
```

2. **Guardar y cerrar**.

3. **Reiniciar Odoo**:

```bash
sudo systemctl restart odoo
sudo systemctl status odoo
```

4. **Usar esa misma contraseña** al crear la base de datos desde la web.

---

## 5. Recomendaciones de seguridad

- No dejar `admin_passwd = admin` en producción.
- Guardar esta contraseña en un gestor seguro (no en texto plano por correo).
- Restringir permisos de `odoo.conf`:

```bash
sudo chown odoo:odoo /etc/odoo/odoo.conf
sudo chmod 640 /etc/odoo/odoo.conf
```

---

## 6. Resumen rápido

- Error `Access Denied` al crear BD = master password incorrecta.
- Ajustar `admin_passwd` en `/etc/odoo/odoo.conf`.
- Reiniciar y usar esa contraseña en el navegador.
