# Examen-into-backend

Este proyecto contiene la implementación de una base de datos diseñada para gestionar las operaciones de un concesionario de automóviles. La base de datos permite administrar información sobre concesionarios, vendedores, clientes, vehículos, ventas, mantenimiento, suministros, facturas y pagos, proporcionando una solución integral para el manejo de estas actividades.

## Descripción de la Base de Datos

La base de datos **Concesionario** está estructurada para soportar las siguientes funcionalidades principales:

- **Gestión de concesionarios**: Registro de información básica de los concesionarios (nombre y ubicación).
- **Administración de vendedores**: Detalles de los empleados que trabajan en los concesionarios.
- **Gestión de clientes**: Información de contacto y dirección de los clientes.
- **Ventas**: Registro de transacciones de venta de vehículos y suministros.
- **Mantenimiento**: Seguimiento de órdenes de mantenimiento y servicios realizados en vehículos.
- **Inventario**: Control de vehículos disponibles en el concesionario y suministros en stock.
- **Facturación y pagos**: Emisión de facturas y registro de los métodos de pago utilizados.

La base de datos consta de **15 tablas** interrelacionadas mediante claves foráneas para garantizar la integridad de los datos. A continuación, se describen brevemente las tablas y sus propósitos:

- **DEALERSHIP**: Información de los concesionarios.
- **SELLER**: Datos de los vendedores (nombre, número de empleado, fecha de contratación).
- **CLIENTS**: Información de los clientes (nombre, teléfono, email, dirección).
- **ORDER_MAINTENANCE**: Órdenes de mantenimiento solicitadas por los clientes.
- **SALE**: Registro de ventas realizadas.
- **VEHICLES_DEALER**: Vehículos disponibles para la venta en el concesionario.
- **SUPPLIES**: Suministros o partes en inventario.
- **DETAIL_SALE**: Detalles de los ítems vendidos en cada venta.
- **VEHICLES_CLIENTS**: Vehículos propiedad de los clientes.
- **MAINTENANCE**: Servicios de mantenimiento realizados.
- **MAINTENANCE_DETAIL**: Detalles específicos de cada servicio de mantenimiento.
- **TYPE_INVOICE**: Tipos de facturas disponibles.
- **INVOICE**: Facturas emitidas a los clientes.
- **PAYMENT_METHODS**: Métodos de pago aceptados.
- **PAYMENT_INVOICE**: Pagos realizados para las facturas.

### Relaciones entre Tablas

- Un **concesionario** puede tener múltiples vendedores, vehículos, suministros, órdenes de mantenimiento, ventas y servicios.
- Un **cliente** puede realizar múltiples ventas y órdenes de mantenimiento.
- Una **venta** está vinculada a un cliente, un vendedor y un concesionario.
- Los **detalles de venta** especifican qué vehículos o suministros se vendieron.
- Un **servicio de mantenimiento** está asociado a una orden de mantenimiento y puede involucrar vehículos del concesionario, vehículos de clientes y suministros.
- Las **facturas** están relacionadas con clientes y tienen pagos asociados.

## Modelos de la Base de Datos

Para facilitar la comprensión de la estructura, se incluyen los siguientes diagramas:

### Modelo Conceptual
Representa las entidades principales y sus relaciones a alto nivel.

