# Error de `dbfilter` y selección de base de datos en Odoo

## 1. Descripción

En Odoo, el parámetro `dbfilter` controla qué bases de datos están visibles y disponibles para una instancia concreta. En tu proyecto, se utilizó una configuración parecida a:

```ini
dbfilter = ^odoo17$
```

Lo que provocó que solo la base de datos llamada exactamente `odoo17` fuese visible.

Problemas típicos:

- La base de datos correcta no aparece en la pantalla de selección.
- Odoo dice que no encuentra la base de datos.
- Tras restaurar una base de datos con otro nombre, Odoo no la lista.

---

## 2. Causa

Un `dbfilter` **demasiado restrictivo** o **mal definido**, que no coincide con el nombre real de la base de datos.

Ejemplos:

- Base de datos real: `odoo17_produccion`
- `dbfilter = ^odoo17$` → Solo coincide con `odoo17`, no con `odoo17_produccion`.

---

## 3. Comandos útiles

Listar bases de datos desde PostgreSQL:

```bash
sudo -u postgres psql -c "\l"
```

Ver el contenido actual de `odoo.conf`:

```bash
sudo cat /etc/odoo/odoo.conf
```

---

## 4. Solución paso a paso

1. **Comprobar el nombre real de la base de datos**:

```bash
sudo -u postgres psql
\l
\q
```

2. **Editar `odoo.conf`**:

```bash
sudo nano /etc/odoo/odoo.conf
```

Hay dos opciones:

### Opción A: Permitir todas las BDs (entorno de pruebas/docente)

```ini
dbfilter = .*
```

### Opción B: Filtrar por prefijo

Si tus BDs son del estilo `odoo17_produccion`, `odoo17_demo`:

```ini
dbfilter = ^odoo17.*
```

3. **Guardar y reiniciar Odoo**:

```bash
sudo systemctl restart odoo
sudo systemctl status odoo
```

4. Volver a entrar en `http://IP:8069/web/database/selector` y comprobar.

---

## 5. Recomendaciones

- En entornos docentes o de pruebas: usar `dbfilter = .*` para evitar confusiones.
- En producción: usar filtros más concretos, pero siempre documentarlos.
- No restaurar BDs con nombres aleatorios si el `dbfilter` espera un patrón concreto.

---

## 6. Resumen rápido

- Error de base de datos no encontrada o no listada ⇒ casi siempre `dbfilter`.
- Solución: ajustar patrón en `/etc/odoo/odoo.conf` según el nombre real de la BD.
