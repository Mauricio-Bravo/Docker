# Sistema de Respaldo Automático a Google Drive

Este script en Bash permite crear respaldos comprimidos (.zip) de una carpeta específica y subirlos automáticamente a Google Drive usando `rclone`.

## 🚀 Requisitos

- Ubuntu 24.04 con entorno gráfico
- rclone instalado y configurado
- Cuenta de Google Drive
- Acceso a terminal

## 🛠 Instalación y Uso

### 1. Instalar `rclone`

```bash
sudo apt update
sudo apt install rclone
```

### 2. Configurar `rclone`

```bash
rclone config
```

Seguir el asistente y conectar tu cuenta de Google Drive. Usar el nombre `drive` para el remoto.

### 3. Crear carpetas necesarias

```bash
mkdir -p ~/respaldos_tmp
mkdir -p ~/Documents/proyecto
```

### 4. Crear el script `respaldo.sh`

```bash
gedit ~/respaldo.sh
```

Pegar el contenido del script y guardar.

### 5. Darle permisos y probarlo

```bash
chmod +x ~/respaldo.sh
./respaldo.sh
```

### 6. Automatizar con cron

```bash
crontab -e
```

Agregar esta línea al final (sin `#`):

```
0 23 * * * /home/david-busta/respaldo.sh >> /home/david-busta/logs_respaldo.txt 2>&1
```

### 7. Verificar respaldo

```bash
ls ~/respaldos_tmp
rclone ls drive:Respaldos
cat ~/logs_respaldo.txt
```

## 📂 Directorios usados

- Carpeta a respaldar: `~/Documents/proyecto`
- Carpeta temporal: `~/respaldos_tmp`
- Archivo de log: `~/logs_respaldo.txt`
