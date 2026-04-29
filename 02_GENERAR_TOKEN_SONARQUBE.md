# 🔑 Paso 2: Crear Proyecto y Generar Token en SonarQube
### Asignatura: Modelos de Evaluación de Software

---

> ⬅️ **Anterior:** `01_SETUP_DOCKER_SONARQUBE.md`
> ➡️ **Siguiente:** `03_CLONAR_Y_ESCANEAR.md`

---

## Objetivo

En este paso crearás un **proyecto manual** en SonarQube para el repositorio `Vulnerable-API` y generarás el **token de autenticación** que el escáner usará para enviar los resultados. Al final tendrás:

- Un `Project Key` (clave del proyecto)
- Un `Token` de autenticación

Guarda ambos valores, los necesitarás en el siguiente paso.

---

## 1. Iniciar Sesión en SonarQube

Abre tu navegador en:
```
http://localhost:9000
```

Ingresa con las credenciales que configuraste:
- **Usuario:** `admin`
- **Contraseña:** La que estableciste al cambiarla (ej. `Admin1234!`)

---

## 2. Crear un Nuevo Proyecto

### 2.1 Navega a la Creación de Proyectos

En la barra superior, haz clic en el botón **"+"** o ve directamente a:
```
http://localhost:9000/projects/create
```

### 2.2 Selecciona "Create a local project"

Verás tres opciones:
- DevOps Platform Integration (GitHub, GitLab, etc.)
- **Create a local project** ← Selecciona esta opción
- Import an existing project

### 2.3 Completa la Información del Proyecto

Llena los campos con los siguientes valores **exactos**:

| Campo | Valor |
|---|---|
| **Project display name** | `Vulnerable-API` |
| **Project key** | `vulnerable-api` |
| **Main branch name** | `main` |

> ⚠️ El **Project key** es el identificador único del proyecto. Anótalo, lo necesitarás más adelante.

Haz clic en **"Next"**.

### 2.4 Configuración de Análisis

En la siguiente pantalla te preguntará cómo quieres definir la **nueva configuración de código**:

Selecciona: **"Use the global setting"**

Haz clic en **"Create project"**.

---

## 3. Generar el Token de Autenticación

Después de crear el proyecto, SonarQube te llevará a una pantalla para configurar el método de análisis.

### 3.1 Selecciona el Método de Análisis

Verás varias opciones:
- CI/CD tools
- Locally
- Other

Selecciona **"Locally"** (análisis local desde tu máquina).

### 3.2 Generar el Token

En la sección **"Provide a token"**:

1. En el campo de nombre del token escribe: `lab-token`
2. Mantén el tipo como **"Project Analysis Token"**
3. En expiración selecciona **"No expiration"** (o 30 días para mayor seguridad)
4. Haz clic en el botón **"Generate"**

Verás aparecer el token, algo similar a:
```
sqp_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8
```

> 🚨 **IMPORTANTE:** Copia este token **ahora mismo** y guárdalo en un lugar seguro (bloc de notas, archivo de texto). **SonarQube no te lo mostrará de nuevo.**

### 3.3 Continuar la Configuración

Una vez copiado el token:
1. Haz clic en **"Continue"**
2. En la siguiente pantalla, selecciona el lenguaje principal del proyecto: **"Other"** (ya que el proyecto usa Python/JavaScript mixto)
3. Selecciona tu sistema operativo: **"Linux"**, **"Windows"** o **"macOS"**

SonarQube te mostrará un comando de ejemplo. **No lo ejecutes todavía**, lo usaremos en el siguiente paso con los parámetros correctos.

---

## 4. Alternativa: Generar Token desde la Configuración de Usuario

Si perdiste el token o necesitas crear uno nuevo, puedes generarlo desde tu perfil:

1. Haz clic en el ícono de tu usuario (esquina superior derecha)
2. Selecciona **"My Account"**
3. Ve a la pestaña **"Security"**
4. En la sección **"Generate Tokens"**:
   - **Name:** `lab-token-2`
   - **Type:** `User Token`
   - **Expiration:** `No expiration`
5. Haz clic en **"Generate"**
6. Copia el token generado

```
http://localhost:9000/account/security
```

---

## 5. Resumen de Valores a Guardar

Antes de continuar, confirma que tienes anotados estos valores:

```
┌─────────────────────────────────────────────────────────────┐
│  DATOS PARA EL ESCANEO - GUÁRDALOS AHORA                    │
│                                                             │
│  URL de SonarQube:  http://localhost:9000                   │
│  Project Key:       vulnerable-api                          │
│  Token:             sqp_XXXXXXXXXXXXXXXXXXXXXXXXXXXX        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 6. Verificar que el Proyecto Existe

Ve a la página principal de proyectos:
```
http://localhost:9000/projects
```

Debes ver el proyecto **"Vulnerable-API"** listado con el estado "Never analyzed" (nunca analizado). Eso es correcto por ahora.

---

## ✅ Verificación del Paso 2

```
[ ] Proyecto "Vulnerable-API" creado con Project Key "vulnerable-api"
[ ] Token generado y copiado en lugar seguro
[ ] El proyecto aparece en http://localhost:9000/projects
```

---

## ➡️ Siguiente Paso

**📄 `03_CLONAR_Y_ESCANEAR.md`** → Clonar el repositorio Vulnerable-API y ejecutar el análisis
