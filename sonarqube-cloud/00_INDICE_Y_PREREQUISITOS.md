# ☁️ Guía Completa: Análisis de Calidad de Código con SonarCloud (Sonar en la Nube)
### Repositorio: `michealkeines/Vulnerable-API`
### Asignatura: Modelos de Evaluación de Software
---
## 📚 Estructura de la Guía
Esta guía está dividida en los siguientes archivos para facilitar su lectura:
| Archivo | Contenido |
|---|---|
| `00_INDICE_Y_PREREQUISITOS.md` | ← Estás aquí. Índice general y requisitos previos |
| `01_CREAR_CUENTA_SONARCLOUD.md` | Crear cuenta y organización en SonarCloud |
| `02_GENERAR_TOKEN_SONARCLOUD.md` | Crear proyecto y generar token en SonarCloud |
| `03_CLONAR_Y_ESCANEAR.md` | Clonar el repositorio y ejecutar el análisis |
| `04_INTERPRETAR_RESULTADOS.md` | Leer e interpretar el reporte de calidad |
> ⚠️ **Sigue los archivos en orden numérico.** Cada paso depende del anterior.
---
## ☁️ ¿Qué es SonarCloud?
**SonarCloud** (también conocido como **SonarQube Cloud**) es la versión SaaS de SonarQube alojada por SonarSource. Ventajas frente a la versión local:
- ✅ **No necesita Docker** ni servidor propio.
- ✅ **No requiere PostgreSQL** ni configuración de infraestructura.
- ✅ **Gratis para proyectos públicos** alojados en GitHub, GitLab, Bitbucket o Azure DevOps.
- ✅ Acceso desde cualquier navegador en `https://sonarcloud.io`.
- ✅ Siempre actualizado con las últimas reglas de calidad.
> 💡 La parte que **sí** sigue siendo local es el **escáner** (`sonar-scanner`), que analiza el código en tu máquina y envía los resultados al servicio en la nube.
---
## 🧰 Prerequisitos del Sistema
Antes de comenzar, asegúrate de tener instalados los siguientes programas en tu máquina.
### 1. Cuenta en GitHub (u otra plataforma soportada)
SonarCloud usa autenticación a través de un proveedor externo. Las opciones son:
- **GitHub** (recomendado para esta práctica)
- GitLab
- Bitbucket Cloud
- Azure DevOps
Si no tienes cuenta de GitHub, créala gratis en: https://github.com/signup
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
El escáner de SonarCloud (`sonar-scanner`) requiere Java para ejecutarse.
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
Es la herramienta de línea de comandos que analiza el código y envía los resultados a SonarCloud.
#### Windows
1. Descarga el ZIP desde: https://docs.sonarsource.com/sonarqube-cloud/advanced-setup/ci-based-analysis/sonarscanner-cli/
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
> 💡 En **macOS** también puedes instalar el escáner con Homebrew:
> ```bash
> brew install sonar-scanner
> ```
---
## ✅ Checklist de Prerequisitos
Antes de continuar, confirma que todo funciona:
```
[ ] Cuenta de GitHub (u otro proveedor) activa
[ ] Conexión estable a Internet
[ ] git --version           → muestra versión de Git
[ ] java -version           → muestra Java 17 o superior
[ ] sonar-scanner --version → muestra versión del escáner
```
---
## 🆚 Comparativa: SonarQube Local vs SonarCloud
| Aspecto | SonarQube Local (Docker) | SonarCloud (Nube) |
|---|---|---|
| Infraestructura | Docker + PostgreSQL en tu máquina | Alojado por SonarSource |
| Costo | Gratis (Community Edition) | Gratis para repos públicos |
| Instalación | ~10 min de configuración | 0 min (solo crear cuenta) |
| Mantenimiento | Tú actualizas y respaldas | Automático |
| URL del servicio | `http://localhost:9000` | `https://sonarcloud.io` |
| Acceso desde otros equipos | No (solo tu red) | Sí (público) |
| Privacidad del código | Total (todo es local) | El código se analiza en la nube |
---
## ➡️ Siguiente Paso
Cuando tengas todos los prerequisitos instalados, continúa con:
**📄 `01_CREAR_CUENTA_SONARCLOUD.md`** → Crear tu cuenta en SonarCloud