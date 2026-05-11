# ☁️ Paso 1: Crear Cuenta y Organización en SonarCloud

### Asignatura: Modelos de Evaluación de Software
---
> ⬅️ **Anterior:** `00_INDICE_Y_PREREQUISITOS.md`
> ➡️ **Siguiente:** `02_GENERAR_TOKEN_SONARCLOUD.md`
---
## Objetivo
En este paso crearás una cuenta en **SonarCloud** (la versión SaaS de SonarQube), configurarás una **organización** y la conectarás con tu cuenta de GitHub. Al final tendrás acceso al panel web de SonarCloud listo para recibir análisis.
---
## 1. Acceder a SonarCloud
Abre tu navegador y entra a:
```
https://sonarcloud.io
```
Verás la página de inicio con un botón **"Log in"** (esquina superior derecha) y opciones para iniciar sesión:
```
┌──────────────────────────────────────────┐
│         SonarCloud                       │
│                                          │
│   Inicia sesión con:                     │
│                                          │
│   [  GitHub  ]                           │
│   [  Bitbucket  ]                        │
│   [  GitLab  ]                           │
│   [  Azure DevOps  ]                     │
│                                          │
└──────────────────────────────────────────┘
```
---
## 2. Crear la Cuenta con GitHub
Para esta práctica usaremos **GitHub** como proveedor de identidad.
### 2.1 Iniciar sesión con GitHub
1. Haz clic en el botón **"With GitHub"** o **"Log in with GitHub"**.
2. Si aún no estás logueado en GitHub, te pedirá usuario y contraseña.
3. GitHub mostrará una pantalla de autorización pidiendo permisos a SonarCloud:
```
┌─────────────────────────────────────────────────┐
│  Authorize SonarCloud                           │
│                                                 │
│  SonarCloud wants to access:                    │
│   • Your email addresses (read-only)            │
│   • Your public repositories                    │
│                                                 │
│        [ Authorize SonarSource ]                │
└─────────────────────────────────────────────────┘
```
4. Haz clic en **"Authorize SonarSource"**.
> ⚠️ Estos permisos iniciales son **solo de lectura**. Más adelante, al agregar el repositorio, GitHub te pedirá permisos adicionales para ese repo específico.
---
### 2.2 Aceptar Términos y Condiciones
La primera vez que entres, SonarCloud te pedirá:
1. Aceptar los **Términos y Condiciones**.
2. Confirmar tu correo electrónico.
3. Indicar si quieres recibir comunicaciones (opcional).
Marca las casillas obligatorias y haz clic en **"Continue"**.
---
## 3. Crear una Organización en SonarCloud
En SonarCloud, los proyectos viven dentro de una **organización**. Cada usuario debe pertenecer al menos a una.
### 3.1 Iniciar la creación
Después de iniciar sesión, SonarCloud te llevará a una pantalla de bienvenida. Haz clic en el botón **"+"** (esquina superior derecha) y selecciona:
```
+ Create an organization
```
O accede directamente a:
```
https://sonarcloud.io/projects/create
```
### 3.2 Elegir el método de creación
Verás dos opciones:
| Opción | Cuándo usarla |
|---|---|
| **Import an organization from GitHub** | Si trabajarás con repositorios alojados en GitHub (recomendado) |
| **Create one manually** | Si analizarás código que no está en GitHub o quieres una organización vacía |
Para esta práctica selecciona: **"Create an organization manually"** (parte inferior de la pantalla, en letra pequeña: *"You can also create an organization manually"*).
### 3.3 Completar los datos de la organización
| Campo | Valor sugerido |
|---|---|
| **Name** | `lab-mes-tu-usuario` (ej. `lab-mes-jestrada`) |
| **Key** | Se autocompleta a partir del nombre. Anótalo. |
| **Plan** | **Free plan** (gratis para proyectos públicos) |
> ⚠️ El **Organization Key** es global en todo SonarCloud, así que debe ser único. Si está tomado, agrega tus iniciales o un número.
Haz clic en **"Continue"** → **"Create Organization"**.
---
## 4. Verificar la Organización
Después de crearla, llegarás al panel de la organización:
```
https://sonarcloud.io/organizations/lab-mes-tu-usuario/projects
```
Verás un mensaje similar a:
```
┌───────────────────────────────────────────────┐
│  No projects yet                              │
│                                               │
│  Analyze a new project to get started.        │
│                                               │
│        [ Analyze new project ]                │
└───────────────────────────────────────────────┘
```
Esto es correcto. Crearemos el proyecto en el siguiente paso.
---
## 5. Resumen de Valores a Guardar
Antes de continuar, anota estos datos:
```
┌─────────────────────────────────────────────────────────────┐
│  DATOS DE TU CUENTA SONARCLOUD                              │
│                                                             │
│  URL:                  https://sonarcloud.io                │
│  Usuario (GitHub):     tu-usuario-github                    │
│  Organization Key:     lab-mes-tu-usuario                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```
---
## ✅ Verificación del Paso 1
Confirma lo siguiente antes de continuar:
```
[ ] Iniciaste sesión en sonarcloud.io con tu cuenta de GitHub
[ ] Aceptaste los términos y verificaste tu correo
[ ] Creaste una organización con plan Free
[ ] Anotaste el Organization Key
[ ] Puedes ver el dashboard vacío de tu organización
```
---
## 🔧 Solución de Problemas Comunes
| Problema | Causa probable | Solución |
|---|---|---|
| GitHub no autoriza a SonarCloud | Bloqueador de pop-ups o cookies de terceros | Permite cookies/pop-ups de `sonarcloud.io` y vuelve a intentar |
| "Organization key already taken" | Otra persona ya usó ese identificador | Cambia el sufijo (agrega año, número o tus iniciales) |
| Solo veo el plan de pago | Estás creando una org. con repos privados | Selecciona **"Free plan"** (asegúrate de que el repo a analizar sea público) |
| No me deja crear la organización | Sesión expirada de GitHub | Cierra sesión en SonarCloud, vuelve a iniciar y reintenta |
---
## ➡️ Siguiente Paso
**📄 `02_GENERAR_TOKEN_SONARCLOUD.md`** → Crear el proyecto y generar el token de autenticación
