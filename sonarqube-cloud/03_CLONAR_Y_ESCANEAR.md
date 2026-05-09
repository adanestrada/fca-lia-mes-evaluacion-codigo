# 🚀 Paso 3: Clonar el Repositorio y Ejecutar el Escaneo
### Asignatura: Modelos de Evaluación de Software
---
> ⬅️ **Anterior:** `02_GENERAR_TOKEN_SONARCLOUD.md`
> ➡️ **Siguiente:** `04_INTERPRETAR_RESULTADOS.md`
---
## Objetivo
En este paso clonarás el repositorio `Vulnerable-API` y ejecutarás el escáner de Sonar para que analice el código fuente y envíe los resultados a tu instancia de **SonarCloud** en la nube.
---
## 1. Clonar el Repositorio
Abre una terminal (o PowerShell en Windows) y clona el repositorio en tu máquina.
#### Linux / macOS
```bash
# Ve a tu directorio home o a donde prefieras trabajar
cd ~
# Clona el repositorio
git clone https://github.com/michealkeines/Vulnerable-API.git
# Entra a la carpeta del repositorio
cd Vulnerable-API
```
#### Windows (PowerShell)
```powershell
# Ve a tu directorio de documentos o a donde prefieras
cd $HOME\Documents
# Clona el repositorio
git clone https://github.com/michealkeines/Vulnerable-API.git
# Entra a la carpeta del repositorio
cd Vulnerable-API
```
**Verifica la estructura del repositorio clonado:**
```bash
# Linux / macOS
ls -la
# Windows
dir
```
Deberías ver archivos de Python (`.py`) u otros archivos de código fuente del proyecto.
---
## 2. Crear el Archivo de Configuración del Escáner
El archivo `sonar-project.properties` le indica al escáner qué analizar y cómo conectarse a **SonarCloud**. Debes crearlo **dentro de la carpeta del repositorio clonado**.
> 📝 Reemplaza los valores `TU_ORG_KEY`, `TU_PROJECT_KEY` y `TU_TOKEN_AQUI` con los datos que copiaste en el paso anterior.
#### Linux / macOS
```bash
# Asegúrate de estar dentro de la carpeta Vulnerable-API
pwd  # Debe mostrar .../Vulnerable-API
cat > sonar-project.properties << 'EOF'
# ===================================================
# Configuración del Proyecto SonarCloud
# ===================================================
# Organización en SonarCloud (creada en el paso 1)
sonar.organization=TU_ORG_KEY
# Identificador único del proyecto (debe coincidir con el creado en SonarCloud)
sonar.projectKey=TU_PROJECT_KEY
# Nombre que se mostrará en la interfaz de SonarCloud
sonar.projectName=Vulnerable-API
# Versión del proyecto (puedes cambiarla libremente)
sonar.projectVersion=1.0
# Directorio raíz del código fuente a analizar
# El punto (.) significa el directorio actual
sonar.sources=.
# URL del servicio en la nube (SonarCloud)
sonar.host.url=https://sonarcloud.io
# Token de autenticación (reemplaza con tu token real)
sonar.token=TU_TOKEN_AQUI
# Codificación del código fuente
sonar.sourceEncoding=UTF-8
# Carpetas a excluir del análisis (opcional pero recomendado)
sonar.exclusions=**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**
EOF
```
#### Windows (PowerShell)
Copia y pega el siguiente bloque **completo** en PowerShell. Reemplaza los valores marcados:
```powershell
$orgKey       = "TU_ORG_KEY"        # <-- Tu Organization Key
$projectKey   = "TU_PROJECT_KEY"    # <-- Tu Project Key
$tokenValue   = "TU_TOKEN_AQUI"     # <-- Tu token real
@"
# ===================================================
# Configuración del Proyecto SonarCloud
# ===================================================
# Organización en SonarCloud
sonar.organization=$orgKey
# Identificador único del proyecto
sonar.projectKey=$projectKey
# Nombre que se mostrará en la interfaz de SonarCloud
sonar.projectName=Vulnerable-API
# Versión del proyecto
sonar.projectVersion=1.0
# Directorio raíz del código fuente
sonar.sources=.
# URL del servicio en la nube
sonar.host.url=https://sonarcloud.io
# Token de autenticación
sonar.token=$tokenValue
# Codificación del código fuente
sonar.sourceEncoding=UTF-8
# Carpetas a excluir del análisis
sonar.exclusions=**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**
"@ | Out-File -FilePath sonar-project.properties -Encoding utf8
```
**Verifica que el archivo se creó correctamente:**
```bash
# Linux / macOS
cat sonar-project.properties
# Windows
type sonar-project.properties
```
> 🚨 **Recuerda:** Si vas a subir este repo a GitHub, **agrega `sonar-project.properties` a `.gitignore`** o quita la línea del token y pásalo por argumento (Opción B más abajo).
---
## 3. Alternativa: Pasar el Token por Variable de Entorno (Recomendado)
Para no guardar el token en disco, puedes exportarlo como variable de entorno. Sonar reconoce la variable `SONAR_TOKEN` automáticamente.
#### Linux / macOS
```bash
# Exporta el token solo en esta sesión de terminal
export SONAR_TOKEN="sqp_xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
# Crea el archivo SIN la línea sonar.token
cat > sonar-project.properties << 'EOF'
sonar.organization=TU_ORG_KEY
sonar.projectKey=TU_PROJECT_KEY
sonar.projectName=Vulnerable-API
sonar.projectVersion=1.0
sonar.sources=.
sonar.host.url=https://sonarcloud.io
sonar.sourceEncoding=UTF-8
sonar.exclusions=**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**
EOF
```
#### Windows (PowerShell)
```powershell
# Solo en la sesión actual
$env:SONAR_TOKEN = "sqp_xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```
---
## 4. Ejecutar el Escaneo
Asegúrate de estar dentro de la carpeta `Vulnerable-API` antes de ejecutar cualquiera de las siguientes opciones.
### Opción A: Con token en `sonar-project.properties` (más simple)
#### Linux / macOS
```bash
sonar-scanner
```
#### Windows (PowerShell)
```powershell
sonar-scanner.bat
```
---
### Opción B: Con token como argumento (más seguro)
Reemplaza `TU_TOKEN_AQUI` con tu token real.
#### Linux / macOS
```bash
sonar-scanner \
  -Dsonar.token=TU_TOKEN_AQUI
```
#### Windows (PowerShell)
```powershell
sonar-scanner.bat `
  -Dsonar.token=TU_TOKEN_AQUI
