# Suite‑Zero3D Develop
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

---

## 📑 Índice

* [✨ Descripción breve](#-descripción-breve)
* [🖼️ Capturas](#️-capturas)
* [🚀 Inicio rápido!](#-comenzar)
* [🐳 Ejecutando con Docker Compose](#-ejecutar-con-docker)
* [🛠️ Entorno de desarrollo](#️-entorno-de-desarrollo)
* [📂 Estructura del proyecto](#estructura-del-proyecto)
* [⚙️ Variables de entorno](#️-variables-del-entorno)
* [🔌 API Reference](#-api-reference)
* [🧪 Tests](#-tests)
* [🤝 Contribuir](#-contribuir-al-proyecto)
* [📄 Licencia](#-licencia)

---

## ✨ Descripción breve

Suite‑Zero3D es una suite de aplicaciones orientadas a facilitar la gestión de pedidos y producción posterior para empresas dedicadas a la impresión 3D.

Rama develop!

---

## 🖼️ Capturas

Ejemplos de la web:
![Ejemplo1](docs/img/Ejemplo1.png)
![Ejemplo2](docs/img/Ejemplo2.png)

---

## 🚀 Comenzar

```bash
# 0. Prerequisitos (Docker y docker-compose):
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


# 1. Clonamos el repositorio para desarrollo:
$ git clone --branch develop --recurse-submodules https://github.com/JulianCabanillas/Suite-Zero3D.git


# 2. Entramos en el directorio raíz del proyecto:
$ cd Suite-Zero3D


# 3. Comprobar que estamos en la rama develop:
$ git status  # Debes estar en develop, sino:
$ git checkout develop # !! CAMBIA LA RAMA EN TODO LOS SUBMODULOS TAMBIÉN !!


# 4. Arranca todo en modo desarrollo
$ docker compose up --build


# 5. Accedemos al front desde el navegador:
#    http://localhost:3000
```

> **Tip:** Servicios:
 - Puerto 80 (Nginx) 
 - Puerto 3000 (frontend dev) -> http://localhost:3000
 - Puerto 8000 (backend)
 - Puerto 5050 (PGAdmin)
 - Puerto 9000 (Portainer).
  Puertos accesibles desde http://localhost

---

## 🐳 Ejecutar con Docker

Podemos incluir un nombre especifico de archivo con -f 
Si queremos ademas omitir el log de arranque usamos -d

```bash
# Arranque con -f y -d:
$ docker compose -f docker-compose.yml up -d --build


# Podemos logear en vivo un contendor especifico con:
$ docker compose logs -f backend


# Para hacer stop de los contenedores y eliminar:
$ docker compose down --volumes --remove-orphans --rmi local
```

La orden --volumes elimina los volumenes generados, la orden --remove-orphans para los huerfanos y --rmi all para las imagenes.

---

## 🛠️ Entorno de desarrollo

Si en algun momento queremos levantar un servicio indepemdiente debemos saber:

```bash
# Frontend con la opcion hot‑reload:
$ cd Client-Front-Zero3D
$ sudo npm install
$ sudo npm run dev
# Asi tendremos el Front ejecutandose solo y con hot-reload


# Backend con un .env para desarrollar con las dependencias
# Primero el entorno virtual:
$ sudo apt install python3-pip       # → Actualizamos libreria PIP
$ sudo apt install python3-venv      # → Entorno virtual python para trabajar con las dependencias
$ sudo apt install 
$ sudo apt upgrade -y                # → Actualizamos todo lo descargado.
$ python3 -m venv env                # → Crear entorno, una vez !!.
$ source env/bin/activate            # → Para iniciar entorno virtual.

$ deactivate                         # → Para cerrar entorno virtual.

# Con el entorno abierto hacemos lo siguiente:
$ pip install -r requirements.txt    # → Instalamos todos los requerimientos para el entorno.
$ docker compose up -d backend  # → Iniciamos el servidor backend con una base de datos provisional para que no se interrumpa, ya podemos desarrollar en caliente.

$ docker compose down --volumes --remove-orphans --rmi all # → Parar y eliminar todo.
```

---

## 📂 Estructura del proyecto

```
Suite‑Zero3D/
├─ docker-compose.yml
├─ Client-Front-Zero3D/     # Preact + Vite
│  └─ src/
├─ Client-Back-Zero3D/      # Django + DRF
│  └─ api/
└─ Nginx/                   # En entorno develop no se utiliza.
   └─ default.conf
```

| Directorio             | Descripción breve                  |
| ---------------------- | ---------------------------------- |
| `Client-Front-Zero3D/` | Código de Preact y librerías       |
| `Client-Back-Zero3D/`  | API REST + lógica de la aplicación |
| `Nginx/`               | Configuración de proxy y estáticos |

---

## ⚙️ Variables del entorno

Desde la raiz del modulo Client-Back-Zero3D, podemos copiar `.env.example` en la raíz con el nombre de `.env.development`. Tendremos lo siguiente:

```env.develpment
SECRET_KEY='django-insecure-gc9w9#$!#3p$46*#0kfghec86c0w-08l8ez1$cw-sjp81=br#'
DB_NAME=zero3d_dev_db
DB_USER=admin
DB_PASSWORD=admin
DB_HOST=db
DB_PORT=5432

```

---

## 🔌 Api Reference

| Método | Endpoint                 | Descripción                      |
| ------ | ------------------------ | -------------------------------- |
| `POST` | `/api/login_client/`     | Autenticación y obtención de JWT |
| `POST` | `/api/register_client/`  | Registra y posterior hace login  |
| `POST` | `/api/register_order/`   | Registra pedido con el usuario   |
| `POST` | `/api/calculator/`       | Calcula precio de impresión 3D   |

---

## 🧪 Tests

```bash
# Backend
$ docker compose exec backend pytest -q

# Frontend
$ docker compose exec frontend npm test
```

---

## 🤝 Contribuir al proyecto

1. Haz un fork del proyecto.
2. Crea tu rama: `git checkout -b develop/mi-feature`.
3. Haz commit de tus cambios: `git commit -m "feat: añade mi feature"`.
4. Sube la rama: `git push origin develop/mi-feature`.
5. Abre un Pull Request.

---

## 📄 Licencia

Este proyecto se distribuye bajo la licencia **GNU General Public License**. Consulta el archivo `LICENSE` para más información.
