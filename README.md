# Suite‑Zero3D Prod

![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Gunicorn](https://img.shields.io/badge/gunicorn-%298729.svg?style=for-the-badge&logo=gunicorn&logoColor=white)
![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white)
![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge\&logo=postgresql\&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-%23009639.svg?style=for-the-badge\&logo=nginx\&logoColor=white)
![build](https://img.shields.io/badge/build-passing-brightgreen)
![license](https://img.shields.io/badge/license-MIT-blue)


> **Este README está realizado para el entorno *producción*.**
> A diferencia de la verisión para *Stage*, aquí el **frontend está servido por Nginx** dentro del mismo contenedor por lo que el docker stack no levanta ningun servicio de frontend.
> Las imágenes del frontend ya vienen pre‑compiladas en la propia imagen Docker servido por Nginx.

---

## 📑 Índice

* [✨ Descripción breve](#-descripción-breve)
* [🖼️ Capturas](#️-capturas)
* [🚀 Despliegue rápido](#-comenzar)
* [🐳 Ejecutar con Docker Compose](#-ejecutar-con-docker)
* [📂 Estructura de contenedores](#estructura-de-contenedores)
* [⚙️ Variables de entorno](#️-variables-de-entorno)
* [🔌 API Reference](#-api-reference)
* [🧪 Smoke Tests](#-tests)
* [🤝 Contribuir](#-contribuir-al-proyecto)
* [📄 Licencia](#-licencia)

---

## ✨ Descripción breve

Entorno **Productivo** de Suite‑Zero3D.

* **Nginx** sirve la imagen estática de Preact y reenvía `/api/` a **Gunicorn**, el servicio de backend, a través de la propia red interna.
* **Postgres** está siendo ejecutado en segundo plano, **no expuesto** al host: todo el tráfico está dirigido por la red `zero3d_prod_network`.
* **Imágenes pre‑compiladas**  están publicadas en Docker Hub (`juliancabanillas/zero3d-[prod/stage]:<versión>`).

---

## 🖼️ Capturas


Ejemplos de la web:
![Ejemplo1](docs/img/Ejemplo1.png)
![Ejemplo2](docs/img/Ejemplo2.png)

---

## 🚀 Comenzar

### 0. Prerequisitos
```bash
$ sudo apt remove docker docker-engine docker.io containerd runc   # → Eliminar versiones antiguas.
$ sudo apt update
$ sudo apt upgrade -y 
$ sudo apt install -y ca-certificates curl gnupg lsb-release

$ sudo mkdir -p /etc/apt/keyrings                                  # → Agrega clave GPG oficial de Docker.
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null          # → Agrega repositorio oficial.

$ sudo apt update
$ sudo apt upgrade -y 
$ sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 1. Clonar el repositorio (rama `main`)
```bash
# 1. Clonar sólo la rama prod, usara imagenes de DockerHub, descargamos submodulos por archivos de con.
$ git clone --branch main --recurse-submodules https://github.com/JulianCabanillas/Suite-Zero3D.git
$ cd Suite-Zero3D

# 2. (Opc.) Actualizar a la última versión de las imágenes o actualizar repositorios desde remoto:
$ docker compose -f docker-compose-prod.yml pull
# ó
$ git pull origin main
```

### 2. Crear archivo `.env.production`

```bash
$ cp Client-Back-Zero3D/.env.example Client-Back-Zero3D/.env.production
$ nano .env.production
# Descomenta la parte de production!!
```

### 3. Descargar las imágenes y levanta los servicions

```bash
$ docker stack deploy -c docker-stack-prod.yml zero3d_prod
```

### 4. Entrar

| Servicio    | URL                                            |
| ----------- | ---------------------------------------------- |
| Front + API | [http://localhost:80](http://localhost:80)     |
| PGAdmin     | [http://localhost:5050](http://localhost:5050) |
| Portainer   | [http://localhost:9000](http://localhost:9000) |


> **Nota** : Los únicos puertos expuestos al host son `443` (TLS) y opcionalmente `80` para redirección a HTTPS,las demás conexiones son privadas internamente.

> En prod no hay **hot‑reload** ni puerto 3000; todo pasa por Nginx.
Cuando se levanta por primera vez Portainer hay que crear rapidamente usuario, tiene tiempo determinado.

> **Tip:** Servicios:
 - Puerto 80   (Nginx) -> http://stage.zero3d.shop:8080
 - Puerto 3000 (frontend dev) 
 - Puerto 8000 (backend)
 - Puerto 5050 (PGAdmin) -> http://stage.zero3d.shop:5051
 - Puerto 9000 (Portainer). -> http://stage.zero3d.shop:9001

---

## 🐳 Ejecutar con Docker

| Acción              | Comando                                                                                             | Qué hace                                                                         |
| ------------------- | --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Arrancar todo       | `docker stack deploy -c docker-stack-prod.yml zero3d_prod`                                                   | Inicia Nginx, backend, postgres, portainer, también actualiza y reinicia contenedores con cero downtime.                                      |
| Parar               | `docker stack rm zero3d` | Despliega drain y elimina todos los servicios, redes y secretos. |
| Limpiar imagenes    | `docker system prune -af --volumes`                                                    | Borra capas e imágenes no utilizadas, incl. volúmenes anónimos.                                            |

---

## 📂 Estructura de contenedores

```
Suite‑Zero3D/
├─ docker-compose-stage.yml        # Este archivo orquesta el entorno
├─ Client-Front-Zero3D/            # Preact (compilado ⇢ /dist en la imagen)
├─ Client-Back-Zero3D/             # Django + DRF (código fuente — opcional hack)
└─ Nginx/                          # Configuración del proxy
```

---

| Servicio      | Imagen                                 | Puertos host       | Notas                                    |
| ------------- | -------------------------------------- | ------------------ | ---------------------------------------- |
| **nginx**     | `juliancabanillas/zero3d-nginx:prod-v000`   | 80 ↦ 80, 443 ↦ 443 | Sirve frontend y actúa de proxy inverso. |
| **backend**   | `juliancabanillas/zero3d-backend:prod-v000` | *interno*          | Ejecuta Gunicorn y Django.               |
| **db**        | `postgres:15-alpine`                   | *interno*          | Volumen `postgres_data_prod`.            |
| **portainer** | `portainer/portainer-ce:2.20`          | 9443 ↦ 9443        | (Opc.) panel de administración.          |

---

## ⚙️ Variables de entorno

Las imágenes esperan un archivo `.env.production` en la raíz que podemos copiar y renombrar desde `.env-example` descomentando a postarior las partes de production:

```dotenv
# Core Django
SECRET_KEY="<cadena‑32‑chars>"
DJANGO_DEBUG=False
ALLOWED_HOSTS=app.zero3d.com,.zero3d.com

# Base de datos
POSTGRES_DB=zero3d_prod_db
POSTGRES_USER=zero3d_admin
POSTGRES_PASSWORD=<strong-pass>
POSTGRES_HOST=db
POSTGRES_PORT=5432

# Seguridad
CORS_ALLOWED_ORIGINS=https://app.zero3d.com,https://zero3d.com
CSRF_TRUSTED_ORIGINS=https://app.zero3d.com
```
---

## 🔌 API Reference

| Método | Endpoint                | Descripción               |
| ------ | ----------------------- | ------------------------- |
| `POST` | `/api/login_client/`    | Autenticación JWT         |
| `POST` | `/api/register_client/` | Registro y login          |
| `POST` | `/api/register_order/`  | Crear pedido              |
| `POST` | `/api/calculator/`      | Calcular precio impresión |

---

## 🧪 Tests

Si es necesario se puede ejecutar un test rápido para comprobar que todo responde correctamente:

```bash
curl -fsSL https://app.zero3d.com/api/health/ || echo "Backend no responde"
curl -fsSL https://app.zero3d.com | grep -q "<title>Zero3D" && echo "Front OK"
```

---

## 🤝 Contribuir al proyecto

Las contribuciones se realizaran en la rama develop, consulta dicha rama para mas información.

---

## 📄 Licencia

Distribuido bajo **GNU GPL v3** — consulta `LICENSE` para los detalles.
