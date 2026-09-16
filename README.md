# Análisis y Dashboard de Ventas — Cafetería (2019-2022)

> Proyecto de análisis y visualización de datos con Excel de principio a fin: desde datos crudos en tres tablas relacionadas hasta un dashboard interactivo, pasando por limpieza, combinación de tablas, formateo y tablas dinámicas.

## Descripción del proyecto

El archivo `coffeeOrdersData.xlsx` contiene la información de ventas de una cafetería, organizada en tres tablas:

- **`customers`** — Clientes: ID, nombre, email, teléfono, dirección, ciudad, país, código postal y si tienen tarjeta de fidelización (Loyalty Card).
- **`products`** — Catálogo de productos: ID de producto, tipo de café, tipo de tueste, tamaño, precio unitario, precio por 100g y beneficio.
- **`orders`** — Pedidos: cada compra registrada con un ID único, fecha, cliente y producto asociados, cantidad y ventas totales.

La tabla `orders` llegó "en bruto", con solo los identificadores de cliente y producto, sin formato consistente en fechas/importes/pesos y con posible ruido (duplicados). El objetivo del proyecto es transformarla en una tabla analítica completa y construir un dashboard que permita monitorizar ingresos, beneficios y clientes principales de un vistazo.

### Estructura original de las tablas

**`customers`**

| Columna | Descripción |
|---|---|
| Customer ID | Identificador único del cliente |
| Customer Name | Nombre del cliente |
| Email | Correo electrónico |
| Phone Number | Teléfono |
| Address Line 1 | Dirección |
| City | Ciudad |
| Country | País |
| Postcode | Código postal |
| Loyalty Card | Si tiene tarjeta de fidelización (Sí/No) |

**`products`**

| Columna | Descripción |
|---|---|
| Product ID | Identificador único del producto |
| Coffee Type | Tipo de café (código abreviado) |
| Roast Type | Tipo de tueste (código abreviado) |
| Size | Tamaño/peso del producto |
| Unit Price | Precio unitario |
| Price per 100g | Precio por cada 100g |
| Profit | Beneficio del producto |

**`orders` (estructura original, antes de enriquecerla)**

| Columna | Descripción |
|---|---|
| Order ID | Identificador único del pedido |
| Order Date | Fecha del pedido |
| Customer ID | Referencia al cliente (clave para cruzar con `customers`) |
| Product ID | Referencia al producto (clave para cruzar con `products`) |
| Quantity | Cantidad de unidades pedidas |

Como se observa, `orders` solo contenía sus propias columnas y las claves de referencia (`Customer ID`, `Product ID`); no incluía ningún dato descriptivo de clientes o productos (nombre, país, tipo de café, precio, etc.). Esas columnas se añadieron en el paso 1 de la metodología, combinando las tres tablas.

## Metodología

### 1. Enriquecimiento de la tabla de órdenes
Se combinó `orders` con `customers` y `products` mediante **`BUSCARX`** (XLOOKUP), añadiendo a cada pedido: nombre y país del cliente, email, tarjeta de fidelización, tipo de café, tipo de tueste y tamaño del producto. También se creó la columna de valor **Ventas totales** (`Sales`).

### 2. Limpieza y traducción de códigos
Los códigos abreviados de producto (p. ej. `Rob`, `M`) se tradujeron a etiquetas legibles mediante **`SI.CONJUNTO`** (IFS): tipo de café completo (Robusto, Excelso, Arábico, Libérico) y tipo de tueste completo (Medium, Light, Dark).

### 3. Formateo de datos
Se homogeneizaron los formatos de las columnas para que fueran interpretables a simple vista:
- Fechas → formato "día / mes en texto / año".
- Peso del producto → kilogramos (kg).
- Precio unitario y ventas → euros (€).

### 4. Control de calidad
Verificación y eliminación de registros duplicados con la herramienta **Eliminar duplicados** de Excel.

### 5. Análisis con tablas dinámicas
Se construyeron tres tablas dinámicas clave:
- Ventas totales a lo largo del periodo, segmentadas por tipo de café.
- Ventas totales por país.
- Ranking de clientes con mayor número/importe de compras.

### 6. Construcción del dashboard
A partir de gráficos dinámicos vinculados a las tres tablas dinámicas, y con segmentaciones de datos (slicers) para filtrar por tipo de producto, tipo de tueste y tarjeta de fidelización, además de una línea de tiempo para filtrar por fecha de pedido.

## Funciones principales utilizadas

| Función | Uso |
|---|---|
| `BUSCARX` (XLOOKUP) | Unir `orders` con `customers` y `products` (nombre, país, email, tarjeta de fidelización, tipo de café, tueste, tamaño) |
| `SI` (IF) | Limpiar valores `0` devueltos por `BUSCARX` cuando el email no existe |
| `SI.CONJUNTO` (IFS) | Traducir códigos abreviados a nombres completos de tipo de café y tueste |
| Formato de celda personalizado | Mostrar unidades (kg, €) y fechas en formato largo sin alterar el valor numérico subyacente |
| Tablas y gráficos dinámicos + Segmentación de datos | Analizar y visualizar ventas por periodo, tipo de café, país y cliente |

## 📊 Resultado final

<img width="1391" height="721" alt="DASHBOARD" src="https://github.com/user-attachments/assets/1699956d-7bc0-4899-bf23-e79fe9d06e8c" />

El dashboard permite ver de un vistazo la evolución de ventas por tipo de café, la distribución de ventas por país y el top de clientes, con filtros interactivos por fecha, tipo de producto, tueste y tarjeta de fidelización.
