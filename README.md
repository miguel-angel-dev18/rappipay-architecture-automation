# 📱 RappiPay – Framework de Automatización Mobile

## 1. Stack Tecnológico
* **Lenguaje:** Java 11+.
* **Core:** Appium + Selenium.
* **Patrón de Diseño:** Screenplay + Factory.
* **Gestión de Dependencias:** Maven.
* **Testing Framework:** TestNG / JUnit.
* **Reportes:** Serenity BDD / Allure.
* **Logs:** Log4j2 / SLF4J.

---

## 2. Estructura del Proyecto (📂)
La arquitectura se organiza bajo el estándar de Maven, garantizando la separación de responsabilidades:

```
src/main/java/com/rappipay/
├── drivers/        # Factory Pattern: Gestión dinámica de Android e iOS.
├── ui/             # Localizadores: Uso de @AndroidFindBy y @iOSXCUITFindBy.
├── tasks/          # Lógica de negocio: Acciones como Login o Transferir.
├── questions/      # Aserciones: Verificaciones de estado y resultados.
├── interactions/   # Gestos: Implementación de Swipe, Scroll y Tap.
└── utils/          # Soporte: Configuración de propiedades y constantes.
```
## 3. Patrones de Diseño Aplicados

### 🎭 Screenplay Pattern
Se utiliza para modelar las pruebas como historias de usuario, donde un **Actor** ejecuta **Tasks** y valida mediante **Questions**. Esto asegura:
* **Alta reutilización:** Las tareas se crean una vez y se usan en múltiples escenarios.
* **Legibilidad:** El código se lee como lenguaje natural, facilitando la auditoría técnica.

### 🧱 Arquitectura Híbrida (UI Layer)
Se centralizan los localizadores en la capa **ui** utilizando anotaciones multiplataforma (`@AndroidFindBy` / `@iOSXCUITFindBy`).
* Permite que una misma lógica de negocio (**Task**) funcione tanto para **Android** como para **iOS** sin duplicar código.

### 🏭 Factory Pattern
Implementado en la capa de drivers para inicializar el `AppiumDriver` dinámicamente.
* El framework decide en tiempo de ejecución si levanta una sesión de Android o iOS basándose estrictamente en la configuración externa.

---

## 4. Gestión Multiplataforma y Configuración
La plataforma de ejecución se define en el archivo `config.properties`, permitiendo cambiar el entorno de pruebas sin modificar el código fuente:

```
platform=ANDROID
deviceName=Pixel_7
automationName=UiAutomator2
```
## 5. Escalabilidad, Logs y Reportes

* **Escalabilidad:** Al desacoplar la interfaz (UI) de la lógica de negocio (Tasks), se pueden agregar nuevas funcionalidades o flujos complejos sin riesgo de romper los tests existentes.
* **Logs:** Integración de **SLF4J / Log4j2** para obtener una trazabilidad completa de cada comando enviado al driver de Appium, facilitando la depuración en entornos CI/CD.
* **Reportes:** Generación automática de reportes detallados (**Serenity/Allure**) que incluyen capturas de pantalla automáticas ante cualquier fallo detectado.

---

## 6. Ejecución de Pruebas

Las pruebas se gestionan mediante archivos de suite **XML (TestNG)**, lo que permite ejecuciones paralelas y una integración nativa con pipelines de automatización.

Para ejecutar la suite de pruebas desde la terminal, utilice el siguiente comando de **Maven**:

```
mvn clean test -DsuiteXmlFile=smoke.xml
```


