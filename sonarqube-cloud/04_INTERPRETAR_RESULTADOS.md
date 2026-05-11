# 📊 Paso 4: Interpretar los Resultados del Análisis
### Asignatura: Modelos de Evaluación de Software
---
> ⬅️ **Anterior:** `03_CLONAR_Y_ESCANEAR.md`
> 🏁 **Fin de la guía**
---
## Objetivo
En este paso aprenderás a navegar el dashboard de **SonarCloud**, entender las métricas reportadas y relacionarlas con los conceptos de calidad de software vistos en clase (ISO/IEC 25000).
---
## 1. El Dashboard Principal del Proyecto
Abre:
```
https://sonarcloud.io/dashboard?id=TU_PROJECT_KEY
```
O navega desde:
```
sonarcloud.io → Tu organización → Projects → Vulnerable-API
```
El dashboard muestra el **Quality Gate** (Puerta de Calidad), que es el veredicto general del análisis:
```
┌────────────────────────────────────────────────────────────┐
│  Quality Gate                                              │
│                                                            │
│  ● PASSED  ✅  (el código cumple los estándares)           │
│  ● FAILED  ❌  (el código no cumple los estándares)        │
└────────────────────────────────────────────────────────────┘
```
Debajo del Quality Gate verás las métricas divididas en categorías. SonarCloud muestra dos vistas:
- **Overall Code:** todo el código del proyecto.
- **New Code:** solo el código añadido o modificado desde el último análisis (útil en CI/CD).



> 🚨 **IMPORTANTE:**  La razón principal por la que SonarCloud (y SonarQube) no incluye todas las reglas de seguridad avanzadas, como la detección profunda de Inyección SQL (SQL Injection), en su versión gratuita o comunitaria es una estrategia de diferenciación de producto. Sonar Community +1SonarSource, la empresa detrás de la herramienta, ofrece las capacidades básicas de análisis de calidad de código de forma gratuita, pero reserva el análisis avanzado de seguridad **(conocido como SAST - Static Application Security Testing)** para sus planes de pago.  En la siguiente práctica abordaremos la implementación de estas reglas con herramientas de código abierto.

> 🚨 En esta guía se presentará como referencia los issues de seguridad como si tuviésemos una versión de paga de Sonar sólo para referencia. Esto será diferente a lo que ustedes visualicen en sus reporte de su análisis con su cuenta de Sonar. Es solo referencia.


