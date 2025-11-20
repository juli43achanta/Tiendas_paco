# Notas sobre la conexión de Odoo a Cloud SQL (Google Cloud)

## 1. Descripción general

En el proyecto se planteó conectar una instancia de Odoo (en VM) con una base de datos PostgreSQL alojada en Cloud SQL. Finalmente se valoraron varias opciones (Cloud SQL Proxy, conexión directa, Supabase, etc.) y también se trabajó con instalaciones locales.

---

## 2. Ejemplo de configuración en `odoo.conf`

Cuando se conecta a una base de datos remota (Cloud SQL), la configuración de Odoo puede ser similar a:

```ini
[options]
db_host = /cloudsql/utility-destiny-474411-s7:us-central1:odoo-db-produccion
db_port = False
db_user = odoo
db_password = Proyecto1.
db_name = odoo17
db_sslmode = disable
dbfilter = ^odoo17$
```

Puntos clave:

- `db_host` puede ser un socket de Cloud SQL (`/cloudsql/PROYECTO:REGION:INSTANCIA`) si se usa el Cloud SQL Proxy.
- `db_user` y `db_password` deben coincidir con lo creado en Cloud SQL.
- `dbfilter` debe ajustarse al nombre de la base de datos.

---

## 3. Consideraciones de red y permisos

- La VM que ejecuta Odoo debe tener permisos suficientes (a través de su cuenta de servicio) para acceder a la instancia de Cloud SQL.
- Es posible usar Cloud SQL Auth Proxy si las restricciones de red lo requieren.
- En entornos docentes se planteó la alternativa de trabajar con PostgreSQL local para simplificar.

---

## 4. Recomendaciones

- Documentar el **DSN completo** (host, puerto, usuario, BD).
- No mezclar entornos (producción vs. pruebas) en la misma instancia de Cloud SQL sin una buena estrategia de nombres.
- Probar la conexión desde la VM con herramientas como `psql` antes de arrancar Odoo.

---

## 5. Resumen

La conexión Odoo–Cloud SQL funciona bien siempre que:

- `odoo.conf` refleje correctamente host y credenciales.
- La red y las cuentas de servicio estén bien configuradas.
- Se pruebe previamente con `psql` y se monitorice con logs de Odoo y de Cloud SQL.
