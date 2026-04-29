# 🔬 Laboratorio: Análisis de Calidad de Código con SonarQube

**Asignatura:** Modelos de Evaluación de Software
**Repositorio a analizar:** [`michealkeines/Vulnerable-API`](https://github.com/michealkeines/Vulnerable-API)

---

## 📌 Descripción General

Este laboratorio es la continuación práctica del ejercicio de cálculo manual de cobertura de pruebas (`lab_manual_test_coverage_calculation.md`). Mientras que en ese ejercicio calculaste métricas de cobertura a mano sobre una función pequeña, en esta práctica aplicarás un enfoque **automatizado e industrial** sobre un proyecto real usando **SonarQube**, una de las herramientas de análisis estático más utilizadas en la industria del software.

El alumno configurará un entorno completo de análisis de calidad, ejecutará un escaneo real sobre código vulnerable y aprenderá a interpretar los resultados en el contexto del modelo de calidad **ISO/IEC 25010 (SQuaRE)**.

---

## 🎯 Objetivos de la Práctica

Al finalizar este laboratorio, el alumno será capaz de:

1. **Levantar** una instancia local de SonarQube usando Docker, comprendiendo su arquitectura de servicios.
2. **Configurar** un proyecto de análisis y generar tokens de autenticación seguros.
3. **Ejecutar** el escáner de SonarQube (`sonar-scanner`) sobre un repositorio real desde la línea de comandos.
4. **Identificar** bugs, vulnerabilidades de seguridad, code smells y deuda técnica en el código analizado.
5. **Relacionar** las métricas automáticas de SonarQube con las características de calidad definidas por la norma **ISO/IEC 25010**: fiabilidad, seguridad, mantenibilidad y cobertura de pruebas.
6. **Contrastar** el esfuerzo del cálculo manual de cobertura con la automatización que ofrecen las herramientas SAST (Static Application Security Testing).

---

## 🗂️ Estructura de la Guía

Sigue los archivos **en orden numérico**. Cada paso depende del anterior.

```
📁 laboratorio-sonarqube/
│
├── README.md                        ← Estás aquí
│
├── 00_INDICE_Y_PREREQUISITOS.md     ← Herramientas necesarias e instalación
├── 01_SETUP_DOCKER_SONARQUBE.md     ← Levantar SonarQube con Docker
├── 02_GENERAR_TOKEN_SONARQUBE.md    ← Crear proyecto y generar token
├── 03_CLONAR_Y_ESCANEAR.md          ← Clonar el repo y ejecutar el análisis
└── 04_INTERPRETAR_RESULTADOS.md     ← Leer métricas y responder preguntas
```

| # | Archivo | Qué harás |
|---|---|---|
| 0 | `00_INDICE_Y_PREREQUISITOS.md` | Instalar Docker, Git, Java y sonar-scanner |
| 1 | `01_SETUP_DOCKER_SONARQUBE.md` | Levantar SonarQube + PostgreSQL con Docker Compose |
| 2 | `02_GENERAR_TOKEN_SONARQUBE.md` | Crear el proyecto y obtener el token de autenticación |
| 3 | `03_CLONAR_Y_ESCANEAR.md` | Clonar `Vulnerable-API` y lanzar el escaneo |
| 4 | `04_INTERPRETAR_RESULTADOS.md` | Analizar resultados y responder preguntas de reflexión |

---

## 🧰 Herramientas Requeridas

| Herramienta | Versión mínima | Propósito |
|---|---|---|
| [Docker Desktop](https://www.docker.com/products/docker-desktop) | 24.x | Correr SonarQube y PostgreSQL en contenedores |
| [Git](https://git-scm.com/) | 2.x | Clonar el repositorio a analizar |
| [Java JDK](https://adoptium.net/) | 17 LTS | Requerido por sonar-scanner |
| [sonar-scanner CLI](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/) | 6.x | Analizar el código y enviar resultados a SonarQube |

---

## ⚡ Inicio Rápido

```bash
# 1. Clona o descarga esta guía en tu máquina

# 2. Verifica que tienes todas las herramientas
docker --version && git --version && java -version && sonar-scanner --version

# 3. Levanta SonarQube (desde la carpeta con el docker-compose.yml)
docker compose up -d

# 4. Abre SonarQube en el navegador
#    → http://localhost:9000  (admin / admin)

# 5. Crea el proyecto y genera el token (ver Paso 2)

# 6. Clona el repositorio a analizar
git clone https://github.com/michealkeines/Vulnerable-API.git
cd Vulnerable-API

# 7. Ejecuta el escaneo
sonar-scanner -Dsonar.token=TU_TOKEN_AQUI

# 8. Ve los resultados en:
#    → http://localhost:9000/dashboard?id=vulnerable-api
```

---

## 🔗 Relación con el Laboratorio Manual

Este laboratorio es complementario al ejercicio de cálculo manual de cobertura:

| Concepto | Laboratorio Manual | Este Laboratorio |
|---|---|---|
| **Line Coverage** | Calculado a mano contando líneas ejecutadas | Reportado automáticamente por SonarQube |
| **Branch Coverage** | Tabla de ramas TRUE/FALSE completada a mano | Calculado para todo el proyecto en segundos |
| **Path Coverage** | 3 caminos identificados manualmente | Análisis de flujo completo de todos los archivos |
| **Bugs** | Identificados por revisión de código | Detectados automáticamente con reglas SAST |
| **Escala** | 1 función, ~10 líneas | Proyecto completo, decenas de archivos |

> 💡 **Reflexión clave:** El cálculo manual te enseña el *fundamento* de las métricas. SonarQube te muestra por qué esas métricas deben *automatizarse* en proyectos reales.

---

## 📋 Entregables del Laboratorio

Al concluir la práctica, el alumno debe ser capaz de reportar:

- [ ] Captura de pantalla del dashboard de SonarQube con el análisis completado
- [ ] Número total de **Bugs**, **Vulnerabilities** y **Code Smells** encontrados
- [ ] Calificaciones de **Reliability**, **Security** y **Maintainability**
- [ ] Porcentaje de **cobertura de pruebas** (Coverage) y su interpretación
- [ ] Respuestas a las preguntas de reflexión del archivo `04_INTERPRETAR_RESULTADOS.md`

---

## ⚠️ Nota sobre el Repositorio

`Vulnerable-API` es un proyecto **intencionalmente inseguro**, diseñado con fines educativos para practicar detección de vulnerabilidades. SonarQube encontrará múltiples problemas de seguridad. Esto es esperado y es el objetivo del análisis.

> 🚫 **Este proyecto NO debe desplegarse en un servidor público ni usarse en producción.**

---

## 📚 Referencias

- [Documentación SonarQube](https://docs.sonarsource.com/sonarqube/latest/)
- [ISO/IEC 25010 — Modelo de Calidad del Producto](https://iso25000.com/index.php/normas-iso-25000/iso-25010)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [SonarQube Metric Definitions](https://docs.sonarsource.com/sonarqube/latest/user-guide/metric-definitions/)
