# Problemas de SSH y permisos en usuarios del servidor Odoo

## 1. Descripción

Se presentaron varios problemas al intentar conectarse por SSH:

- Error `Permission denied (publickey)`.
- Usuarios que existían en `/etc/passwd` pero no podían iniciar sesión.
- Usuarios con shell `/usr/sbin/nologin`.

---

## 2. Usuario `odoo` y por qué no debe usarse para SSH

Al inspeccionar el usuario `odoo`:

```bash
cat /etc/passwd | grep odoo
```

Salida típica:

```text
odoo:x:108:111::/opt/odoo:/usr/sbin/nologin
```

El shell `/usr/sbin/nologin` indica que este usuario es solo de servicio y **no debe usar SSH**. Esto explica errores de:

```text
Permission denied (publickey)
```

cuando se intenta `ssh odoo@IP`.

---

## 3. Solución: usar un usuario de administración (por ejemplo `julianblancoelvira`)

Se creó/configuró el usuario `julianblancoelvira` para acceso SSH y se le añadieron claves públicas.

### 3.1. Crear carpeta `.ssh` y `authorized_keys`

En el servidor:

```bash
sudo mkdir -p /home/julianblancoelvira/.ssh
echo "ssh-ed25519 AAAA... julia@tu-correo.com" | sudo tee /home/julianblancoelvira/.ssh/authorized_keys
sudo chmod 700 /home/julianblancoelvira/.ssh
sudo chmod 600 /home/julianblancoelvira/.ssh/authorized_keys
sudo chown -R julianblancoelvira:julianblancoelvira /home/julianblancoelvira/.ssh
```

### 3.2. Conexión desde el cliente

```bash
ssh julianblancoelvira@34.63.38.132
```

Conexión establecida correctamente.

---

## 4. Gestionar otros usuarios con problemas de contraseña

En otros casos, se intentó cambiar la contraseña de usuarios con:

```bash
sudo passwd usuario
```

Y se recibió:

```text
passwd: Authentication token manipulation error
```

Esto suele indicar:

- Problemas de permisos sobre `/etc/shadow`.
- Uso incorrecto de `sudo`.
- El usuario realmente no existe en `/etc/passwd` aunque sí en otros sitios.

La solución recomendada en estos casos es:

1. Verificar el usuario en `/etc/passwd`.
2. Si no existe, crearlo correctamente con `useradd`.
3. Asignar contraseña con `sudo passwd usuario`.

---

## 5. Buenas prácticas

- No permitir nunca el login SSH del usuario de servicio `odoo`.
- Usar siempre un usuario administrador (`julianblancoelvira`, etc.) y luego:

```bash
sudo -u odoo -H bash
```

cuando se necesite ejecutar algo como `odoo`.
- Mantener las claves privadas **solo en el cliente**, nunca copiarlas al servidor.

---

## 6. Resumen

- `odoo` es un usuario de sistema sin shell de login: no usarlo para SSH.
- Crear un usuario administrador con claves públicas y usar `sudo`.
- Resolver errores de `Permission denied (publickey)` revisando claves y permisos de `.ssh`.
