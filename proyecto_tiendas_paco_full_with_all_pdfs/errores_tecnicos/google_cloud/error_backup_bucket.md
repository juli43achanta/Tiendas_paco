# Error de backup: bucket sin permisos o mal configurado

## 1. Descripción

Al ejecutar el script de backup de la base de datos de Odoo hacia un bucket de Cloud Storage, aparecían errores relacionados con permisos o con el bucket configurado, por ejemplo:

```text
The service account does not have the required permissions for the bucket.
```

---

## 2. Causa

- El bucket existe pero la cuenta de servicio de la VM o del comando `gcloud` no tiene los permisos necesarios.
- El nombre del bucket está mal escrito.
- El bucket está en un proyecto diferente al que se está usando en la VM.

---

## 3. Verificaciones clave

1. Listar buckets:

```bash
gcloud storage buckets list
```

2. Ver proyecto activo:

```bash
gcloud config get-value project
```

3. Probar un comando de prueba (por ejemplo, listar objetos del bucket):

```bash
gcloud storage ls gs://tienda-paco-erp-docu-v1
```

---

## 4. Solución paso a paso

1. **Confirmar nombre del bucket** usado en el script, por ejemplo:

```bash
BUCKET=gs://tienda-paco-erp-docu-v1
```

2. **Dar permisos a la cuenta de servicio** en el bucket (desde la consola web o con `gcloud storage buckets add-iam-policy-binding`).

Ejemplo (ejecutar desde Cloud Shell):

```bash
gcloud storage buckets add-iam-policy-binding gs://tienda-paco-erp-docu-v1   --member="serviceAccount:943789641071-compute@developer.gserviceaccount.com"   --role="roles/storage.objectAdmin"
```

3. **Reintentar el backup** y revisar los logs.

---

## 5. Buenas prácticas

- Usar un bucket exclusivo para backups (por ejemplo `tienda-paco-odoo-backups`).
- Activar **versionado de objetos** para poder recuperar versiones antiguas.
- Probar el backup manualmente antes de programarlo con cron.

---

## 6. Resumen

La mayoría de errores de backup a Cloud Storage se resuelven corrigiendo:
- Nombre de bucket.
- Permisos de la cuenta de servicio.
- Proyecto activo en la VM.
