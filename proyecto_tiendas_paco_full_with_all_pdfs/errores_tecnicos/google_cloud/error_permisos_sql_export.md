# Error de permisos al exportar SQL desde Cloud SQL a Cloud Storage

## 1. Descripción

Durante la configuración del script de **backup automático** de la base de datos de Odoo en Cloud SQL, se encontró el error:

```text
ERROR: (gcloud.sql.export.sql) PERMISSION_DENIED: Request had insufficient authentication scopes.
```

Y en otro momento:

```text
HTTPError 412: The service account does not have the required permissions for the bucket.
```

---

## 2. Causa

La cuenta de servicio que usa el comando `gcloud sql export sql` (normalmente algo como:

```text
943789641071-compute@developer.gserviceaccount.com
```

) no tiene permisos suficientes sobre el **bucket de Cloud Storage** donde se quiere guardar el backup.

---

## 3. Comandos implicados

Script típico ejecutado en la VM:

```bash
sudo /usr/local/bin/odoo_auto_backup.sh
```

Dentro, algo como:

```bash
gcloud sql export sql odoo-db-produccion   gs://tienda-paco-erp-docu-v1/backup.sql   --database=odoo17
```

---

## 4. Solución paso a paso

### 4.1. Identificar la cuenta de servicio

Ejecutar:

```bash
gcloud config get-value account
```

y revisar los mensajes de error, donde suele aparecer una línea indicando la cuenta, por ejemplo:

```text
This command is authenticated as 943789641071-compute@developer.gserviceaccount.com
```

### 4.2. Dar permisos en el bucket

En Google Cloud Console:

1. Ir a **Cloud Storage → Buckets → tienda-paco-erp-docu-v1**.
2. Pestaña **Permisos**.
3. Añadir la cuenta de servicio:
   - `943789641071-compute@developer.gserviceaccount.com`
4. Asignar el rol:
   - **Storage Object Admin** (*roles/storage.objectAdmin*) o mínimo:
     - **Storage Object Creator** (*roles/storage.objectCreator*).

Guardar cambios.

### 4.3. Reintentar el comando

Volver a lanzar el script:

```bash
sudo /usr/local/bin/odoo_auto_backup.sh
```

Comprobar que ahora el backup se genera correctamente en el bucket.

---

## 5. Recomendaciones

- Documentar siempre qué cuenta de servicio utiliza cada VM o cada script.
- Usar roles mínimos pero suficientes (principio de privilegio mínimo).
- Probar manualmente el `gcloud sql export sql` antes de automatizarlo con cron.

---

## 6. Resumen

- Error `PERMISSION_DENIED` al exportar SQL ⇒ permisos insuficientes en Cloud Storage.
- Solución: añadir la cuenta de servicio al bucket con el rol adecuado.
