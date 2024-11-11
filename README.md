# Biblioteca de Dependencias gradle / transitivas 🤘🏼

| **Sección**                      | **Descripción**                                                                                                                                                                                                                                                        |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Objetivo del Proyecto**        | Proporcionar una biblioteca que incluya la dependencia `com.pacifico.kuntur:core:0.17.0` como dependencia transitiva para pruebas en otros proyectos.                                                                                                                   |
| **Estructura del Proyecto**      | - `build.gradle`: Define las dependencias, incluidas las transitivas.<br>- `settings.gradle`: Configura el nombre del proyecto.<br>- `.github/workflows/publish.yml`: Configura la publicación automática en GitHub Packages.                                           |
| **Dependencias**                 | Incluye `com.pacifico.kuntur:core:0.17.0` como dependencia transitiva para otros proyectos que consuman `biblioteca`.                                                                                                                                                    |
| **Publicación en GitHub Packages** | El flujo de trabajo de GitHub Actions (`publish.yml`) publica automáticamente la biblioteca en GitHub Packages cuando se realiza un push a la rama `main`.                                                                                                           |
| **Configuración de Publicación** | Configurada en `build.gradle` con el uso de `GITHUB_TOKEN` para autenticación en GitHub Packages.                                                                                                                                                                        |
| **Uso en Otros Proyectos**       | Para usar `biblioteca` en otros proyectos, agregar el repositorio de GitHub Packages y la dependencia `com.example:test-gradle-biblioteca:1.0.0-SNAPSHOT` en `build.gradle`.                                                                                           |
| **Verificación de Dependencias Transitivas** | Ejecutar `./gradlew dependencies --configuration compileClasspath` en el proyecto que consume `biblioteca` para ver el árbol de dependencias e identificar la dependencia transitiva `com.pacifico.kuntur:core:0.17.0`.                                       |
| **Monitoreo de Vulnerabilidades con Dependabot** | Configura Dependabot en el proyecto que incluye `biblioteca` para recibir alertas sobre vulnerabilidades en dependencias transitivas, como `com.pacifico.kuntur:core:0.17.0`.                                        |
| **Contacto**                     | Abre un **Issue** en el repositorio o contacta a **WillClever** para asistencia o preguntas.                                                                                                                                                                             |

Este repositorio contiene una biblioteca de dependencias Java utilizada para probar la resolución de dependencias transitivas en otros proyectos. Aquí publicamos la dependencia `biblioteca`, la cual puede incluir dependencias adicionales que serán transitivas cuando `biblioteca` se use en otros proyectos.

## Objetivo del Proyecto

El objetivo de este repositorio es proporcionar una biblioteca que contenga una dependencia específica (`com.pacifico.kuntur:core:0.17.0`) como dependencia transitiva. Esto permite probar cómo los sistemas de gestión de dependencias, como Dependabot, manejan y alertan sobre dependencias transitivas en otros repositorios.

## Estructura del Proyecto

La biblioteca está configurada en Gradle y se publica en GitHub Packages para que otros repositorios puedan consumirla como una dependencia.

### Principales Archivos y Carpetas

- `build.gradle`: Define las dependencias del proyecto, incluida la dependencia transitiva.
- `settings.gradle`: Configura el nombre del proyecto.
- `.github/workflows/publish.yml`: Configuración de GitHub Actions para publicar automáticamente la biblioteca en GitHub Packages cuando se hace un push a la rama `main`.

## Dependencias

Este proyecto incluye las siguientes dependencias:

- `com.pacifico.kuntur:core:0.17.0`: Dependencia principal que se publicará de forma transitiva para otros proyectos que incluyan `biblioteca` como dependencia.

## Publicación en GitHub Packages

Cada vez que se realiza un push a la rama `main`, GitHub Actions ejecuta el flujo de trabajo definido en `.github/workflows/publish.yml`. Este flujo de trabajo realiza los siguientes pasos:

1. Compila el proyecto con Gradle.
2. Publica la biblioteca en GitHub Packages.
3. Hace que la dependencia `com.pacifico.kuntur:core:0.17.0` esté disponible como dependencia transitiva para otros proyectos que consuman `biblioteca`.

### Configuración de Publicación

El archivo `build.gradle` incluye la configuración necesaria para publicar en GitHub Packages, utilizando el `GITHUB_TOKEN` para autenticación.

```gradle
publishing {
    publications {
        mavenJava(MavenPublication) {
            from components.java
        }
    }
    repositories {
        maven {
            name = "GitHubPackages"
            url = uri("https://maven.pkg.github.com/WillClever/biblioteca")
            credentials {
                username = System.getenv("USERNAME")
                password = System.getenv("GITHUB_TOKEN")
            }
        }
    }
}
```
## Uso de la Biblioteca en Otros Proyectos

Para usar esta biblioteca en otro proyecto y acceder a sus dependencias transitivas, sigue estos pasos:
	1.	Asegúrate de tener acceso a GitHub Packages en tu entorno de desarrollo. Esto se logra configurando las credenciales USERNAME y GITHUB_TOKEN.
	2.	Agrega biblioteca como dependencia en el archivo build.gradle de tu proyecto.

Ejemplo de Configuración en un Proyecto Externo

```gradle
repositories {
    mavenCentral()
    maven {
        url = uri("https://maven.pkg.github.com/WillClever/biblioteca")
        credentials {
            username = project.findProperty("gpr.user") ?: System.getenv("USERNAME")
            password = project.findProperty("gpr.token") ?: System.getenv("GITHUB_TOKEN")
        }
    }
}

dependencies {
    implementation 'com.example:test-gradle-biblioteca:1.0.0-SNAPSHOT'
}
```

## Verificación de Dependencias Transitivas

Puedes verificar que com.pacifico.kuntur:core:0.17.0 esté incluida como dependencia transitiva en un proyecto externo ejecutando el siguiente comando en el proyecto que incluye biblioteca:

```bash
./gradlew dependencies --configuration compileClasspath
```

## Monitoreo Monitoreo de Vulnerabilidades con Dependabot

Para asegurar que cualquier vulnerabilidad en las dependencias transitivas sea detectada, configura Dependabot en el proyecto que incluye biblioteca. Dependabot escaneará las dependencias transitivas y generará alertas si encuentra alguna vulnerabilidad en com.pacifico.kuntur:core:0.17.0 o cualquier otra dependencia transitiva.

## Contribuciones

Este repositorio está destinado para pruebas de dependencias y monitoreo de seguridad, pero las contribuciones son bienvenidas para mejorar su funcionalidad y documentación.


