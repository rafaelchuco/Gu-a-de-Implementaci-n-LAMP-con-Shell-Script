# 📘 Guía de Implementación LAMP con Shell Script

Este repositorio contiene un **Shell Script** automatizado para instalar y configurar un servidor **LAMP** (Linux + Apache + MariaDB/MySQL + PHP) en **Ubuntu**.

---

## 🎯 Objetivo

Automatizar la instalación y configuración de un servidor web **LAMP** mediante un único script Bash. Este proyecto aplica conocimientos de administración de sistemas operativos, scripting y redes.

---

## ✅ Prerrequisitos

Antes de comenzar, asegúrate de cumplir con estos requisitos:

- ✔️ Sistema operativo **Ubuntu 20.04 LTS** o superior.
- ✔️ Usuario con privilegios de **sudo**.
- ✔️ Conexión a Internet para descargar paquetes.

---

## 📂 Archivos del Proyecto

- `instalar_lamp.sh` : Script principal que realiza toda la instalación y configuración.

---

## 🚀 Pasos de Implementación

### 1️⃣ Clonar o Crear el Script

Puedes copiar el script manualmente o clonarlo si está en un repositorio. Por ejemplo:

```bash
git clone https://github.com/usuario/tu-repo.git
cd tu-repo
```

O bien, crea un archivo nuevo con `nano`:

```bash
nano instalar_lamp.sh
```

### 2️⃣ Pegar el Contenido del Script

Copia y pega el siguiente contenido dentro de `instalar_lamp.sh`:

> 👉 **Nota:** Asegúrate de copiar todo el bloque sin errores.

```bash
#!/bin/bash

# Variables
PROJECT_DIR="mi_sitio_web"

# Actualizar paquetes
sudo apt update -y

# Instalar Apache
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2

# Instalar MariaDB
sudo apt install mariadb-server -y
sudo systemctl enable mariadb
sudo systemctl start mariadb

# Instalar PHP
sudo apt install php libapache2-mod-php php-mysql -y

# Crear directorio del proyecto
sudo mkdir -p /var/www/$PROJECT_DIR
sudo chown -R www-data:www-data /var/www/$PROJECT_DIR

# Crear Virtual Host
sudo tee /etc/apache2/sites-available/$PROJECT_DIR.conf > /dev/null <<EOF
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/$PROJECT_DIR
    ServerName localhost

    <Directory /var/www/$PROJECT_DIR>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF

# Habilitar nuevo sitio y deshabilitar default
sudo a2ensite $PROJECT_DIR.conf
sudo a2dissite 000-default.conf

# Crear página de prueba PHP
sudo tee /var/www/$PROJECT_DIR/index.php > /dev/null <<EOF
<!DOCTYPE html>
<html>
<head>
    <title>¡Bienvenido a mi Servidor LAMP!</title>
    <style>
        body { font-family: sans-serif; background-color: #f0f0f0; text-align: center; }
        .container { max-width: 800px; margin: 50px auto; padding: 20px; background: #fff; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        h1 { color: #333; }
    </style>
</head>
<body>
    <div class="container">
        <h1>¡Felicidades! Tu servidor LAMP está funcionando.</h1>
        <p>Esta página está siendo servida por Apache y procesada por PHP.</p>
        <hr>
        <?php phpinfo(); ?>
    </div>
</body>
</html>
EOF

# Reiniciar Apache
sudo systemctl restart apache2

# Mostrar IP local
IP_ADDR=$(hostname -I | awk '{print $1}')
echo "============================================================="
echo "          ¡INSTALACIÓN COMPLETADA CON ÉXITO!"
echo "============================================================="
echo "Puedes acceder desde: http://$IP_ADDR"
echo "Archivos del sitio: /var/www/$PROJECT_DIR"
echo "============================================================="
```

---

### 3️⃣ Guardar y Cerrar el Script

1. Presiona **Ctrl + X** para salir de `nano`.
2. Pulsa **Y** para confirmar los cambios.
3. Pulsa **Enter** para guardar como `instalar_lamp.sh`.

---

### 4️⃣ Dar Permisos de Ejecución

Permite la ejecución del script:

```bash
chmod +x instalar_lamp.sh
```

---

### 5️⃣ Ejecutar el Script

Ejecuta el script con privilegios de administrador:

```bash
./instalar_lamp.sh
```

📌 **Nota:** Te pedirá tu contraseña de usuario para ejecutar los comandos `sudo`.

---

## 🔍 Verificación

Al finalizar la instalación:

1. El script mostrará tu **IP local**.
2. Abre tu navegador.
3. Ingresa la dirección IP, por ejemplo: [http://192.168.1.10](http://192.168.1.10)
4. Verifica que se muestra la página de bienvenida con información de PHP.

---

## 📚 Conceptos Reforzados

* ✅ **Scripting en Bash:** Variables, redirección, Heredoc.
* ✅ **Administración de Paquetes:** `apt update`, `apt install`.
* ✅ **Administración de Servicios:** `systemctl`.
* ✅ **Manejo de Archivos:** `mkdir`, `chown`, `tee`.
* ✅ **Virtual Hosts en Apache.**
* ✅ **Configuración de Red Local:** IP con `hostname -I`.

---

## ⚡ Resultados Esperados

Si todo está correcto, tendrás un servidor **LAMP** operativo sirviendo una página web de prueba con información PHP.
