# 📊 Paso 4: Interpretar los Resultados del Análisis
### Asignatura: Modelos de Evaluación de Software

---

> ⬅️ **Anterior:** `03_CLONAR_Y_ESCANEAR.md`
> 🏁 **Fin de la guía**

---

## Objetivo

En este paso aprenderás a navegar el dashboard de SonarQube, entender las métricas reportadas y relacionarlas con los conceptos de calidad de software vistos en clase (ISO/IEC 25000).

---

## 1. El Dashboard Principal del Proyecto

Abre:
```
http://localhost:9000/dashboard?id=vulnerable-api
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

Debajo del Quality Gate verás las métricas divididas en categorías:

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

> ⚠️ **Nota:** Para que SonarQube muestre la cobertura, el proyecto necesita tener pruebas y un reporte de cobertura generado. Si el repositorio `Vulnerable-API` no tiene tests, este valor aparecerá como `0%` o `N/A`, lo cual es en sí mismo un hallazgo importante.

**Relación con el laboratorio:**
- La cobertura de **líneas** y **ramas** que calculaste manualmente en el `lab_manual_test_coverage_calculation.md` corresponde a las métricas `Line Coverage` y `Branch Coverage` que SonarQube calcula automáticamente.

---

### 📦 Duplications (Duplicación de Código)
Porcentaje de líneas que son código duplicado. Un porcentaje alto indica que el código es difícil de mantener.

---

## 3. Navegar por las Issues (Problemas Encontrados)

Haz clic en la pestaña **"Issues"** en el menú del proyecto:

```
http://localhost:9000/project/issues?id=vulnerable-api
```

### Filtros Disponibles

En el panel izquierdo puedes filtrar por:

| Filtro | Opciones |
|---|---|
| **Type** | Bug, Vulnerability, Code Smell, Security Hotspot |
| **Severity** | Blocker, Critical, Major, Minor, Info |
| **Status** | Open, Confirmed, Resolved, Won't Fix |
| **File** | Archivo específico del proyecto |
| **Rule** | Regla de SonarQube que lo detectó |

### Entender una Issue Individual

Haz clic en cualquier issue para ver:

```
┌─────────────────────────────────────────────────────────────┐
│  [VULNERABILITY] SQL Injection                    CRITICAL   │
│                                                             │
│  app.py, línea 42                                           │
│                                                             │
│  42  │  query = "SELECT * FROM users WHERE id = " + user_id │
│      │  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^       │
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
http://localhost:9000/code?id=vulnerable-api
```

Aquí puedes explorar el repositorio y ver qué archivos tienen más problemas. El color de cada archivo indica su estado:
- 🟢 Verde: Pocos o ningún problema
- 🟡 Amarillo: Algunos problemas menores
- 🔴 Rojo: Problemas críticos

Haz clic en cualquier archivo para ver el **código fuente anotado** con los problemas marcados directamente en las líneas afectadas.

---

## 5. Relación con ISO/IEC 25000 (SQuaRE)

Las métricas de SonarQube se alinean directamente con el modelo de calidad de la norma ISO/IEC 25010:

| Característica ISO 25010 | Métrica SonarQube |
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

1. **¿Cuántas vulnerabilidades de seguridad encontró SonarQube?** ¿Cuáles son las más críticas?

2. **¿Cuál es la calificación de Fiabilidad (Reliability Rating)?** ¿Qué bugs fueron detectados?

3. **¿Hay código duplicado?** ¿En qué archivos?

4. **¿Cuál es el porcentaje de cobertura de pruebas?** Si es 0%, ¿qué implicaciones tiene esto para la calidad del software?

5. **Relaciona** al menos 2 vulnerabilidades encontradas por SonarQube con los conceptos de la práctica de cálculo manual de cobertura. ¿Qué pruebas habrían sido necesarias para detectar esos bugs antes de llegar a producción?

---

## 8. Exportar el Reporte (Opcional)

SonarQube Community Edition no genera reportes PDF directamente, pero puedes:

### Exportar Issues en CSV via API

#### Linux / macOS
```bash
# Reemplaza TU_TOKEN_AQUI con tu token
curl -u TU_TOKEN_AQUI: \
  "http://localhost:9000/api/issues/search?projectKeys=vulnerable-api&ps=500" \
  -o reporte-issues.json
```

#### Windows (PowerShell)
```powershell
$token = "TU_TOKEN_AQUI"
$headers = @{ Authorization = "Bearer $token" }
Invoke-WebRequest `
  -Uri "http://localhost:9000/api/issues/search?projectKeys=vulnerable-api&ps=500" `
  -Headers $headers `
  -OutFile "reporte-issues.json"
```

### Ver Métricas del Proyecto via API

#### Linux / macOS
```bash
curl -u TU_TOKEN_AQUI: \
  "http://localhost:9000/api/measures/component?component=vulnerable-api&metricKeys=bugs,vulnerabilities,code_smells,coverage,duplicated_lines_density,reliability_rating,security_rating,sqale_rating" \
  | python3 -m json.tool
```

#### Windows (PowerShell)
```powershell
$token = "TU_TOKEN_AQUI"
$headers = @{ Authorization = "Bearer $token" }
$metricas = "bugs,vulnerabilities,code_smells,coverage,duplicated_lines_density,reliability_rating,security_rating,sqale_rating"
Invoke-WebRequest `
  -Uri "http://localhost:9000/api/measures/component?component=vulnerable-api&metricKeys=$metricas" `
  -Headers $headers | Select-Object -ExpandProperty Content | ConvertFrom-Json | ConvertTo-Json -Depth 10
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

SonarQube guardará un historial de cada análisis. Puedes ver la evolución en **Activity** dentro del proyecto.

---

## ✅ Checklist Final del Laboratorio

```
[ ] SonarQube corriendo en http://localhost:9000
[ ] Proyecto "Vulnerable-API" analizado exitosamente
[ ] Revisé las Vulnerabilities del proyecto
[ ] Revisé los Bugs del proyecto
[ ] Revisé los Code Smells y la Deuda Técnica
[ ] Identifiqué la cobertura de pruebas (Coverage)
[ ] Respondí las preguntas de reflexión de la Sección 7
[ ] Relacioné los resultados con la norma ISO/IEC 25010
```

---

## 🧹 Limpiar el Entorno al Terminar

Cuando ya no necesites SonarQube corriendo:

```bash
# Ve a la carpeta sonarqube-lab
cd ~/sonarqube-lab          # Linux/macOS
cd $HOME\sonarqube-lab      # Windows

# Detener los contenedores (conserva todos los datos)
docker compose stop

# Si quieres liberar espacio y eliminar todo:
docker compose down -v
```

---

## 📚 Referencias

- [Documentación oficial de SonarQube](https://docs.sonarsource.com/sonarqube/latest/)
- [SonarQube Metric Definitions](https://docs.sonarsource.com/sonarqube/latest/user-guide/metric-definitions/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — Relacionado con vulnerabilidades detectadas
- [ISO/IEC 25010](https://iso25000.com/index.php/normas-iso-25000/iso-25010) — Modelo de calidad del producto software
- [sonar-scanner CLI](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/)