---
## 2. Métricas Principales y su Significado
### 🐛 Bugs (Fiabilidad)
Problemas que **definitivamente causarán un comportamiento incorrecto** en producción.
| Calificación | Significado |
|---|---|
| A | 0 bugs |
| B | Al menos 1 bug menor |
| C | Al menos 1 bug importante |
| D | Al menos 1 bug crítico |
| E | Al menos 1 bug bloqueante |
**Ejemplo en Vulnerable-API:** Una variable que puede ser `None` y se usa sin verificación previa.
---
### 🔒 Vulnerabilities (Seguridad)
Puntos del código que podrían ser **explotados por un atacante**.
Categorías comunes encontradas en APIs vulnerables:
| Tipo | Descripción | Ejemplo |
|---|---|---|
| SQL Injection | Consultas SQL construidas con inputs del usuario | `query = "SELECT * FROM users WHERE id = " + user_input` |
| Hardcoded Credentials | Contraseñas escritas en el código | `password = "admin123"` |
| Insecure Deserialization | Deserialización de datos no confiables | `pickle.loads(user_data)` |
| Path Traversal | Acceso a archivos arbitrarios del sistema | `open(user_path)` sin sanitizar |
**Cómo verlas:**
1. En el dashboard, haz clic en el número de **Vulnerabilities**
2. Verás la lista de vulnerabilidades con su archivo, línea y descripción
3. Haz clic en cualquiera para ver el detalle y la explicación
---
### 🛡️ Security Hotspots (Revisión de Seguridad)
A diferencia de las Vulnerabilities, los Hotspots son **código que requiere revisión humana** para determinar si es un riesgo real. No son definitivamente bugs, pero merecen atención.
**Ejemplo:** Uso de algoritmos de hash (puede ser correcto o incorrecto según el contexto).
En SonarCloud encuentras los Hotspots en una pestaña dedicada: **Security Hotspots**.
---
### 💩 Code Smells (Mantenibilidad)
Problemas de **calidad del código** que no causan errores directamente pero dificultan el mantenimiento.
| Tipo | Descripción |
|---|---|
| Duplicated Code | Código repetido que debería estar en una función |
| Long Method | Funciones con demasiadas líneas |
| Magic Numbers | Números sin nombre ni explicación en el código |
| Dead Code | Código que nunca se ejecuta |
| Unused Variables | Variables declaradas pero no usadas |
---
### 📏 Coverage (Cobertura de Pruebas)
Porcentaje del código cubierto por pruebas automatizadas.
> ⚠️ **Nota:** Para que SonarCloud muestre la cobertura, el proyecto necesita tener pruebas y un reporte de cobertura generado (por ejemplo, `coverage.xml` con `pytest-cov`). Si el repositorio `Vulnerable-API` no tiene tests, este valor aparecerá como `0%` o `–`, lo cual es en sí mismo un hallazgo importante.
**Relación con el laboratorio:**
- La cobertura de **líneas** y **ramas** que calculaste manualmente en el `lab_manual_test_coverage_calculation.md` corresponde a las métricas `Line Coverage` y `Branch Coverage` que SonarCloud calcula automáticamente.
---
### 📦 Duplications (Duplicación de Código)
Porcentaje de líneas que son código duplicado. Un porcentaje alto indica que el código es difícil de mantener.
---
## 3. Navegar por las Issues (Problemas Encontrados)
Haz clic en la pestaña **"Issues"** en el menú del proyecto:
```
https://sonarcloud.io/project/issues?id=TU_PROJECT_KEY
```
### Filtros Disponibles
En el panel izquierdo puedes filtrar por:
| Filtro | Opciones |
|---|---|
| **Type** | Bug, Vulnerability, Code Smell |
| **Severity** | Blocker, Critical, Major, Minor, Info |
| **Status** | Open, Confirmed, Resolved, Won't Fix |
| **File** | Archivo específico del proyecto |
| **Rule** | Regla de Sonar que lo detectó |
| **Clean Code Attribute** | Categoría del nuevo modelo Clean Code |
### Entender una Issue Individual
Haz clic en cualquier issue para ver:
```
┌─────────────────────────────────────────────────────────────┐
│  [VULNERABILITY] SQL Injection                    CRITICAL   │
│                                                             │
│  app.py, línea 42                                           │
│                                                             │
│  42  │  query = "SELECT * FROM users WHERE id = " + user_id │
│      │  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^       │
│                                                             │
│  Why is this an issue?                                      │
│  User-controlled data should not be used directly in SQL   │
│  queries. Construct the query with parameterized inputs.    │
│                                                             │
│  How to fix it:                                             │
│  Use parameterized queries or prepared statements.          │
│                                                             │
│  Tags: cwe, injection, owasp-a1                             │
└─────────────────────────────────────────────────────────────┘
```
---
## 4. Ver el Análisis por Archivo
Haz clic en la pestaña **"Code"**:
```
https://sonarcloud.io/code?id=TU_PROJECT_KEY
```
Aquí puedes explorar el repositorio y ver qué archivos tienen más problemas. El color de cada archivo indica su estado:
- 🟢 Verde: Pocos o ningún problema
- 🟡 Amarillo: Algunos problemas menores
- 🔴 Rojo: Problemas críticos
Haz clic en cualquier archivo para ver el **código fuente anotado** con los problemas marcados directamente en las líneas afectadas.
---
## 5. Relación con ISO/IEC 25000 (SQuaRE)
Las métricas de SonarCloud se alinean directamente con el modelo de calidad de la norma ISO/IEC 25010:
| Característica ISO 25010 | Métrica SonarCloud |
|---|---|
| **Fiabilidad** → Tolerancia a fallos | Bugs, Reliability Rating |
| **Seguridad** → Integridad | Vulnerabilities, Security Rating |
| **Mantenibilidad** → Modificabilidad | Code Smells, Technical Debt |
| **Mantenibilidad** → Reusabilidad | Duplications (%) |
| **Fiabilidad** → Madurez | Coverage (%), Tests |
---
## 6. Technical Debt (Deuda Técnica)
En la sección de **Code Smells** verás el concepto de **"Technical Debt"** expresado en horas o días. Representa el tiempo estimado que tomaría corregir todos los problemas de mantenibilidad.
**Ejemplo de interpretación:**
```
Technical Debt: 3h 45min
→ Significa que un desarrollador tardaría aproximadamente
  3 horas y 45 minutos en limpiar todo el código problemático
  del proyecto según las reglas configuradas.
```
---
## 7. Preguntas de Reflexión para el Laboratorio
Basándote en los resultados del escaneo de `Vulnerable-API`, responde:
1. **¿Cuántas vulnerabilidades de seguridad encontró SonarCloud?** ¿Cuáles son las más críticas?
2. **¿Cuál es la calificación de Fiabilidad (Reliability Rating)?** ¿Qué bugs fueron detectados?
3. **¿Hay código duplicado?** ¿En qué archivos?
4. **¿Cuál es el porcentaje de cobertura de pruebas?** Si es 0%, ¿qué implicaciones tiene esto para la calidad del software?
5. **Relaciona** al menos 2 vulnerabilidades encontradas por SonarCloud con los conceptos de la práctica de cálculo manual de cobertura. ¿Qué pruebas habrían sido necesarias para detectar esos bugs antes de llegar a producción?
6. **Ventaja de la nube:** ¿Qué beneficios prácticos te dio SonarCloud frente a una instalación local de SonarQube? ¿Y qué desventajas identificas (privacidad, dependencia de Internet, etc.)?
---
## 8. Exportar el Reporte (Opcional)
SonarCloud permite consumir resultados a través de su **API REST pública**. La autenticación se hace con el mismo token del paso 2.
### Exportar Issues en JSON via API
#### Linux / macOS
```bash
# Reemplaza TU_TOKEN y TU_PROJECT_KEY con tus valores reales
curl -u TU_TOKEN: \
  "https://sonarcloud.io/api/issues/search?componentKeys=TU_PROJECT_KEY&ps=500" \
  -o reporte-issues.json
```
#### Windows (PowerShell)
```powershell
$token = "TU_TOKEN"
$projectKey = "TU_PROJECT_KEY"
$pair = "${token}:"
$bytes = [System.Text.Encoding]::ASCII.GetBytes($pair)
$base64 = [System.Convert]::ToBase64String($bytes)
$headers = @{ Authorization = "Basic $base64" }
Invoke-WebRequest `
  -Uri "https://sonarcloud.io/api/issues/search?componentKeys=$projectKey&ps=500" `
  -Headers $headers `
  -OutFile "reporte-issues.json"
