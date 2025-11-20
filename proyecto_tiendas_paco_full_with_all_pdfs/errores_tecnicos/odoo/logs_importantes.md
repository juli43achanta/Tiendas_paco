# Logs importantes de Odoo y cómo interpretarlos

## 1. Introducción

Durante la depuración del proyecto se usó frecuentemente:

```bash
sudo journalctl -u odoo -n 50 --no-pager
```

Este comando muestra las últimas líneas del log del servicio Odoo, clave para detectar errores de:

- Conexión a PostgreSQL
- Carga de módulos
- Problemas de permisos
- Errores Python de los addons

---

## 2. Comandos clave de logs

### 2.1. Ver últimos 50 registros

```bash
sudo journalctl -u odoo -n 50 --no-pager
```

### 2.2. Seguir el log en tiempo real

```bash
sudo journalctl -u odoo -f
```

### 2.3. Filtrar por niveles

En `odoo.conf` se puede establecer:

```ini
log_level = info
logfile = /var/log/odoo/odoo-server.log
logrotate = True
```

Y después revisar el archivo:

```bash
sudo tail -n 50 /var/log/odoo/odoo-server.log
```

---

## 3. Ejemplos típicos de errores

### 3.1. Error de rol inexistente

```text
FATAL:  role "odoo" does not exist
```

→ Ver documento `error_usuario_odoo.md`.

### 3.2. Error de módulos / rutas

```text
FileNotFoundError: No such file or directory: '/odoo/odoo-server/addons'
```

→ Ver documento `error_carga_modulos.md`.

### 3.3. Error de contraseña maestra

```text
Database creation error: Access Denied
```

→ Ver documento `error_master_password.md`.

---

## 4. Buenas prácticas

- Tras **cada cambio** en `odoo.conf` o en la base de datos, revisar los logs.
- No limitarse solo al mensaje en el navegador; ir siempre a los logs del sistema.
- Documentar los comandos de logs (como se ha hecho en este proyecto) para el resto del equipo.

---

## 5. Resumen

Los logs de Odoo son la primera fuente de verdad cuando algo falla. Dominar `journalctl` y la lectura de `/var/log/odoo` ahorra muchísimo tiempo de depuración.
