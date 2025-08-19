# Taller Diseño de base de datos - Postgres
Teniendo en cuenta el diagrama entidad relación resuelva los siguientes requerimientos:

### DDL
- Genere los comandos DDL que permitan la creación de la base de datos del DER suministrado.
```sql
CREATE DATABASE miscompras;

CREATE TABLE clientes(
    id VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(40) NOT NULL,
    apellidos VARCHAR(100) NOT NULL,
    celular NUMERIC(10, 0) NOT NULL,
    direccion VARCHAR(80) NOT NULL,
    correo_electronico VARCHAR(70) NOT NULL
);

CREATE TABLE categorias(
    id_categoria serial PRIMARY KEY,
    descripcion VARCHAR(45) NOT NULL,
    estado SMALLINT NOT NULL
);

CREATE TABLE productos(
    id_producto serial PRIMARY KEY,
    nombre VARCHAR(45) NOT NULL,
    id_categoria serial NOT NULL,
    codigo_barras VARCHAR(150) NOT NULL,
    precio_venta numeric(16, 2) NOT NULL,
    cantidad_stock INT NOT NULL, 
    estado SMALLINT NOT NULL,
    CONSTRAINT fk_categoria FOREIGN KEY (id_categoria)
    REFERENCES categorias(id_categoria)
);

CREATE TABLE compras(
    id_compra serial PRIMARY KEY,
    id_cliente VARCHAR(20) NOT NULL,
    fecha TIMESTAMP NOT NULL,
    medio_pago CHAR(1) NOT NULL,
    comentario varchar(300) NOT NULL, 
    estado CHAR(1) NOT NULL,
    CONSTRAINT fk_cliente FOREIGN KEY (id_cliente)
    REFERENCES clientes(id)
);

CREATE TABLE compras_productos(
    id serial PRIMARY KEY,
    id_compra serial NOT NULL,
    id_producto serial NOT NULL,
    cantidad INT NOT NULL,
    total NUMERIC(16, 2) NOT NULL,
    estado SMALLINT NOT NULL,
    CONSTRAINT fk_compra FOREIGN KEY (id_compra)
    REFERENCES compras(id_compra),
    CONSTRAINT fk_producto FOREIGN KEY (id_producto)
    REFERENCES productos(id_producto)
);

```
---
### DML
- Genere los comandos DML que permitan insertar datos en la base de datos creada en el paso 1.

>Inserts `clientes`
```sql
INSERT INTO clientes (id, nombre, apellidos, celular, direccion, correo_electronico) VALUES
('C001', 'Ana', 'Caballero López', 3124567890, 'Calle 10 #5-20', 'ana.caballero@example.com'),
('C002', 'Luis', 'García Pérez', 3109876543, 'Carrera 15 #8-45', 'luis.garcia@example.com'),
('C003', 'María', 'Rodríguez Gómez', 3112233445, 'Av. 7 #12-30', 'maria.rodriguez@example.com'),
('C004', 'Carlos', 'Medina Torres', 3205566778, 'Calle 25 #18-90', 'carlos.Medina@example.com'),
('C005', 'Sofía', 'Hernández Díaz', 3009988776, 'Carrera 8 #3-14', 'sofia.hernandez@example.com');
```

>Inserts `categorias`
```sql
INSERT INTO categorias (descripcion, estado) VALUES
('Bebidas', 1),
('Snacks', 1),
('Aseo', 1),
('Lácteos', 1),
('Frutas', 1);
```

>Inserts `productos`
```sql
INSERT INTO productos (nombre, id_categoria, codigo_barras, precio_venta, cantidad_stock, estado) VALUES
('Coca Cola 1.5L', 1, '7701234567890', 4500.00, 50, 1),
('Papas Pobres 150g', 2, '7700987654321', 2500.00, 100, 1),
('Jabón Ponds 200g', 3, '7701122334455', 1500.00, 80, 1),
('Leche 1L', 4, '7702233445566', 3200.00, 60, 1),
('Manzana Verde Kg', 5, '7703344556677', 6000.00, 40, 1);
```

>Inserts `compras`
```sql
INSERT INTO compras (id_cliente, fecha, medio_pago, comentario, estado) VALUES
('C001', '2025-08-01 10:30:00', 'E', 'Compra en efectivo', 'A'),
('C002', '2025-08-02 15:45:00', 'T', 'Pago con tarjeta débito', 'A'),
('C003', '2025-08-03 09:20:00', 'C', 'Compra a crédito', 'A'),
('C004', '2025-08-04 19:00:00', 'E', 'Compra en efectivo', 'A'),
('C005', '2025-08-05 12:15:00', 'T', 'Pago con tarjeta crédito', 'A');
```

>Inserts `compras_productos`
```sql
INSERT INTO compras_productos (id_compra, id_producto, cantidad, total, estado) VALUES
(1, 1, 2, 9000.00, 1),  
(2, 2, 3, 7500.00, 1),
(3, 3, 5, 7500.00, 1),   
(4, 4, 2, 6400.00, 1),  
(5, 5, 1, 6000.00, 1);   

```