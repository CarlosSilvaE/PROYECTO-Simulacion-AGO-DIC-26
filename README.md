# Simulador de inglés

Aplicación web desarrollada con Flask para realizar pruebas de práctica y un examen final de inglés. Los usuarios pueden registrarse, iniciar sesión, responder preguntas con tiempo limitado y consultar su resultado. La información se guarda en MySQL.

## Requisitos

- Docker Desktop
- Python 3.12 o superior para ejecutar sin Docker
- MySQL 8.x si se ejecuta sin Docker

## Instalación y ejecución con Docker

1. Abre una terminal en la carpeta del proyecto.

2. Crea el archivo de variables de entorno:

   **Windows (PowerShell):**
   ```powershell
   Copy-Item .env.example .env
   ```

   **Linux / macOS:**
   ```bash
   cp .env.example .env
   ```

3. Edita `.env` y cambia las contraseñas de ejemplo:

   ```env
   SECRET_KEY=cambia-esta-clave-por-una-aleatoria
   DB_PASSWORD=cambia-esta-contrasena
   MYSQL_ROOT_PASSWORD=cambia-esta-contrasena-root
   ```

4. Construye y inicia los servicios:

   ```bash
   docker compose up --build -d
   ```

5. Abre `http://localhost:5000`.

Docker inicia dos servicios:

- `web`: aplicación Flask servida por Gunicorn en el puerto 5000.
- `db`: MySQL, inicializado con `database/simulador_ingles_mysql.sql`.

Los datos se conservan en el volumen `mysql_data`. Para detener el sistema:

```bash
docker compose down
```

Para eliminar los datos y volver a crear la base:

```bash
docker compose down -v
docker compose up --build -d
```

## Instalación con entorno virtual

Esta opción ejecuta Flask directamente y requiere una instancia de MySQL disponible.

### Windows (PowerShell)

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Configura en la terminal las variables necesarias para acceder a MySQL:

```powershell
$env:DB_HOST = "localhost"
$env:DB_PORT = "3306"
$env:DB_USER = "simulador"
$env:DB_PASSWORD = "tu-contrasena"
$env:DB_NAME = "simulador_ingles"
$env:SECRET_KEY = "cambia-esta-clave"
```

Importa primero el esquema y los datos de `database/simulador_ingles_mysql.sql` en MySQL. Después inicia el servidor:

```bash
python run.py
```

Abre `http://localhost:5000`.

## Funcionamiento

1. El usuario se registra o inicia sesión.
2. Puede realizar una prueba de práctica.
3. Puede presentar el examen final.
4. Las respuestas, los intentos, las calificaciones y el nivel obtenido se almacenan en MySQL.

La prueba final usa el modo completo de 40 preguntas de forma predeterminada. Para una ejecución corta de prueba, configura `FINAL_MODO_PRUEBA=1`.
