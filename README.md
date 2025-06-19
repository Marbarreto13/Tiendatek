# Tienda-Tek: 

Bienvenido a **Tienda-Tek**, un proyecto educativo y funcional de comercio electrónico diseñado para demostrar la arquitectura, desarrollo y despliegue de una aplicación web moderna de tres capas.

Este repositorio es mantenido por **[Bayron Cañas y Fernanda García]**, como un proyecto práctico que abarca desde la concepción inicial hasta el despliegue en un entorno de producción en la nube. Presenta desafíos reales de conectividad, persistencia de datos y seguridad, diseñados para ilustrar las mejores prácticas en el desarrollo full-stack.

---

### 🛍️ Funcionalidades Implementadas

Cada aspecto del sistema ha sido construido para crear una experiencia de e-commerce completa:

*   **Autenticación de Usuarios**
    *   Registro de cuentas para clientes y empleados (con código de acceso).
    *   Inicio de sesión seguro con contraseñas hasheadas (`bcryptjs`).
    *   Gestión de sesiones mediante **JSON Web Tokens (JWT)**.

*   **Catálogo de Productos para Clientes**
    *   Visualización de productos con imágenes, precios y stock.
    *   Búsqueda por nombre de producto.
    *   Filtrado por categorías.

*   **Flujo de Compra Completo**
    *   Carrito de compras persistente en `localStorage`.
    *   Proceso de checkout que valida el stock.
    *   Creación de pedidos mediante **transacciones SQL** para garantizar la integridad de los datos.
    *   Visualización del historial de pedidos para cada cliente.

*   **Panel de Administración para Empleados**
    *   Sistema de navegación por pestañas para gestionar el negocio.
    *   **CRUD completo de productos** (Crear, Leer, Actualizar, Borrar).
    *   Subida de imágenes de productos con almacenamiento persistente en la nube (**Cloudinary**).
    *   Visualización de un listado completo de todas las ventas realizadas.
    *   Log de auditoría para rastrear las acciones de los empleados.

---

### 🛠️ Tecnologías y Arquitectura

El proyecto está construido sobre una arquitectura desacoplada, utilizando un stack de tecnologías modernas y eficientes.

*   **Arquitectura General: Modelo-Controlador (API RESTful)**
    *   El backend sigue una adaptación del patrón MVC, separando la lógica de las rutas (Controladores) de las interacciones con la base de datos.
    *   El frontend consume la API, actuando como la capa de Vista.

*   **Stack Tecnológico Principal**
    *   **Frontend**: JavaScript puro (Vanilla JS, ES6+), HTML5, CSS3.
    *   **Backend**: Node.js con el framework Express.js.
    *   **Base de Datos**: PostgreSQL.
    *   **Almacenamiento de Archivos**: Cloudinary para imágenes.

*   **Plataformas de Despliegue (CI/CD)**
    *   `Frontend` desplegado en **Netlify** para una entrega global rápida.
    *   `Backend` y `Base de Datos PostgreSQL` desplegados en **Render** para una gestión robusta del servidor y los datos.

---

### ⚙️ Configuración y Puesta en Marcha Local

Para ejecutar este proyecto en tu propia máquina, sigue estos pasos.

1.  **Clonar el Repositorio**
    ```bash
    git clone https://github.com/ChorizoConPollo/Tienda-Tek.git
    cd Tienda-Tek
    ```

2.  **Configurar el Backend (`/backend`)**
    *   Instalar dependencias: `npm install`
    *   Crear un archivo `.env` y configurar las variables (ver la sección de Base de Datos a continuación).
    *   Ejecutar en modo desarrollo: `npm run dev`

3.  **Configurar la Base de Datos (PostgreSQL)**
    *   Crea una base de datos local.
    *   Ejecuta el script SQL que se encuentra en la sección `💾 Arquitectura de la Base de Datos` del `README.md` de este repositorio para crear todas las tablas y relaciones.

4.  **Ejecutar el Frontend (`/frontend`)**
    *   Abre los archivos `index.html` y `empleado.html` directamente en tu navegador.
    *   **Importante:** Asegúrate de que las variables `API_URL` y `BASE_URL` en los scripts apunten a tu servidor backend local (ej: `http://localhost:3001`).

---

### 💾 Arquitectura de la Base de Datos

El diseño de la base de datos es relacional y consta de 5 tablas principales.

*   **Tablas Clave:**
    *   `usuarios`: Gestiona la información y roles de los usuarios.
    *   `productos`: Catálogo de la tienda.
    *   `ventas`: Cabecera de cada pedido.
    *   `detalleventas`: Artículos específicos dentro de cada pedido.
    *   `historialactividades`: Bitácora de acciones de los empleados.

*   **Script de Creación (DDL):** El script completo para crear la estructura de la base de datos se encuentra en la sección `README.md` de este repositorio.

---

### ✒️ Autor

*   **Bayron Cañas y Fernanda García**

---

### 🎓 Conclusión del Proyecto

"Tienda-Tek" ha sido un ejercicio práctico integral que demuestra la construcción y despliegue de una aplicación web full-stack desde cero. El proyecto no solo cumple con los requisitos funcionales de un e-commerce, sino que también implementa soluciones a desafíos del mundo real como la **autenticación segura, la integridad de datos transaccionales, el almacenamiento persistente de archivos y el despliegue automatizado (CI/CD)**.

La arquitectura desacoplada seleccionada ha demostrado ser robusta, escalable y mantenible, sentando una base sólida para futuras expansiones.