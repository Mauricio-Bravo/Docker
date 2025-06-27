# 📦 Proyecto: Sistema de Respaldo Automático a Google Drive

Este proyecto permite automatizar respaldos comprimidos en `.zip` de una carpeta local, y subirlos directamente a Google Drive utilizando `rclone`. El respaldo puede ejecutarse manualmente o automáticamente todos los días mediante `cron`.

## 🛠️ Tecnologías y herramientas utilizadas

- Ubuntu Server 24.04 (con interfaz gráfica)
- Bash
- `rclone`
- `cron`


## 🚀 Pasos realizados con sus comandos

### 🟣 1. Instalar `rclone`

```bash
sudo apt update
sudo apt install rclone
```

### 🟡 2. Configurar `rclone` con Google Drive

```bash
rclone config
```

✅ En el asistente:
- Presiona `n` para crear un nuevo remoto
- Nombre: `drive`
- Tipo: `drive`
- Sigue los pasos de autenticación
- Finaliza y prueba con `rclone ls drive:`

### 🟢 3. Crear carpetas necesarias

```bash
mkdir -p ~/respaldos_tmp
mkdir -p ~/Documents/proyecto
```

### 🔵 4. Crear el script de respaldo

```bash
gedit ~/respaldo.sh
```

⬇️ Luego copia y pega este código:

```bash
#!/bin/bash

CARPETA_ORIGEN="$HOME/Documents/proyecto"
CARPETA_TMP="$HOME/respaldos_tmp"
NOMBRE_ZIP="respaldo_$(date +%F_%H-%M).zip"
ARCHIVO_ZIP="$CARPETA_TMP/$NOMBRE_ZIP"

mkdir -p "$CARPETA_TMP"
zip -r "$ARCHIVO_ZIP" "$CARPETA_ORIGEN"
rclone copy "$ARCHIVO_ZIP" drive:Respaldos

echo "Respaldo completado: $NOMBRE_ZIP"
```

### 🟠 5. Dar permisos de ejecución

```bash
chmod +x ~/respaldo.sh
```

### 🔴 6. Ejecutar el script manualmente (prueba inicial)

```bash
./respaldo.sh
```

### ⚫ 7. Verificar los resultados

```bash
ls ~/respaldos_tmp
rclone ls drive:Respaldos
```

### ⚪ 8. Automatizar el respaldo con cron

```bash
crontab -e
```

⬇️ Agregar esta línea al final:

```bash
0 23 * * * /home/david-busta/respaldo.sh >> /home/david-busta/logs_respaldo.txt 2>&1
```

### 🧾 9. Verificar el log de respaldo

```bash
cat ~/logs_respaldo.txt
```

### 🧪 Prueba manual con log

```bash
/home/david-busta/respaldo.sh >> /home/david-busta/logs_respaldo.txt 2>&1
cat ~/logs_respaldo.txt
```

## 📁 Estructura esperada

```
/home/david-busta/
├── respaldo.sh
├── logs_respaldo.txt
├── respaldos_tmp/
│   └── respaldo_YYYY-MM-DD_HH-MM.zip
└── Documents/
    └── proyecto/
        └── [archivos a respaldar]
```

## 🎉 Proyecto completado

Ahora tienes un sistema funcional de respaldo automático que:
- Comprime archivos
- Los guarda localmente
- Los sube a Google Drive
- Y se ejecuta solo todos los días

## 📂 Directorios usados

- Carpeta a respaldar: `~/Documents/proyecto`
- Carpeta temporal: `~/respaldos_tmp`
- Archivo de log: `~/logs_respaldo.txt`