```
### Ver Métricas del Proyecto via API
#### Linux / macOS
```bash
curl -u TU_TOKEN: \
  "https://sonarcloud.io/api/measures/component?component=TU_PROJECT_KEY&metricKeys=bugs,vulnerabilities,code_smells,coverage,duplicated_lines_density,reliability_rating,security_rating,sqale_rating" \
  | python3 -m json.tool
```
#### Windows (PowerShell)
```powershell
$token = "TU_TOKEN"
$projectKey = "TU_PROJECT_KEY"
$pair = "${token}:"
$bytes = [System.Text.Encoding]::ASCII.GetBytes($pair)
$base64 = [System.Convert]::ToBase64String($bytes)
$headers = @{ Authorization = "Basic $base64" }
$metricas = "bugs,vulnerabilities,code_smells,coverage,duplicated_lines_density,reliability_rating,security_rating,sqale_rating"
Invoke-WebRequest `
  -Uri "https://sonarcloud.io/api/measures/component?component=$projectKey&metricKeys=$metricas" `
  -Headers $headers | Select-Object -ExpandProperty Content | ConvertFrom-Json | ConvertTo-Json -Depth 10
```
### Badges del Proyecto
SonarCloud genera **badges** (escudos) que puedes pegar en el README de tu repositorio. Ve a:
```
Tu proyecto → Information (icono ⓘ) → Get project badges
```
Ejemplo:
```markdown
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=TU_PROJECT_KEY&metric=alert_status)](https://sonarcloud.io/dashboard?id=TU_PROJECT_KEY)
```
---
## 9. Re-ejecutar el Análisis
Si modificas el código o la configuración y quieres volver a escanear:
1. Asegúrate de estar en la carpeta `Vulnerable-API`
2. Ejecuta nuevamente:
```bash
# Linux / macOS
sonar-scanner
# Windows
sonar-scanner.bat
```
SonarCloud guardará un **historial completo** de cada análisis. Puedes ver la evolución temporal de métricas en la pestaña **"Activity"** del proyecto.
---
## 10. Integración Continua (Opcional)
Una de las grandes ventajas de SonarCloud es que se integra de manera nativa con plataformas de CI/CD. Puedes automatizar el escaneo en cada `push` o `pull request`.
### Ejemplo: GitHub Actions
Crea el archivo `.github/workflows/sonarcloud.yml` en tu repo:
```yaml
name: SonarCloud
on:
  push:
    branches: [ main ]
  pull_request:
    types: [opened, synchronize, reopened]
jobs:
  sonarcloud:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: SonarCloud Scan
        uses: SonarSource/sonarcloud-github-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```
Luego, en tu repo de GitHub, agrega el secreto `SONAR_TOKEN` con el token de SonarCloud:
```
GitHub → Settings → Secrets and variables → Actions → New repository secret
```
> 💡 Esto permite que **cada Pull Request** sea analizado automáticamente y reciba comentarios con los hallazgos directamente en GitHub.
---
## ✅ Checklist Final del Laboratorio
```
[ ] SonarCloud accesible en https://sonarcloud.io con tu cuenta
[ ] Proyecto "Vulnerable-API" analizado exitosamente
[ ] Revisé las Vulnerabilities del proyecto
[ ] Revisé los Bugs del proyecto
[ ] Revisé los Code Smells y la Deuda Técnica
[ ] Identifiqué la cobertura de pruebas (Coverage)
[ ] Respondí las preguntas de reflexión de la Sección 7
[ ] Relacioné los resultados con la norma ISO/IEC 25010
```
---
## 🧹 Limpiar al Terminar (Opcional)
Como SonarCloud vive en la nube, no hay contenedores que detener. Si deseas eliminar tu trabajo:
### Borrar el proyecto
```
Tu proyecto → Administration → Deletion → Delete Project
```
### Borrar la organización completa
```
Tu organización → Administration → Delete Organization
```
### Revocar el token (recomendado al finalizar la práctica)
```
https://sonarcloud.io/account/security
→ Encuentra "lab-token" → Click en "Revoke"
```
> 🔐 **Buena práctica:** Siempre revoca tokens que ya no uses, aunque hayas terminado el laboratorio.
---
## 📚 Referencias
- [Documentación oficial de SonarCloud](https://docs.sonarsource.com/sonarqube-cloud/)
- [SonarCloud Metric Definitions](https://docs.sonarsource.com/sonarqube-cloud/digging-deeper/metric-definitions/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — Relacionado con vulnerabilidades detectadas
- [ISO/IEC 25010](https://iso25000.com/index.php/normas-iso-25000/iso-25010) — Modelo de calidad del producto software
- [SonarScanner CLI para SonarCloud](https://docs.sonarsource.com/sonarqube-cloud/advanced-setup/ci-based-analysis/sonarscanner-cli/)
- [SonarCloud GitHub Action](https://github.com/SonarSource/sonarcloud-github-action)