![Modelo Conceptual](https://github.com/user-attachments/assets/152f62a1-01cc-445a-8e52-4516bee9a62f)  
[Link al diagrama](https://www.mermaidchart.com/app/projects/2e5af0da-7276-4e3c-ad1a-e4cd5f0d8392/diagrams/19b56f36-5c29-45c0-8c65-7d3560408308/version/v0.1/edit)

### Modelo Lógico
Detalla las entidades, atributos y relaciones sin considerar la implementación física.

![Modelo Lógico](https://github.com/user-attachments/assets/6db5b663-441b-4f69-958f-bd72bf51dfb7)  
[Link al diagrama](https://www.mermaidchart.com/app/projects/2e5af0da-7276-4e3c-ad1a-e4cd5f0d8392/diagrams/a7ad84de-5e95-4c25-a84d-e9004557adbd/version/v0.1/edit)

### Modelo Físico
Muestra la estructura exacta de la base de datos, incluyendo tablas, columnas y tipos de datos.

![Modelo Físico](https://github.com/user-attachments/assets/cdec07fc-6306-426d-bf96-93461c589d5b)  
[Link al diagrama](https://www.mermaidchart.com/app/projects/2e5af0da-7276-4e3c-ad1a-e4cd5f0d8392/diagrams/94e744eb-fce7-4fdd-8f6e-b37184388d42/version/v0.1/edit)

## Código MySQL

A continuación, se proporciona el script SQL para crear la base de datos y todas sus tablas:

```sql
-- Crear la base de datos
CREATE DATABASE Concesionario;

-- Usar la base de datos
USE Concesionario;

-- Tabla DEALERSHIP
CREATE TABLE DEALERSHIP (
    dealership_id INT PRIMARY KEY,
    name VARCHAR(10),
    location VARCHAR(255)
);

-- Tabla SELLER
CREATE TABLE SELLER (
    seller_id INT PRIMARY KEY,
    name VARCHAR(8),
    employee_number VARCHAR(20) UNIQUE,
    hire_date DATE,
    dealership_id INT,
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIP(dealership_id)
);

-- Tabla CLIENTS
CREATE TABLE CLIENTS (
    client_id INT PRIMARY KEY,
    name VARCHAR(8),
    phone INT,
    email VARCHAR(15),
    address VARCHAR(20)
);

-- Tabla ORDER_MAINTENANCE
CREATE TABLE ORDER_MAINTENANCE (
    order_id INT PRIMARY KEY,
    request_date DATE,
    status VARCHAR(5),
    dealership_id INT,
    client_id INT,
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIP(dealership_id),
    FOREIGN KEY (client_id) REFERENCES CLIENTS(client_id)
);

-- Tabla SALE
CREATE TABLE SALE (
    sale_id INT PRIMARY KEY,
    sale_date DATE,
    total DECIMAL,
    payment_method VARCHAR(8),
    client_id INT,
    seller_id INT,
    dealership_id INT,
    FOREIGN KEY (client_id) REFERENCES CLIENTS(client_id),
    FOREIGN KEY (seller_id) REFERENCES SELLER(seller_id),
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIP(dealership_id)
);

-- Tabla VEHICLES_DEALER
CREATE TABLE VEHICLES_DEALER (
    vehicle_dealer_id INT PRIMARY KEY,
    brand VARCHAR(5),
    model VARCHAR(8),
    year INT,
    vin VARCHAR(10) UNIQUE,
    price DECIMAL,
    color VARCHAR(5),
    fuel_type VARCHAR(5),
    transmission_type VARCHAR(10),
    status VARCHAR(8),
    availability VARCHAR(10),
    dealership_id INT,
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIP(dealership_id)
);

-- Tabla SUPPLIES
CREATE TABLE SUPPLIES (
    supply_id INT PRIMARY KEY,
    name VARCHAR(10),
    description TEXT,
    price DECIMAL,
    stock INT,
    dealership_id INT,
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIP(dealership_id)
);

-- Tabla DETAIL_SALE
CREATE TABLE DETAIL_SALE (
    detail_sale_id INT PRIMARY KEY,
    quantity INT,
    subtotal DECIMAL,
    sale_id INT,
    vehicle_dealer_id INT,
    supply_id INT,
    FOREIGN KEY (sale_id) REFERENCES SALE(sale_id),
    FOREIGN KEY (vehicle_dealer_id) REFERENCES VEHICLES_DEALER(vehicle_dealer_id),
    FOREIGN KEY (supply_id) REFERENCES SUPPLIES(supply_id)
);

-- Tabla VEHICLES_CLIENTS
CREATE TABLE VEHICLES_CLIENTS (
    vehicle_client_id INT PRIMARY KEY,
    brand VARCHAR(5),
    model VARCHAR(8),
    year INT,
    vin VARCHAR(10) UNIQUE,
    client_id INT,
    dealership_id INT,
    FOREIGN KEY (client_id) REFERENCES CLIENTS(client_id),
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIP(dealership_id)
);

-- Tabla MAINTENANCE
CREATE TABLE MAINTENANCE (
    maintenance_id INT PRIMARY KEY,
    service_type VARCHAR(15),
    cost DECIMAL,
    service_date DATE,
    dealership_id INT,
    order_id INT,
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIP(dealership_id),
    FOREIGN KEY (order_id) REFERENCES ORDER_MAINTENANCE(order_id)
);

-- Tabla MAINTENANCE_DETAIL
CREATE TABLE MAINTENANCE_DETAIL (
    maintenance_detail_id INT PRIMARY KEY,
    description TEXT,
    partial_cost DECIMAL,
    maintenance_id INT,
    vehicle_dealer_id INT,
    vehicle_client_id INT,
    supply_id INT,
    FOREIGN KEY (maintenance_id) REFERENCES MAINTENANCE(maintenance_id),
    FOREIGN KEY (vehicle_dealer_id) REFERENCES VEHICLES_DEALER(vehicle_dealer_id),
    FOREIGN KEY (vehicle_client_id) REFERENCES VEHICLES_CLIENTS(vehicle_client_id),
    FOREIGN KEY (supply_id) REFERENCES SUPPLIES(supply_id)
);

-- Tabla TYPE_INVOICE
CREATE TABLE TYPE_INVOICE (
    type_invoice_id INT PRIMARY KEY,
    description VARCHAR(100)
);

-- Tabla INVOICE
CREATE TABLE INVOICE (
    invoice_id INT PRIMARY KEY,
    invoice_number VARCHAR(10) UNIQUE,
    invoice_date DATE,
    total DECIMAL,
    client_id INT,
    type_invoice_id INT,
    FOREIGN KEY (client_id) REFERENCES CLIENTS(client_id),
    FOREIGN KEY (type_invoice_id) REFERENCES TYPE_INVOICE(type_invoice_id)
);

-- Tabla PAYMENT_METHODS
CREATE TABLE PAYMENT_METHODS (
    payment_method_id INT PRIMARY KEY,
    type VARCHAR(8)
);

-- Tabla PAYMENT_INVOICE
CREATE TABLE PAYMENT_INVOICE (
    payment_id INT PRIMARY KEY,
    amount DECIMAL,
    payment_date DATE,
    invoice_id INT,
    payment_method_id INT,
    FOREIGN KEY (invoice_id) REFERENCES INVOICE(invoice_id),
    FOREIGN KEY (payment_method_id) REFERENCES PAYMENT_METHODS(payment_method_id)
);
