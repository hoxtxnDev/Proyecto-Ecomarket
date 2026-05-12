# 🛒 ECOMARKET - Documentación Oficial y Flujo de Trabajo

Bienvenido al repositorio central de **Ecomarket**. Este proyecto utiliza una arquitectura orientada a servicios (API REST) con **Spring Boot (Java)** en el backend y una base de datos relacional (**SQL**). 

El objetivo principal de nuestra arquitectura es la **estabilidad**: si ocurre un error, el sistema debe capturarlo y devolver un mensaje limpio, sin que la aplicación completa colapse.

---

## 👥 Roles del Equipo y Zonas de Trabajo

Cada miembro del equipo tiene responsabilidades y zonas de código estrictamente delimitadas:

### 🟣 Tech Lead & Gestión (Horacio)
*   **Rol:** Administrador del repositorio y orquestador del proyecto.
*   **Responsabilidades:** 
    *   Crear y asignar tareas (Issues) en GitHub.
    *   Revisar el código de los demás y aprobar los *Pull Requests* (PR) hacia la rama `main`.
    *   Resolver conflictos de integración y bloqueos técnicos.

### 🟠 Base de Datos & QA / Testing (Williams)
*   **Rol:** Arquitecto de datos y control de calidad.
*   **Zona de Trabajo:** Paquetes `model` (Entidades JPA) y `repository` (Consultas). Archivo `application.properties`.
*   **Responsabilidades (BD):** Diseñar las tablas SQL, configurar las credenciales de conexión y crear las entidades Java correspondientes.
*   **Responsabilidades (QA):** Usar **Postman** para disparar peticiones a las URLs que construya el Backend y verificar que los datos se guarden correctamente en SQL antes de que el Frontend las use.

### 🟢 Backend Core (Hans)
*   **Rol:** Motor lógico e integrador.
*   **Zona de Trabajo:** Paquetes `service` (Lógica) y `controller` (Endpoints REST).
*   **Responsabilidades:** 
    *   Recibir los datos de la base de datos y transformarlos en APIs REST (Ej: `/api/usuarios`).
    *   **Regla Anti-Caídas:** Implementar `@RestControllerAdvice` para capturar excepciones (como "Usuario no encontrado" o "Datos nulos") y devolver respuestas JSON estructuradas con códigos HTTP correctos (404, 400, 500) en lugar de que el servidor crashee.
    *   Configurar `@CrossOrigin` para permitir que el Frontend se conecte sin bloqueos de seguridad.

### 🔵 Frontend UI (Santibañez)
*   **Rol:** Experiencia de Usuario e Interfaz.
*   **Zona de Trabajo:** ¡Fuera del entorno Java! Trabajarás en una carpeta o repositorio separado utilizando HTML, CSS, Bootstrap y JavaScript (Fetch API).
*   **Responsabilidades:** Construir las pantallas (Ej: Formulario de Registro, Catálogo) y utilizar la función `fetch()` de JavaScript para enviar y recibir datos de las APIs construidas por Hans.

---

## 🔄 El Flujo de Trabajo: Ciclo de una Funcionalidad

Para que no nos pisemos el código, toda nueva funcionalidad (ej. "Módulo de Usuarios") debe seguir este orden exacto:

1.  **Estructura (Williams):** Crea la tabla `usuarios` en SQL, arma la clase `Usuario.java` en la carpeta `model` y la interfaz en `repository`. Sube su código.
2.  **Lógica (Hans):** Actualiza su repositorio para descargar lo de Williams. Crea `UsuarioService.java` para las validaciones y `UsuarioController.java` para exponer la URL `/api/usuarios`. Sube su código.
3.  **Pruebas (Williams):** Toma la URL construida por Hans, abre Postman y envía datos de prueba. Si todo está en orden y llega a SQL, le da luz verde a Frontend.
4.  **Interfaz (Santibañez):** Diseña el formulario visual en HTML, toma el botón "Guardar" y, usando JavaScript, lo conecta a la URL de Hans.

---

## 🛠️ Reglas de Oro y Comandos Git

**🚨 LA RAMA `main` ESTÁ PROTEGIDA.** Nadie puede hacer un `git push` directamente a ella. 

Todo el trabajo diario de cada integrante debe seguir estos pasos en la terminal:

**1. Actualizar siempre tu base antes de empezar a programar:**
```bash
git checkout main
git pull origin main