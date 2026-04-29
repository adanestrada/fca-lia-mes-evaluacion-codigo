# 🚀 Paso 3: Clonar el Repositorio y Ejecutar el Escaneo
### Asignatura: Modelos de Evaluación de Software

---

> ⬅️ **Anterior:** `02_GENERAR_TOKEN_SONARQUBE.md`
> ➡️ **Siguiente:** `04_INTERPRETAR_RESULTADOS.md`

---

## Objetivo

En este paso clonarás el repositorio `Vulnerable-API` y ejecutarás el escáner de SonarQube para que analice el código fuente y envíe los resultados a tu instancia local de SonarQube.

---

## 1. Clonar el Repositorio

Abre una terminal (o PowerShell en Windows) y clona el repositorio en una ubicación diferente a tu carpeta `sonarqube-lab`.

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

El archivo `sonar-project.properties` le indica al escáner qué analizar y cómo conectarse a SonarQube. Debes crearlo **dentro de la carpeta del repositorio clonado**.

> 📝 Reemplaza `TU_TOKEN_AQUI` con el token que copiaste en el paso anterior.

#### Linux / macOS
```bash
# Asegúrate de estar dentro de la carpeta Vulnerable-API
pwd  # Debe mostrar .../Vulnerable-API

cat > sonar-project.properties << 'EOF'
# ===================================================
# Configuración del Proyecto SonarQube
# ===================================================

# Identificador único del proyecto (debe coincidir con el Project Key creado en SonarQube)
sonar.projectKey=vulnerable-api

# Nombre que se mostrará en la interfaz de SonarQube
sonar.projectName=Vulnerable-API

# Versión del proyecto (puedes cambiarla libremente)
sonar.projectVersion=1.0

# Directorio raíz del código fuente a analizar
# El punto (.) significa el directorio actual
sonar.sources=.

# URL de tu instancia local de SonarQube
sonar.host.url=http://localhost:9000

# Token de autenticación (reemplaza con tu token real)
sonar.token=TU_TOKEN_AQUI

# Codificación del código fuente
sonar.sourceEncoding=UTF-8

# Carpetas a excluir del análisis (opcional pero recomendado)
sonar.exclusions=**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**

# Lenguaje del proyecto (Python en este caso)
sonar.language=py
EOF
```

#### Windows (PowerShell)

Copia y pega el siguiente bloque **completo** en PowerShell. Reemplaza `TU_TOKEN_AQUI` con tu token real:

```powershell
$tokenValue = "TU_TOKEN_AQUI"  # <-- Reemplaza esto con tu token real

@"
# ===================================================
# Configuración del Proyecto SonarQube
# ===================================================

# Identificador único del proyecto
sonar.projectKey=vulnerable-api

# Nombre que se mostrará en la interfaz de SonarQube
sonar.projectName=Vulnerable-API

# Versión del proyecto
sonar.projectVersion=1.0

# Directorio raíz del código fuente
sonar.sources=.

# URL de tu instancia local de SonarQube
sonar.host.url=http://localhost:9000

# Token de autenticación
sonar.token=$tokenValue

# Codificación del código fuente
sonar.sourceEncoding=UTF-8

# Carpetas a excluir del análisis
sonar.exclusions=**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**

# Lenguaje del proyecto
sonar.language=py
"@ | Out-File -FilePath sonar-project.properties -Encoding utf8
```

**Verifica que el archivo se creó correctamente:**
```bash
# Linux / macOS
cat sonar-project.properties

# Windows
type sonar-project.properties
```

---

## 3. Alternativa: Pasar el Token por Línea de Comandos

Si prefieres no guardar el token en un archivo (recomendado en entornos compartidos), puedes pasarlo directamente como parámetro. En este caso, crea el archivo `sonar-project.properties` **sin** la línea del token:

#### Linux / macOS
```bash
cat > sonar-project.properties << 'EOF'
sonar.projectKey=vulnerable-api
sonar.projectName=Vulnerable-API
sonar.projectVersion=1.0
sonar.sources=.
sonar.host.url=http://localhost:9000
sonar.sourceEncoding=UTF-8
sonar.exclusions=**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**
sonar.language=py
EOF
```

