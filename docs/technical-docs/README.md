

 Arquitectura y Configuración de la Base de Datos

La base de datos es el pilar de la persistencia de datos de "Tienda-Tek". Se ha elegido **PostgreSQL** por su robustez, escalabilidad y su amplio soporte para tipos de datos complejos y transacciones.

     1. Modelo de Datos Entidad-Relación

El diseño de la base de datos es relacional y está normalizado para evitar la redundancia y garantizar la integridad de los datos. Consta de 5 tablas principales que modelan el negocio:

**`usuarios`**: Almacena la información de los usuarios registrados, incluyendo sus credenciales de acceso y su rol dentro del sistema (cliente o empleado). Es el centro del sistema de autenticación. **`productos`**: Contiene el catálogo completo de los productos de la tienda. Incluye detalles como nombre, descripción, precio, stock y categoría.
**`ventas`**: Actúa como la "cabecera" de cada pedido. Registra la información general de la transacción, como el ID del usuario que compró, la fecha y el monto total.
**`detalleventas`**: Es una tabla de unión que desglosa el contenido de cada venta. Cada fila representa un producto específico dentro de un pedido, con su cantidad y el precio al momento de la compra. Está directamente relacionada con `ventas` y `productos`.
**`historialactividades`**: Funciona como una bitácora o log de auditoría. Registra acciones importantes realizadas por los empleados (crear, editar, eliminar productos) para fines de seguimiento y control.

**Diagrama de Relaciones:**

```
+------------+      +--------------------+      +---------------+
|  usuarios  |----<|       ventas       |>----<| detalleventas |>----| productos |
+------------+      +--------------------+      +---------------+      +-----------+
      |                                                                       
      |
      +--------------------<| historialactividades |
                           +------------------------+
```
*(Leyenda: `---<` indica una relación "uno a muchos")*

### 2. Script de Creación de Tablas (DDL)

Para configurar la base de datos desde cero, ya sea en un entorno local o de producción, se debe ejecutar el siguiente script SQL. Este script define la estructura completa, los tipos de datos, las claves primarias, las claves foráneas y las restricciones de cada tabla.

```sql
-- ========= TABLA DE USUARIOS =========
-- Almacena la información de clientes y empleados.
CREATE TABLE usuarios (
    idusuario SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(100) UNIQUE NOT NULL,
    contraseña VARCHAR(255) NOT NULL,
    rol VARCHAR(20) NOT NULL DEFAULT 'cliente'
);

-- ========= TABLA DE PRODUCTOS =========
-- Contiene el catálogo de todos los productos de la tienda.
CREATE TABLE productos (
    idproducto SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    preciounitario NUMERIC(10, 2) NOT NULL,
    stock INTEGER NOT NULL,
    categoria VARCHAR(50),
    imagen_url VARCHAR(255)
);

-- ========= TABLA DE VENTAS =========
-- Representa la cabecera de un pedido o una venta completa.
CREATE TABLE ventas (
    idventa SERIAL PRIMARY KEY,
    fecha TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    total NUMERIC(10, 2) NOT NULL,
    idusuario INTEGER NOT NULL,
    CONSTRAINT fk_usuario
        FOREIGN KEY(idusuario) 
        REFERENCES usuarios(idusuario)
);

-- ========= TABLA DE DETALLE DE VENTAS =========
-- Almacena cada línea de artículo dentro de una venta.
CREATE TABLE detalleventas (
    iddetalle SERIAL PRIMARY KEY,
    idventa INTEGER NOT NULL,
    idproducto INTEGER NOT NULL,
    cantidad INTEGER NOT NULL,
    preciounitario NUMERIC(10, 2) NOT NULL,
    CONSTRAINT fk_venta
        FOREIGN KEY(idventa) 
        REFERENCES ventas(idventa),
    CONSTRAINT fk_producto
        FOREIGN KEY(idproducto) 
        REFERENCES productos(idproducto)
);

-- ========= TABLA DE HISTORIAL DE ACTIVIDADES =========
-- Registra las acciones importantes realizadas por los empleados.
CREATE TABLE historialactividades (
    idhistorial SERIAL PRIMARY KEY,
    idusuario INTEGER NOT NULL,
    accion VARCHAR(50) NOT NULL,
    detalles TEXT,
    fecha TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_usuario_actividad
        FOREIGN KEY(idusuario) 
        REFERENCES usuarios(idusuario)
);
```

### 3. Configuración de la Conexión

La conexión desde el backend de Node.js se gestiona a través del archivo `backend/config/database.js`, utilizando la librería `pg`. Este módulo implementa un **pool de conexiones** para un manejo eficiente de las peticiones concurrentes y es capaz de detectar automáticamente el entorno:

*   **En Producción (Render):** Utiliza la variable de entorno `DATABASE_URL` para conectarse a la base de datos desplegada, incluyendo la configuración SSL requerida.
*   **En Desarrollo Local:** Utiliza las variables individuales (`PGUSER`, `PGHOST`, `PGDATABASE`, etc.) definidas en un archivo `.env` para conectarse a una instancia local de PostgreSQL.

Este enfoque dual permite trabajar y probar la aplicación localmente con la misma base de código que se despliega en producción, sin necesidad de realizar cambios.