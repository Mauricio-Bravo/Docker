# Docker

# 🐳 Proyecto Docker DEVASC 
- Johan Espinoza
- Mauricio Bravo
- David Bustamante

## 📘 Descripción

Este proyecto contiene tres aplicaciones web contenerizadas con Docker, creadas como parte de la guía de aprendizaje de la VM DEVASC. Cada app fue diseñada con un propósito diferente:

- **App1**: Flask básico que muestra la IP del cliente (puerto 8000)
- **App2**: Flask + HTML + CSS estilizado (puerto 8181)
- **App3**: Servidor web estático con HTML + Bash script (puerto 8888)

## 📁 Estructura de carpetas

```bash
docker_proyecto/
├── app1/
│   ├── app1.py
│   └── Dockerfile
├── app2/
│   ├── app2.py
│   ├── Dockerfile
│   ├── templates/
│   │   └── index.html
│   └── static/
│       └── style.css
└── web8888/
    ├── index.html
    ├── Dockerfile
    └── run.sh
```

## 🔧 Requisitos previos

- Máquina virtual DEVASC (desde `.ova`)
- Docker instalado y configurado:

```bash
sudo apt update && sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

Reiniciar la máquina virtual después de agregar el usuario al grupo `docker`:

```bash
reboot
```

## 🚀 Paso a paso de cada app

### 🔹 App 1 – Flask básico (puerto 8000)

```bash
mkdir -p ~/docker_proyecto/app1 && cd ~/docker_proyecto/app1
```

**Archivo `app1.py`:**

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def index():
    ip = request.remote_addr
    return f'Tu IP es: {ip}'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
```

**Dockerfile:**

```Dockerfile
FROM python:3.10-alpine
WORKDIR /app
COPY app1.py .
RUN apk add --no-cache gcc musl-dev libffi-dev && pip install --no-cache-dir --progress-bar off flask
CMD ["python", "app1.py"]
```

```bash
docker build -t flask-ip-app .
docker run -d --restart always -p 8000:8000 --name contenedor8000 flask-ip-app
```

---

### 🔹 App 2 – Flask + HTML + CSS (puerto 8181)

```bash
mkdir -p ~/docker_proyecto/app2/templates ~/docker_proyecto/app2/static
cd ~/docker_proyecto/app2
```

**Archivo `app2.py`:**

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def index():
    ip = request.remote_addr
    return render_template('index.html', ip=ip)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8181)
```

**templates/index.html:**

```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    <title>Tu IP</title>
</head>
<body>
    <div class="card">
        <h1>Bienvenido</h1>
        <p>Tu IP es: <strong>{{ ip }}</strong></p>
    </div>
</body>
</html>
```

**static/style.css:**

```css
body {
    background-color: #f0f0f0;
    font-family: sans-serif;
    text-align: center;
    margin-top: 50px;
}
.card {
    background: white;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 0 10px #888;
    display: inline-block;
}
```

**Dockerfile:**

```Dockerfile
FROM python:3.10-alpine
WORKDIR /app
COPY . .
RUN apk add --no-cache gcc musl-dev libffi-dev && pip install --no-cache-dir --progress-bar off flask
CMD ["python", "app2.py"]
```

```bash
docker build -t flask-ip-style .
docker run -d --restart always -p 8181:8181 --name contenedor8181 flask-ip-style
```

---

### 🔹 App 3 – HTML estático + Bash (puerto 8888)

```bash
mkdir ~/docker_proyecto/web8888 && cd ~/docker_proyecto/web8888
```

**index.html:**

```html
<!DOCTYPE html>
<html>
<head><title>Servidor 8888</title></head>
<body><h1>Servidor Web funcionando en el puerto 8888</h1></body>
</html>
```

**Dockerfile:**

```Dockerfile
FROM python:3.10-alpine
WORKDIR /web
COPY index.html .
CMD ["python3", "-m", "http.server", "8888"]
```

**run.sh:**

```bash
#!/bin/bash
echo "Construyendo imagen..."
docker build -t web8888 .
echo "Ejecutando contenedor..."
docker run -d --restart always -p 8888:8888 --name contenedor8888 web8888
echo "Contenedores activos:"
docker ps
```

```bash
chmod +x run.sh
./run.sh
```

---

## 🔁 Verificación Final

```bash
docker ps
```

✔️ Deberías ver:

- contenedor8000
- contenedor8181
- contenedor8888

---

## 🌐 Pruebas en navegador (desde la VM)

```bash
xdg-open http://localhost:8000
xdg-open http://localhost:8181
xdg-open http://localhost:8888
```