Y al ejecutar el escáner, agrega el token como argumento (ver Sección 4, Opción B).

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
  -Dsonar.projectKey=vulnerable-api \
  -Dsonar.projectName="Vulnerable-API" \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.token=TU_TOKEN_AQUI \
  -Dsonar.sourceEncoding=UTF-8 \
  -Dsonar.exclusions="**/__pycache__/**,**/*.pyc,**/node_modules/**,**/.git/**"
```

#### Windows (PowerShell)
```powershell
sonar-scanner.bat `
  -Dsonar.projectKey=vulnerable-api `
  -Dsonar.projectName="Vulnerable-API" `
  -Dsonar.sources=. `
  -Dsonar.host.url=http://localhost:9000 `
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
INFO: SonarScanner 6.x.x
INFO: Java 17 OpenJDK 64-Bit Server VM
INFO: User cache: /home/user/.sonar/cache
INFO: Analyzing on SonarQube server 10.x
INFO: Load global settings
INFO: Load global settings (done) | time=250ms
INFO: Server id: XXXXXXXX
INFO: Load quality profiles
INFO: Load quality profiles (done) | time=120ms
INFO: Load active rules
INFO: Load active rules (done) | time=3500ms
INFO: Indexing files...
INFO: Project configuration:
INFO:   Excluded sources: **/__pycache__/**, **/*.pyc, **/node_modules/**, **/.git/**
INFO: 23 files indexed
INFO: Quality profile for py: Sonar way
INFO: ------------- Run sensors on module Vulnerable-API
INFO: Load metrics repository
INFO: Python Sensor [python]
INFO: Sensor Python Sensor [python] (done) | time=8500ms
INFO: ------------- Run sensors on module Vulnerable-API
INFO: Execute project builders
INFO: 23/23 source files have been analyzed
INFO: ANALYSIS SUCCESSFUL, you can find the results at: http://localhost:9000/dashboard?id=vulnerable-api
INFO: Note that you will be able to access the updated dashboard once the server has processed the submitted analysis report
INFO: More about the report processing at http://localhost:9000/api/ce/task?id=XXXXXXXXXXXXXXXXXXXXXXXX
INFO: Analysis total time: 45.876 s
INFO: ------------------------------------------------------------------------
INFO: EXECUTION SUCCESS
INFO: ------------------------------------------------------------------------
```

> ✅ El mensaje **"EXECUTION SUCCESS"** indica que el escaneo fue exitoso y los resultados fueron enviados a SonarQube.

---

## 6. Verificar que los Resultados Llegaron a SonarQube

1. Abre tu navegador en `http://localhost:9000`
2. Ve a **Projects** en el menú superior
3. Haz clic en el proyecto **"Vulnerable-API"**
4. Verás el **dashboard del proyecto** con las métricas de calidad

Si ves el proyecto con datos (número de bugs, vulnerabilidades, etc.), ¡el escaneo fue exitoso!

---

## 🔧 Solución de Problemas Comunes

| Problema | Causa probable | Solución |
|---|---|---|
| `sonar-scanner: command not found` | PATH no configurado | Verifica que `/opt/sonar-scanner/bin` esté en `$PATH` |
| `Authentication failed` | Token incorrecto o expirado | Genera un nuevo token desde `http://localhost:9000/account/security` |
| `Project not found` | Project key no coincide | Verifica que `sonar.projectKey=vulnerable-api` coincida exactamente con el creado |
| `Connection refused` | SonarQube no está corriendo | Ejecuta `docker compose start` desde la carpeta `sonarqube-lab` |
| `No files to analyze` | Ruta incorrecta de `sonar.sources` | Asegúrate de ejecutar el escáner desde la raíz del repositorio |
| `Error: Java not found` | Java no está en el PATH | Instala JDK 17 y agrega el directorio `bin` al PATH |

---

## ✅ Verificación del Paso 3

```
[ ] Repositorio clonado en tu máquina
[ ] Archivo sonar-project.properties creado (o parámetros preparados)
[ ] sonar-scanner ejecutado sin errores
[ ] Salida muestra "EXECUTION SUCCESS"
[ ] El proyecto aparece con datos en http://localhost:9000/projects
```

---

## ➡️ Siguiente Paso

**📄 `04_INTERPRETAR_RESULTADOS.md`** → Leer e interpretar el reporte de calidad generado
