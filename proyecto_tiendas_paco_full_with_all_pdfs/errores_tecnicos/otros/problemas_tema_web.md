# Problemas con temas y plantillas web en Odoo (Website)

## 1. Descripción

Durante la configuración del sitio web de Tiendas Paco en Odoo, se detectaron problemas como:

- El tema no se instalaba completamente.
- La plantilla aparecía “a medias”, sin la estructura esperada.
- Tras reinstalar el módulo de website o el tema, seguía sin cargarse la interfaz de demo.

---

## 2. Causas probables

1. **Instalación incompleta del tema** o dependencia de otros módulos no instalada.
2. Errores en los logs de Odoo al cargar el módulo del tema.
3. Conflictos con vistas heredadas o personalizaciones previas.
4. Problemas de caché del navegador o de assets (ver también `error_carga_css_js.md`).

---

## 3. Pasos de diagnóstico

1. Revisar logs:

```bash
sudo journalctl -u odoo -n 50 --no-pager
```

2. Comprobar si el tema está instalado:

- Ir a **Aplicaciones → Filtros → Módulos instalados**.
- Buscar el nombre del tema.

3. Activar modo desarrollador y revisar **Vistas**:

- Ajustes → Interfaz de desarrollador → Vistas.
- Buscar vistas relacionadas con `website` o con el nombre del tema.

---

## 4. Solución práctica aplicada

En el contexto del proyecto, se tomaron los siguientes pasos:

1. Reinstalar el tema desde **Aplicaciones**.
2. Limpiar caché de navegación (Ctrl+F5) y probar en ventana privada.
3. Revisar que no hubiera errores de assets ni rutas en los logs.
4. Cuando el tema no proporcionó una demo adecuada, se optó por:
   - Crear un catálogo de productos manualmente.
   - Importar productos vía CSV.
   - Ajustar las páginas con el editor visual de Odoo.

---

## 5. Recomendaciones

- No depender al 100% de la demo del tema; usarla como base, pero estar preparado para ajustar secciones manualmente.
- Documentar qué tema se ha instalado y qué versión de Odoo se usa.
- Tras problemas repetidos, plantear probar con otro tema más estándar.

---

## 6. Resumen

Los problemas con temas web suelen ser una combinación de:

- Errores de assets.
- Dependencias no instaladas.
- Expectativas distintas de lo que realmente trae la demo del tema.

La solución pasa por revisar logs, reinstalar, limpiar caché y, en última instancia, maquetar con el editor web.
