# PostgreSQL con Docker

## Creación del Contenedor

```bash
docker run -d --name postgres_container -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -e POSTGRES_DB=campus -p 5433:5432 -v pgdata:/var/lib/postgresql/data --restart=unless-stopped postgres:15
```

## Conectar al contenedor

```bash
docker exec -it postgres_container bash
```

## Conectar con PostgreSQL bajo consola. Versión larga-Versión corta

```bash
psql --host=localhost --username=admin -d campus --password

psql -h localhost -U admin -W admin -d campus
```

## Enumeradores

```sql
CREATE TYPE sexo AS ENUM('Masculino', 'Femenino', 'Otro');

CREATE TABLE camper(
    name VARCHAR(100) NOT NULL,
    sexo_camper sexo NOT NULL
);
```

## Comandos PSQL

- `\l`: Listar bases de datos
- `\c <db_name>`: Cambiar a una base de datos existente
- `\d`: Describe las tablas de la base de datos actual
- `\ds`: Secuencias, que se crean con el tipo de datos `serial`
- `\di`: Listar los índices
- `\dp \z`: Listado de privilegios de las tablas
- `\dt`: Listado de las tablas de la base de datos actual

## Ejemplo general de tipos de datos

```sql
CREATE TABLE ejemplo(
    id serial PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion text NULL,
    precio numeric(10, 2) NOT NULL,
    en_stock boolean NOT NULL,
    fecha_creacion date NOT NULL,
    hora_creacion time NOT NULL,
    fecha_hora timestamp NOT NULL,
    fecha_hora_zona timestamp with time zone,
    duracion interval,
    direccion_ip inet,
    direccion_mac macaddr,
    punto_geometrico point,
    datos_json json,
    datos_jsonb jsonb,
    identificador_unico uuid,
    cantidad_monetario money,
    rangos int4range,
    colores_preferidos varchar(20)[]
);
```

### Insert para `ejemplo`

```sql
INSERT INTO ejemplo(nombre, descripcion, precio, en_stock, fecha_creacion, hora_creacion, fecha_hora, fecha_hora_zona, duracion, direccion_ip, direccion_mac, punto_geometrico, datos_json, datos_jsonb, identificador_unico, cantidad_monetario, rangos, colores_preferidos)
VALUES(
'Ejemplo A', 'Lorem Ipsum...', 9990.99, true, '2025-07-10', '20:30:10', '2025-07-10 20:30:10', '2025-07-10 20:30:10-05', '1 day', '192.168.0.1', '08:00:27:00:00:00', '(10, 20)', '{"key":"value"}', '{"key":"value"}', '7d500cde-d0c4-429b-9b4a-0d50f159a948', '100.00', '[10, 20)', ARRAY['rojo', 'rosado']

);
```

## Definición Constraints (Restricciones)
### Tabla de ejemplo
```sql
CREATE TABLE empleados(
    id serial,
    nombre varchar(100) NOT NULL,
    edad integer NOT NULL,
    salario numeric(10, 2) NOT NULL,
    fecha_contrato date,
    vigente boolean DEFAULT true
);

CREATE TABLE departamentos(
    id serial,
    nombre varchar(100) NOT NULL,
    vigente boolean DEFAULT true,
    PRIMARY KEY(id)
);

ALTER TABLE empleados ADD COLUMN departamento_id integer NOT NULL;
```

## Constaints a tablas existentes
### Primary Key
```sql
ALTER TABLE empleados ADD PRIMARY KEY(id);
```

### Foreign Key
```sql
ALTER TABLE empleados ADD CONSTRAINT fk_departamento FOREIGN KEY(departamento_id)
REFERENCES departamentos(id);
```

### Unique
```sql
ALTER TABLE empleados ADD CONSTRAINT nombre_unique UNIQUE(nombre);
```

### Check
```sql
ALTER TABLE empleados ADD CONSTRAINT check_edad CHECK(edad>=18);
```

### Default
```sql
ALTER TABLE empleados ALTER COLUMN salario SET DEFAULT 400.00;
```

### Not Null
```sql
ALTER TABLE empleados ALTER COLUMN salario SET NOT NULL;
```

# Taller de Constraints
>Definir los Constraints (Primary Key, Foreign Key, Not Null, Default) mediante ALTER TABLE
```sql
CREATE TABLE country (
    id serial,
    name varchar(50)
);

CREATE TABLE region (
    id serial,
    name varchar(50),
    idcountry integer
);

CREATE TABLE city (
    id serial,
    name varchar(50),
    idregion integer
);
```

### Primary Key
```sql
ALTER TABLE country ADD PRIMARY KEY(id);

ALTER TABLE region ADD PRIMARY KEY(id);

ALTER TABLE city ADD PRIMARY KEY(id);
```

### Foreign Key
```sql
ALTER TABLE region ADD CONSTRAINT fk_country FOREIGN KEY(idcountry)
REFERENCES country(id);

ALTER TABLE city ADD CONSTRAINT fk_region FOREIGN KEY(idregion)
REFERENCES region(id);
```

### Unique
```sql
ALTER TABLE country ADD CONSTRAINT unique_name UNIQUE(name);

ALTER TABLE region ADD CONSTRAINT unique_name_region UNIQUE(name);

ALTER TABLE city ADD CONSTRAINT unique_name_city UNIQUE(name);
```

### Default
```sql
ALTER TABLE country ALTER COLUMN name SET DEFAULT 'Colombia';
```

### Not Null
```sql
ALTER TABLE country ALTER COLUMN name SET NOT NULL;

ALTER TABLE region ALTER COLUMN name SET NOT NULL;
ALTER TABLE region ALTER COLUMN idcountry SET NOT NULL;

ALTER TABLE city ALTER COLUMN name SET NOT NULL;
ALTER TABLE city ALTER COLUMN idregion SET NOT NULL;
```