# Comandos útiles de Odoo usados en el proyecto

## 1. Logs y estado del servicio

Ver últimos logs:

```bash
sudo journalctl -u odoo -n 50 --no-pager
```

Seguir logs en tiempo real:

```bash
sudo journalctl -u odoo -f
```

Ver estado:

```bash
sudo systemctl status odoo
```

Reiniciar Odoo:

```bash
sudo systemctl restart odoo
```

---

## 2. Comprobación de PostgreSQL

Ver estado:

```bash
sudo systemctl status postgresql
```

Entrar en PostgreSQL:

```bash
sudo -u postgres psql
```

Listar bases de datos:

```sql
\l
```

Listar roles:

```sql
\du
```

Salir:

```sql
\q
```

---

## 3. Lanzar Odoo manualmente (entorno virtual)

En algunos casos se lanzó Odoo manualmente con:

```bash
/opt/odoo/odoo/venv/bin/python3 /opt/odoo/odoo/odoo-bin -c /etc/odoo/odoo.conf
```

---

## 4. SSH y gestión de usuarios

Ver usuario en `/etc/passwd`:

```bash
cat /etc/passwd | grep odoo
```

Cambiar propietario de un archivo:

```bash
sudo chown odoo:odoo /etc/odoo/odoo.conf
```

Cambiar permisos:

```bash
sudo chmod 640 /etc/odoo/odoo.conf
```

---

## 5. Resumen

Estos comandos forman la base del mantenimiento diario de la instancia Odoo durante el proyecto Tiendas Paco.
