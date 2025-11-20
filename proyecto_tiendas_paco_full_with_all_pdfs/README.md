# Proyecto Tiendas Paco – Infraestructura Odoo & Google Cloud

Repositorio oficial de documentación técnica, errores y configuraciones del proyecto **Tiendas Paco**, donde se despliega un ERP Odoo sobre infraestructura de Google Cloud y entornos locales.

## 📂 Estructura del repositorio

- `documentacion/`  
  Documentos PDF generados durante el proyecto (conexión Supabase, errores Odoo, SSH, Google Cloud, etc.).

- `errores_tecnicos/`  
  Documentación detallada de errores reales que ocurrieron durante la implantación:
  - `odoo/` → Errores de Odoo (CSS/JS, dbfilter, rol odoo, master password, addons, logs…).
  - `google_cloud/` → Errores en Cloud SQL, backups, buckets, IAP, SSH.
  - `otros/` → Problemas con temas web, plantillas y CSVs.

- `scripts/`  
  Comandos y scripts útiles para operar la instancia de Odoo (backups, restauración, comandos frecuentes).

- `configuraciones/`  
  Archivos de configuración de referencia, como `ejemplo_odoo.conf` y notas de conexión a Cloud SQL.

## 🎯 Objetivo del proyecto

- Servir como **base documental** del despliegue de Odoo para Tiendas Paco.
- Recoger **todos los errores** que se encontraron, junto con su análisis y solución paso a paso.
- Proporcionar un recurso docente para entender una implantación real de Odoo sobre Google Cloud.

## 👥 Autores

- Equipo Tiendas Paco  
- Coordinación técnica: Julian  
- Asistencia de depuración y documentación: ChatGPT

## ✅ Estado

La documentación está pensada para ser utilizada como material de apoyo en clase y como referencia para futuros proyectos similares (ERP + Cloud).
