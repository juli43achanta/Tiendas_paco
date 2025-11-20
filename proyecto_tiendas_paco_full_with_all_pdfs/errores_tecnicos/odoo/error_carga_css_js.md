# Error de carga de CSS y JS en Odoo (assets rotos)

## 1. Descripción del problema

En algunas ocasiones, al acceder a Odoo (interfaz web), la página se muestra sin estilos (todo en blanco y negro, sin maquetación) o ciertas funcionalidades dejan de funcionar porque no se cargan los archivos **CSS** y **JavaScript**.

Síntomas típicos:

- La web se ve “rota” o muy básica.
- La consola del navegador muestra errores 404 al cargar `/web/content/*` o `/web/assets/*`.
- Tras cambiar configuración de dominio, proxy o `web.base.url`, los assets dejan de cargar.

---

## 2. Causas habituales

1. **URL base mal configurada** en `odoo.conf` (`web.base.url` apuntando a una IP/puerto incorrecto).
2. Cambios en **proxy inverso / Nginx / Apache** que no reenvían bien las rutas `/web` o `/web/content`.
3. Errores al actualizar módulos o al limpiar assets.
4. Cambios de puerto (por ejemplo de 8069 a otro) sin actualizar la configuración.
5. Problemas de permisos en el directorio de datos (`data_dir`) donde Odoo genera los assets.

---

## 3. Comandos útiles de diagnóstico

Ver últimos logs del servicio Odoo:

```bash
sudo journalctl -u odoo -n 50 --no-pager
```

Reiniciar Odoo:

```bash
sudo systemctl restart odoo
sudo systemctl status odoo
```

Comprobar puertos en escucha:

```bash
sudo ss -tulpn | grep 8069
```

---

## 4. Solución paso a paso

### 4.1. Revisar `web.base.url` en `odoo.conf`

Editar el archivo:

```bash
sudo nano /etc/odoo/odoo.conf
```

Verificar que exista y sea correcto:

```ini
[options]
web.base.url = http://IP_O_DOMINIO_CORRECTO:8069
```

Ejemplo típico en tu proyecto:

```ini
web.base.url = http://34.60.47.194:8069
```

> No pongas solo la IP sin puerto si no tienes un proxy que haga el redireccionamiento.

Guardar y salir, luego:

```bash
sudo systemctl restart odoo
```

---

### 4.2. Limpiar y regenerar assets

En modo desarrollador, desde la interfaz web:

1. Activar **Modo desarrollador**.
2. Ir a **Configuración → Acciones del desarrollador → Borrar caché de assets** (si está disponible en tu versión).

Alternativamente, reiniciar el servicio y, si se usan workers, vaciar temporalmente el `data_dir` de assets generados (con cuidado y haciendo copia de seguridad).

---

### 4.3. Verificar proxy / Nginx (si existe)

Si usas Nginx/Apache delante de Odoo, asegúrate de tener algo similar:

```nginx
location / {
    proxy_pass http://127.0.0.1:8069;
    proxy_set_header Host $host;
}
```

Y que no haya reglas que bloqueen `/web/` o `/web/content/`.

---

## 5. Cómo evitar que vuelva a ocurrir

- Documentar siempre la IP, dominio y puerto final del Odoo de producción.
- Mantener consistente `web.base.url` con el entorno real.
- Tras cambios en el servidor (IP nueva, puerto distinto, proxy nuevo), revisar este archivo.
- No tocar el `data_dir` ni permisos sin saber exactamente qué se está haciendo.

---

## 6. Resumen rápido

1. Página sin estilos = problema de **assets**.
2. Revisar `web.base.url` en `/etc/odoo/odoo.conf`.
3. Reiniciar Odoo y borrar caché de assets.
4. Comprobar proxy Nginx/Apache si existe.
