# 📱 RappiPay – Framework de Automatización Mobile

## 📌 Propuesta de Arquitectura  
**Framework de Automatización Mobile (Android / iOS)**  
**Arquitectura Híbrida: Page Object Model (POM) + Screenplay**

---

## 1. Introducción

Este repositorio contiene la propuesta técnica para un **framework de automatización de pruebas móviles** diseñado para aplicaciones **Android e iOS**, utilizando **Appium** y buenas prácticas de automatización.

Se ha implementado una **arquitectura híbrida**, combinando:

- **Page Object Model (POM)** para la gestión y centralización de elementos de la interfaz.
- **Screenplay Pattern** para modelar la lógica de negocio desde la perspectiva del usuario (actores, tareas e interacciones).

Este enfoque garantiza un framework **limpio, escalable, reutilizable y fácil de mantener**, ideal para entornos CI/CD y equipos de QA en crecimiento.

---

## 2. Diagrama de la Estructura del Proyecto

La siguiente estructura de carpetas asegura una correcta **separación de responsabilidades**:

```
rappipay-automation/
├── src/
│   ├── main/java/com/rappipay/
│   │   ├── drivers/        # Factory Pattern para gestión de Android / iOS
│   │   ├── ui/             # POM: Localizadores de elementos (@FindBy)
│   │   ├── tasks/          # Screenplay: Lógica de negocio reutilizable
│   │   ├── interactions/   # Screenplay: Acciones base (Click, Wait, Scroll)
│   │   └── utils/          # Configuración de logs y propiedades
│   └── test/java/com/rappipay/
│       ├── tests/          # Casos de prueba (JUnit / TestNG)
│       └── runners/        # Suites de ejecución
├── resources/
│   ├── log4j2.xml          # Configuración de logging
│   └── config.properties   # Parámetros de plataforma y capacidades
└── pom.xml                 # Gestión de dependencias con Maven
```
---
## 3. Patrones de Diseño Aplicados

### 🎭 Screenplay Pattern

Los tests se modelan como **historias de usuarios**, donde:

- Un **Actor** interactúa con la aplicación.
- El actor ejecuta **Tasks** (ej. `Login`, `Transferencia`).
- Las tareas están compuestas por **Interactions** reutilizables.
- Las validaciones se realizan mediante **Questions**.

#### ✅ Beneficios

- Alta reutilización de código  
- Tests más legibles y expresivos  
- Fácil escalabilidad  

---

### 🧱 Page Object Model (POM)

El patrón **POM** se utiliza exclusivamente para la **capa de UI**, centralizando los localizadores de elementos:

- Un archivo por pantalla  
- Cambios en la UI se corrigen en un solo lugar  
- Reduce el impacto de cambios visuales  

**Ejemplo:**
```
LoginScreen.java
HomeScreen.java
```
### 🏭 Factory Pattern

Implementado en la capa de **drivers**, permite:

- Inicializar dinámicamente el `AppiumDriver`
- Soportar **Android** e **iOS** sin duplicar código
- Selección de plataforma por configuración

---

## 4. Gestión Multiplataforma

La plataforma se define en el archivo `config.properties`:

```properties
platform=ANDROID
deviceName=Pixel_5
```
El framework carga automáticamente las **DesiredCapabilities** según el valor configurado.

---

### Localizadores multiplataforma

En la capa UI se utilizan las anotaciones nativas de Appium:

- `@AndroidFindBy`
- `@iOSXCUITFindBy`

Esto permite compartir la misma lógica de negocio entre plataformas.

## 5. Estrategia de Logs y Reportes

### 📝 Logging

Se integra **Log4j2 / SLF4J** para:

- Registrar cada interacción del driver  
- Tiempos de espera y carga de elementos  
- Errores y excepciones  

Esto facilita el diagnóstico de fallos en ejecución local o CI.

---

### 📊 Reportes

Se propone el uso de:

- **Allure Reports**
- **ExtentReports**

**Características:**

- Detalle paso a paso  
- Capturas automáticas en fallos  
- Reportes visuales y exportables  

## 6. Escalabilidad y Mantenibilidad
### 🚀 Escalabilidad

- Las Tasks son independientes y reutilizables

- Nuevas funcionalidades se agregan sin afectar tests existentes

- Ideal para suites grandes (Smoke, Regression, E2E)

### 🛠️ Mantenibilidad

Separación clara entre:

- Qué se prueba → Tests

- Cómo se ejecuta → Tasks / Interactions

- Dónde está el elemento → UI / POM

- Esto reduce significativamente el costo de mantenimiento ante cambios en la aplicación.

## 7. Organización de Dependencias y Ejecución

### 📦 Maven

Todas las dependencias se gestionan desde `pom.xml`:

- Appium  
- TestNG / JUnit  
- Log4j2  
- Reportes  

---

### ▶️ Ejecución

Las pruebas se ejecutan mediante **TestNG XML Suites**, permitiendo:

- Agrupación de pruebas (Smoke, Regression)  
- Ejecución paralela  
- Integración sencilla con pipelines CI/CD  

**Ejemplo:**
```
mvn clean test -DsuiteXmlFile=smoke.xml
```

