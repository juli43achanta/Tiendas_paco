# Error `role "odoo" does not exist` en PostgreSQL

## 1. Descripción

Al iniciar Odoo o crear una base de datos, aparece el mensaje:

```text
FATAL:  role "odoo" does not exist
connection to server on socket '/var/run/postgresql/.s.PGSQL.5432' failed
```

Esto indica que Odoo intenta conectarse a PostgreSQL con el usuario `odoo`, pero ese rol no existe en el servidor de base de datos.

---

## 2. Causa técnica

En `odoo.conf` se suele configurar:

```ini
db_user = odoo
db_password = odoo
```

Si nunca se ha creado el rol `odoo` en PostgreSQL, la conexión falla.

---

## 3. Verificación

Comprobar que PostgreSQL está activo:

```bash
sudo systemctl status postgresql
```

Probar a listar roles:

```bash
sudo -u postgres psql -c "\du"
```

---

## 4. Solución paso a paso

1. **Asegurarse de que PostgreSQL está en marcha**:

```bash
sudo systemctl start postgresql
```

2. **Crear el rol `odoo` con permisos de creación de BD**:

```bash
sudo -u postgres psql
CREATE ROLE odoo WITH LOGIN PASSWORD 'odoo';
ALTER ROLE odoo CREATEDB;
\q
```

3. **Verificar que `odoo.conf` use ese usuario**:

```bash
sudo nano /etc/odoo/odoo.conf
```

Debe contener:

```ini
db_user = odoo
db_password = odoo
```

4. **Reiniciar el servicio Odoo**:

```bash
sudo systemctl restart odoo
sudo systemctl status odoo
```

5. Acceder de nuevo a Odoo y crear la base de datos.

---

## 5. Cómo evitarlo

- Documentar siempre los usuarios creados en PostgreSQL.
- Usar el mismo usuario en todos los entornos (por ejemplo `odoo`) para evitar diferencias.
- No borrar el rol `odoo` si está en uso por la instancia.

---

## 6. Resumen

- Error claro de PostgreSQL: rol inexistente.
- Crear el rol con `CREATE ROLE` y asignar permisos.
- Revisar `db_user` y `db_password` en `odoo.conf`.
