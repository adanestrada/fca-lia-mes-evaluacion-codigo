# 🐳 Paso 1: Levantar SonarQube con Docker
### Asignatura: Modelos de Evaluación de Software

---

> ⬅️ **Anterior:** `00_INDICE_Y_PREREQUISITOS.md`
> ➡️ **Siguiente:** `02_GENERAR_TOKEN_SONARQUBE.md`

---

## Objetivo

En este paso levantaremos **SonarQube Community Edition** y su base de datos **PostgreSQL** usando Docker Compose. Al final tendrás la interfaz web de SonarQube corriendo localmente en tu navegador.

---

## 1. Crear la Carpeta de Trabajo

Crea una carpeta dedicada para este laboratorio. **No es el repositorio a analizar**, sino el directorio donde vive tu infraestructura.

#### Linux / macOS
```bash
mkdir ~/sonarqube-lab
cd ~/sonarqube-lab
```

#### Windows (PowerShell)
```powershell
mkdir $HOME\sonarqube-lab
cd $HOME\sonarqube-lab
```

---

## 2. Crear el Archivo `docker-compose.yml`

Dentro de la carpeta `sonarqube-lab`, crea el archivo `docker-compose.yml` con el siguiente contenido:

#### Linux / macOS
```bash
cat > docker-compose.yml << 'EOF'
version: "3.8"

services:
  sonarqube:
    image: sonarqube:10-community
    container_name: sonarqube
    depends_on:
      - sonardb
    ports:
      - "9000:9000"
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://sonardb:5432/sonarqube
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
    restart: unless-stopped

  sonardb:
    image: postgres:15
    container_name: sonardb
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonarqube
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
    restart: unless-stopped

volumes:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
  postgresql:
  postgresql_data:
EOF
```

#### Windows (PowerShell)

Copia y pega el siguiente bloque directamente en PowerShell:

```powershell
@"
version: "3.8"

services:
  sonarqube:
    image: sonarqube:10-community
    container_name: sonarqube
    depends_on:
      - sonardb
    ports:
      - "9000:9000"
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://sonardb:5432/sonarqube
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
    restart: unless-stopped

  sonardb:
    image: postgres:15
    container_name: sonardb
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonarqube
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
    restart: unless-stopped

volumes:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
  postgresql:
  postgresql_data:
"@ | Out-File -FilePath docker-compose.yml -Encoding utf8
```

---

## 3. Configuración del Sistema Operativo (Requerida para SonarQube)

SonarQube necesita que el kernel del sistema permita más memoria virtual. **Este paso es obligatorio.**

#### Linux
```bash
# Aumentar el límite de memoria virtual (obligatorio para Elasticsearch)
sudo sysctl -w vm.max_map_count=262144

# Para hacerlo permanente al reiniciar:
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
```

#### macOS
```bash
# En macOS con Docker Desktop, ejecuta dentro del contenedor de Linux:
docker run --rm --privileged alpine sysctl -w vm.max_map_count=262144
```

#### Windows (PowerShell como Administrador)
```powershell
# Docker Desktop en Windows usa WSL2. Configura la memoria virtual en WSL2:
wsl -d docker-desktop -u root sh -c "sysctl -w vm.max_map_count=262144"
```

> ⚠️ **Si omites este paso**, SonarQube fallará al iniciar con un error de Elasticsearch relacionado con `vm.max_map_count`.

---

## 4. Levantar los Contenedores

Asegúrate de estar dentro de la carpeta `sonarqube-lab` y ejecuta:

```bash
# Linux / macOS
docker compose up -d

# Windows (PowerShell o Command Prompt)
docker compose up -d
```

Deberías ver algo similar a:
```
[+] Running 6/6
 ✔ Network sonarqube-lab_default  Created
 ✔ Volume sonarqube_data          Created
 ✔ Volume sonarqube_extensions    Created
 ✔ Volume sonarqube_logs          Created
 ✔ Container sonardb              Started
 ✔ Container sonarqube            Started
```

---

## 5. Verificar que los Contenedores Están Corriendo

```bash
docker ps
```

Debes ver dos contenedores activos:

```
CONTAINER ID   IMAGE                    STATUS          PORTS
xxxxxxxxxxxx   sonarqube:10-community   Up 2 minutes    0.0.0.0:9000->9000/tcp
xxxxxxxxxxxx   postgres:15              Up 2 minutes    5432/tcp
```

---

## 6. Esperar que SonarQube Termine de Iniciar

SonarQube tarda entre **1 y 3 minutos** en estar listo la primera vez (descarga plugins, inicializa la base de datos). Puedes monitorear el progreso:

```bash
# Linux / macOS / Windows (Git Bash o PowerShell)
docker logs -f sonarqube
```

Espera hasta ver este mensaje en los logs:
```
SonarQube is operational
```

Presiona `Ctrl + C` para salir del monitoreo de logs (esto no detiene el contenedor).

---

## 7. Acceder a la Interfaz Web

Abre tu navegador y ve a:

```
http://localhost:9000
```

Deberías ver la pantalla de login de SonarQube:

```
┌─────────────────────────────────────┐
│          SonarQube                  │
│                                     │
│  Login: admin                       │
│  Password: admin                    │
│                                     │
│         [Log In]                    │
└─────────────────────────────────────┘
```

**Credenciales iniciales:**
- **Usuario:** `admin`
- **Contraseña:** `admin`

> 🔐 Al ingresar por primera vez, SonarQube te pedirá **cambiar la contraseña**. Usa una que recuerdes, por ejemplo: `Admin1234!`

---

## 8. Comandos Útiles de Docker

```bash
# Ver estado de los contenedores
docker ps

# Ver logs en tiempo real de SonarQube
docker logs -f sonarqube

# Detener los contenedores (sin borrar datos)
docker compose stop

# Volver a iniciar los contenedores
docker compose start

# Detener y eliminar contenedores (los datos en volúmenes se conservan)
docker compose down

# Detener y eliminar TODO incluyendo volúmenes (⚠️ borra todos los análisis)
docker compose down -v
```

---

## ✅ Verificación del Paso 1

Confirma lo siguiente antes de continuar:

```
[ ] docker ps muestra sonarqube y sonardb en estado "Up"
[ ] http://localhost:9000 carga la pantalla de SonarQube
[ ] Pudiste iniciar sesión con admin / admin
[ ] Cambiaste la contraseña por defecto
```

---

## 🔧 Solución de Problemas Comunes

| Problema | Causa probable | Solución |
|---|---|---|
| Puerto 9000 en uso | Otro proceso usa el puerto | Cambia el mapeo a `"9001:9000"` en el docker-compose.yml |
| SonarQube no inicia (error Elasticsearch) | `vm.max_map_count` muy bajo | Ejecuta el paso 3 de esta guía |
| `docker compose` no reconocido | Docker Compose V1 instalado | Usa `docker-compose` (con guión) en lugar de `docker compose` |
| Página no carga después de 5 min | SonarQube aún inicializando | Espera más tiempo y revisa `docker logs sonarqube` |

---

## ➡️ Siguiente Paso

**📄 `02_GENERAR_TOKEN_SONARQUBE.md`** → Crear el proyecto y generar el token de autenticación
