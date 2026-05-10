# 🔑 Paso 2: Crear Proyecto y Generar Token en SonarCloud
### Asignatura: Modelos de Evaluación de Software
---
> ⬅️ **Anterior:** `01_CREAR_CUENTA_SONARCLOUD.md`
> ➡️ **Siguiente:** `03_CLONAR_Y_ESCANEAR.md`
---
## Objetivo
En este paso crearás un **proyecto manual** en SonarCloud para el repositorio `Vulnerable-API` y generarás el **token de autenticación** que el escáner usará para enviar los resultados. Al final tendrás:
- Un `Organization Key` (creado en el paso anterior)
- Un `Project Key` (clave del proyecto)
- Un `Token` de autenticación
Guarda los tres valores, los necesitarás en el siguiente paso.
---
## 1. Iniciar Sesión en SonarCloud
Abre tu navegador en:
```
https://sonarcloud.io
```
Si no estás logueado, ingresa con la cuenta de GitHub que usaste en el paso anterior.
---
## 2. Crear un Nuevo Proyecto
### 2.1 Navega a la creación de proyectos
En la barra superior, haz clic en el botón **"+"** y selecciona:
```
+ Analyze new project
```
O accede directamente a:
```
https://sonarcloud.io/projects/create
```
### 2.2 Selecciona el método de análisis
SonarCloud te ofrecerá dos rutas:
| Opción | Descripción |
|---|---|
| **Analyze a repository from GitHub** | Importa un repo de GitHub (requiere fork del Vulnerable-API o repo propio) |
| **Analyze a project manually** | Crea el proyecto sin vincular un repo (recomendado para esta práctica) |
Como vamos a clonar y analizar el repositorio `michealkeines/Vulnerable-API` localmente desde nuestra máquina, elegiremos la opción manual.
> 💡 La opción manual aparece en la parte inferior de la pantalla con el texto:
> *"Analyze a project manually"* o *"Create project manually"*.
Haz clic en esa opción.
### 2.3 Completar la información del proyecto
Llena los campos con los siguientes valores **exactos**:
| Campo | Valor |
|---|---|
| **Organization** | `lab-mes-tu-usuario` (la creada en el paso 1) |
| **Project Key** | `vulnerable-api` |
| **Display name** | `Vulnerable-API` |
| **Visibility** | `Public` |
> ⚠️ Solo los proyectos **públicos** son gratis en el plan Free de SonarCloud.
> 📝 En SonarCloud, el `Project Key` real internamente puede ser `lab-mes-tu-usuario_vulnerable-api` (con prefijo de organización). Anota el valor que SonarCloud te muestre.
Haz clic en **"Next"** → **"Create Project"**.
### 2.4 Configuración de la nueva versión de código
En la siguiente pantalla SonarCloud te preguntará cómo definir la **"New Code"** (código nuevo a comparar):
Selecciona: **"Previous version"** (compara con el último análisis).
Haz clic en **"Save"**.
---
## 3. Generar el Token de Autenticación
Después de crear el proyecto, SonarCloud te llevará a una pantalla para configurar el método de análisis.
### 3.1 Selecciona el método de análisis
Verás varias opciones:
- GitHub Actions
- Other CI (Bitbucket Pipelines, GitLab CI, Azure DevOps, Jenkins, etc.)
- **Manually (with the SonarScanner CLI)**
Selecciona **"Manually"** o **"With SonarScanner CLI"**.

### 3.2 Generar el token
SonarCloud te mostrará una sección para crear un token específico del proyecto:
1. Dado que el proyecto a analizar tiene Python y otros, Selecciona **Other(for Go, PHP...)**
2. Elije la opción que corresponda según tu sistema operativo. 
3. Verás aparecer el token, algo similar a:
```
9583e_XXXXXXXXXXXXXXXXXXXXXXXXXXXX 
```
> 🚨 **IMPORTANTE:** Copia este token **ahora mismo** y guárdalo en un lugar seguro (gestor de contraseñas, archivo de texto local, variable de entorno). **SonarCloud no te lo mostrará de nuevo.**
### 3.3 Continuar la configuración
Una vez copiado el token:
1. Haz clic en **"Continue"**
2. SonarCloud te mostrará un **comando de ejemplo** con los parámetros listos para copiar.
Algo similar a:

#### Linux / macOS
```bash
sonar-scanner \
  -Dsonar.organization=lab-mes-jestrada \
  -Dsonar.projectKey=vulnerable-api
```

#### Windows (PowerShell)
```powershell
sonar-scanner.bat `
  -D"sonar.organization=lab-mes-jestrada" `
  -D"sonar.projectKey=vulnerable-api"
```


```

```
> 📝 **No lo ejecutes todavía.** Lo usaremos en el siguiente paso, cuando ya tengamos el repositorio clonado.
---
## 4. Alternativa: Generar Token desde la Configuración de Usuario
Si perdiste el token o necesitas crear uno nuevo, puedes generarlo desde tu perfil:
1. Haz clic en el ícono de tu usuario (esquina superior derecha)
2. Selecciona **"My Account"**
3. Ve a la pestaña **"Security"**
4. En la sección **"Tokens"**:
   - **Name:** `lab-token-2`
   - **Expiration:** `de manera predeterminada asigna 60 dias`
5. Haz clic en **"Generate"**
6. Copia el token generado
```
https://sonarcloud.io/account/security
```
---
## 5. Resumen de Valores a Guardar
Antes de continuar, confirma que tienes anotados estos valores:
```
┌─────────────────────────────────────────────────────────────┐
│  DATOS PARA EL ESCANEO - GUÁRDALOS AHORA                    │
│                                                             │
│  URL de SonarCloud: https://sonarcloud.io                   │
│  Organization Key:  lab-mes-tu-usuario                      │
│  Project Key:       lab-mes-tu-usuario_vulnerable-api       │
│  Token:             9583e_XXXXXXXXXXXXXXXXXXXXXXXXXXXX      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```
> 💡 El **Project Key** que muestra la interfaz puede variar. Usa exactamente el que te muestre SonarCloud en la pantalla de configuración del escáner.
---
## 6. Verificar que el Proyecto Existe
Ve a la página de proyectos de tu organización:
```
https://sonarcloud.io/organizations/lab-mes-tu-usuario/projects
```
Debes ver el proyecto **"Vulnerable-API"** listado con el estado "Not analyzed yet" (aún no analizado). Eso es correcto por ahora.
---
## ✅ Verificación del Paso 2
```
[ ] Proyecto "Vulnerable-API" creado dentro de tu organización
[ ] Tienes anotados Organization Key y Project Key
[ ] Token generado y copiado en lugar seguro
[ ] El proyecto aparece en sonarcloud.io/organizations/.../projects
```
---
## 🔐 Buenas Prácticas con el Token
- ❌ **Nunca** subas el token a un repositorio público (ni siquiera en `sonar-project.properties`).
- ✅ Usa **variables de entorno** o secretos de CI/CD para guardarlo.
- ✅ Si se filtra, **revócalo** desde `sonarcloud.io/account/security` y genera uno nuevo.
- ✅ Para CI/CD, usa **tokens con expiración corta** y rótalos periódicamente.
---
## ➡️ Siguiente Paso
**📄 `03_CLONAR_Y_ESCANEAR.md`** → Clonar el repositorio Vulnerable-API y ejecutar el análisis