```
---
### Opción C: Todos los parámetros por línea de comandos (sin archivo de configuración)
Útil si no quieres crear el archivo `sonar-project.properties`.
#### Linux / macOS
```bash
sonar-scanner \
  -Dsonar.organization=TU_ORG_KEY \
  -Dsonar.projectKey=TU_PROJECT_KEY \
  -Dsonar.projectName="Vulnerable-API" \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonarcloud.io \
  -Dsonar.token=TU_TOKEN_AQUI \
  -Dsonar.sourceEncoding=UTF-8 \
  -Dsonar.exclusions="**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**"
```
#### Windows (PowerShell)
```powershell
sonar-scanner.bat `
  -Dsonar.organization=TU_ORG_KEY `
  -Dsonar.projectKey=TU_PROJECT_KEY `
  -Dsonar.projectName="Vulnerable-API" `
  -Dsonar.sources=. `
  -Dsonar.host.url=https://sonarcloud.io `
  -Dsonar.token=TU_TOKEN_AQUI `
  -Dsonar.sourceEncoding=UTF-8 `
  -Dsonar.exclusions="**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**"
```
---
## 5. Progreso del Escaneo
Mientras el escáner corre, verás una salida similar a esta en la terminal:
```
INFO: Scanner configuration file: .../sonar-scanner.properties
INFO: Project root configuration file: .../sonar-project.properties
INFO: SonarScanner CLI 6.x.x
INFO: Java 17 OpenJDK 64-Bit Server VM
INFO: User cache: /home/user/.sonar/cache
INFO: Communicating with SonarCloud
INFO: Load global settings
INFO: Load global settings (done) | time=320ms
INFO: Server id: SONARCLOUD
INFO: Load quality profiles
INFO: Load quality profiles (done) | time=180ms
INFO: Load active rules
INFO: Load active rules (done) | time=2800ms
INFO: Indexing files...
INFO: Project configuration:
INFO:   Excluded sources: **/__pycache__/**, **/*.pyc, **/node_modules/**, **/.git/**
INFO: 23 files indexed
INFO: Quality profile for py: Sonar way
INFO: ------------- Run sensors on module Vulnerable-API
INFO: Python Sensor [python]
INFO: Sensor Python Sensor [python] (done) | time=7200ms
INFO: ------------- Run sensors on module Vulnerable-API
INFO: Execute project builders
INFO: 23/23 source files have been analyzed
INFO: ANALYSIS SUCCESSFUL, you can find the results at:
INFO:   https://sonarcloud.io/dashboard?id=TU_PROJECT_KEY
INFO: Note that you will be able to access the updated dashboard once the server has processed the submitted analysis report
INFO: More about the report processing at https://sonarcloud.io/api/ce/task?id=XXXXXXXXXXXXXXXXXXXXXXXX
INFO: Analysis total time: 38.421 s
INFO: ------------------------------------------------------------------------
INFO: EXECUTION SUCCESS
INFO: ------------------------------------------------------------------------
```
> ✅ El mensaje **"EXECUTION SUCCESS"** indica que el escaneo fue exitoso y los resultados fueron enviados a SonarCloud.
> ⚠️ El procesamiento en la nube puede tardar entre 10 y 60 segundos adicionales después del "EXECUTION SUCCESS". Refresca el dashboard si aún no ves los datos.
---
## 6. Verificar que los Resultados Llegaron a SonarCloud
1. Abre tu navegador en `https://sonarcloud.io`
2. Ve a tu organización → **Projects**
3. Haz clic en el proyecto **"Vulnerable-API"**
4. Verás el **dashboard del proyecto** con las métricas de calidad
Atajo directo:
```
https://sonarcloud.io/dashboard?id=TU_PROJECT_KEY
```
Si ves el proyecto con datos (número de bugs, vulnerabilidades, etc.), ¡el escaneo fue exitoso!
---
## 🔧 Solución de Problemas Comunes
| Problema | Causa probable | Solución |
|---|---|---|
| `sonar-scanner: command not found` | PATH no configurado | Verifica que `/opt/sonar-scanner/bin` esté en `$PATH` (o usa `brew install sonar-scanner` en macOS) |
| `Authentication failed` (401) | Token incorrecto, expirado o revocado | Genera un nuevo token desde `https://sonarcloud.io/account/security` |
| `Project not found` (404) | Project Key u Organization Key incorrectos | Verifica que coincidan **exactamente** con los que aparecen en SonarCloud |
| `You're not authorized to run analysis` | Token sin permisos sobre el proyecto | Usa un **Project Analysis Token** del proyecto correcto, no de otro |
| `Unable to load component class` / SSL error | Proxy corporativo o firewall | Configura el proxy con `-Dhttp.proxyHost=...` o desde una red sin restricciones |
| `No files to analyze` | Ruta incorrecta de `sonar.sources` | Asegúrate de ejecutar el escáner desde la raíz del repositorio |
| `Error: Java not found` | Java no está en el PATH | Instala JDK 17 y agrega el directorio `bin` al PATH |
| Análisis lento o se queda colgado | Conexión a Internet lenta | El escáner sube datos a la nube; necesitas conectividad estable |
---
## ✅ Verificación del Paso 3
```
[ ] Repositorio clonado en tu máquina
[ ] Archivo sonar-project.properties creado (o parámetros preparados)
[ ] sonar-scanner ejecutado sin errores
[ ] Salida muestra "EXECUTION SUCCESS"
[ ] El proyecto aparece con datos en sonarcloud.io
```
---
## ➡️ Siguiente Paso
**📄 `04_INTERPRETAR_RESULTADOS.md`** → Leer e interpretar el reporte de calidad generado
