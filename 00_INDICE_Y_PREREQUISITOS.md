# 🔍 Guía Completa: Análisis de Calidad de Código con SonarQube
### Repositorio: `michealkeines/Vulnerable-API`
### Asignatura: Modelos de Evaluación de Software

---

## 📚 Estructura de la Guía

Esta guía está dividida en los siguientes archivos para facilitar su lectura:

| Archivo | Contenido |
|---|---|
| `00_INDICE_Y_PREREQUISITOS.md` | ← Estás aquí. Índice general y requisitos previos |
| `01_SETUP_DOCKER_SONARQUBE.md` | Levantar SonarQube con Docker |
| `02_GENERAR_TOKEN_SONARQUBE.md` | Crear proyecto y generar token en SonarQube |
| `03_CLONAR_Y_ESCANEAR.md` | Clonar el repositorio y ejecutar el análisis |
| `04_INTERPRETAR_RESULTADOS.md` | Leer e interpretar el reporte de calidad |

> ⚠️ **Sigue los archivos en orden numérico.** Cada paso depende del anterior.

---

## 🧰 Prerequisitos del Sistema

Antes de comenzar, asegúrate de tener instalados los siguientes programas en tu máquina.

### 1. Docker Desktop

SonarQube y su base de datos corren en contenedores Docker. Es obligatorio tenerlo instalado.

**Verificar si ya está instalado:**

```bash
# Linux / macOS / Windows (Git Bash o PowerShell)
docker --version
docker compose version
```

Si obtienes números de versión, ya está instalado. Si no, descárgalo aquí:

- 🔗 **Windows / macOS:** https://www.docker.com/products/docker-desktop
- 🔗 **Linux (Ubuntu/Debian):**
```bash
sudo apt-get update
sudo apt-get install -y docker.io docker-compose-plugin
sudo systemctl start docker
sudo usermod -aG docker $USER
# Cierra sesión y vuelve a entrar para que el grupo surta efecto
```

---

### 2. Git

Para clonar el repositorio a analizar.

**Verificar:**
```bash
git --version
```

**Instalar si hace falta:**

- **Windows:** https://git-scm.com/download/win
- **Linux:**
```bash
sudo apt-get install -y git
```
- **macOS:**
```bash
brew install git
```

---

### 3. Java (JDK 17 o superior)

El escáner de SonarQube (`sonar-scanner`) requiere Java para ejecutarse.

**Verificar:**
```bash
java -version
```

**Instalar si hace falta:**

- **Windows / macOS:** https://adoptium.net/ (descarga JDK 17 LTS)
- **Linux:**
```bash
sudo apt-get install -y openjdk-17-jdk
```

---

### 4. sonar-scanner (CLI)

Es la herramienta de línea de comandos que analiza el código y envía los resultados a SonarQube.

#### Windows

1. Descarga el ZIP desde: https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/
2. Extrae el contenido en `C:\sonar-scanner`
3. Agrega `C:\sonar-scanner\bin` al `PATH` del sistema:
   - Busca **"Variables de entorno"** en el menú inicio
   - En **Variables del sistema**, edita `Path` y agrega la ruta anterior
4. Verifica en una nueva ventana de PowerShell:
```powershell
sonar-scanner --version
```

#### Linux / macOS

```bash
# Descarga y extrae
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-6.2.1.4610-linux-x64.zip
unzip sonar-scanner-cli-6.2.1.4610-linux-x64.zip
sudo mv sonar-scanner-6.2.1.4610-linux-x64 /opt/sonar-scanner

# Agrega al PATH (añade esta línea a ~/.bashrc o ~/.zshrc)
export PATH="$PATH:/opt/sonar-scanner/bin"

# Recarga el perfil
source ~/.bashrc

# Verifica
sonar-scanner --version
```

---

## ✅ Checklist de Prerequisitos

Antes de continuar, confirma que todo funciona:

```
[ ] docker --version        → muestra versión de Docker
[ ] docker compose version  → muestra versión de Compose
[ ] git --version           → muestra versión de Git
[ ] java -version           → muestra Java 17 o superior
[ ] sonar-scanner --version → muestra versión del escáner
```

---

## ➡️ Siguiente Paso

Cuando tengas todos los prerequisitos instalados, continúa con:

**📄 `01_SETUP_DOCKER_SONARQUBE.md`** → Levantar SonarQube con Docker
