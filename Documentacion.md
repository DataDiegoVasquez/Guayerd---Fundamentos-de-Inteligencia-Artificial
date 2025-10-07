# DOCUMENTACIÓN DEL SISTEMA DE ANÁLISIS DE VENTAS

|  |  |
| :------: | :-----------: |
| **Objetivo**   | Generar la documentacion pertinente para la ejecucion del Proyecto el cual esta basado en una solucion para la tienda Aurelion  |
| **Autor** | Diego Vasquez |
| **Fecha Creación**    | 2025/10/06 |
| **Periodicidad**    | On Demand |

#### Control de Cambios

| Objetivo         | Fecha Modificación     | Autor |
|:--------------:|:-----------:|:------------:|
| Sprint #1 | 2025/10/06 | Diego Vasquez |


## 1. TEMA, PROBLEMA Y SOLUCIÓN

### Tema
**Este proyecto genera una emulacion de bases de datos reales con la finalidad de proporcionar un ambiente de pruebas de un Sistema de Análisis y Gestión de Ventas para Comercio Minorista**

### Problema
Las empresas de comercio minorista necesitan analizar su información de ventas para tomar decisiones estratégicas, pero tienen sus datos distribuidos en múltiples tablas (clientes, productos, ventas y detalles de ventas). Esto dificulta:

- Conocer el desempeño real de ventas por producto, categoría o período
- Identificar patrones de compra de los clientes
- Analizar la efectividad de diferentes medios de pago
- Determinar qué productos tienen mejor rotación
- Evaluar el comportamiento de ventas por ubicación geográfica
- Generar reportes consolidados para la toma de decisiones

### Solución
Desarrollar un sistema de análisis que integre las diferentes fuentes de datos (clientes, productos, ventas y detalle de ventas) para:

- Consolidar información dispersa en un modelo relacional coherente
- Calcular métricas clave de negocio (totales de venta, promedios, rankings)
- Generar reportes por categoría, ciudad, medio de pago y período
- Identificar clientes frecuentes y productos más vendidos
- Facilitar la visualización y análisis de datos para stakeholders
- Permitir consultas específicas según necesidades del negocio

## 2. DATASET DE REFERENCIA

### Fuente
Base de datos de ventas de comercio minorista creada para fines educativos compuesta por 4 archivos Excel:
- `clientes.xlsx`
- `productos.xlsx`
- `ventas.xlsx`
- `detalle_ventas.xlsx`

### Definición
El dataset representa las transacciones comerciales de un negocio minorista que recibe el nombre de Tienda Aurelion que opera en múltiples ciudades de Argentina, vendiendo productos de las categorías Alimentos y Limpieza. Registra información desde el año 2023 hasta 2024, incluyendo datos de clientes, catálogo de productos, cabeceras de ventas y el detalle de cada ítem vendido.

### Estructura

#### Tabla 1: CLIENTES
**Descripción:** Registro maestro de clientes del negocio

| Campo | Descripción | Tipo | Escala | Ejemplo |
|-------|-------------|------|--------|---------|
| id_cliente | Identificador único del cliente | Integer | Nominal | 1 |
| nombre_cliente | Nombre completo del cliente | String | Nominal | Mariana Lopez |
| email | Correo electrónico de contacto | String | Nominal | mariana.lopez@mail.com |
| ciudad | Ciudad de residencia del cliente | String | Nominal | Carlos Paz |
| fecha_alta | Fecha de registro en el sistema | Date | Intervalo | 2023-01-01 |

**Cantidad de registros:** 100 clientes
**Estructura:** Datos Estructurados 

#### Tabla 2: PRODUCTOS
**Descripción:** Catálogo de productos disponibles para la venta

| Campo | Descripción | Tipo | Escala | Ejemplo |
|-------|-------------|------|--------|---------|
| id_producto | Identificador único del producto | Integer | Nominal | 1 |
| nombre_producto | Nombre descriptivo del producto | String | Nominal | Coca Cola 1.5L |
| categoria | Categoría del producto | String | Nominal | Alimentos |
| precio_unitario | Precio por unidad en moneda local | Numeric | Razón | 2347 |

**Cantidad de registros:** 100 productos
**Estructura:** Datos Estructurados 
**Categorías:** Alimentos, Limpieza

#### Tabla 3: VENTAS
**Descripción:** Cabecera de cada transacción de venta realizada

| Campo | Descripción | Tipo | Escala | Ejemplo |
|-------|-------------|------|--------|---------|
| id_venta | Identificador único de la venta | Integer | Nominal | 1 |
| fecha | Fecha de la transacción | Date | Intervalo | 2024-06-16 |
| id_cliente | Referencia al cliente | Integer | Nominal | 62 |
| nombre_cliente | Nombre del cliente (desnormalizado) | String | Nominal | Guadalupe Romero |
| email | Email del cliente (desnormalizado) | String | Nominal | guadalupe.romero@mail.com |
| medio_pago | Forma de pago utilizada | String | Nominal | tarjeta |

**Cantidad de registros:** 120 ventas
**Estructura:** Datos Estructurados
**Medios de pago:** tarjeta, qr, transferencia, efectivo

#### Tabla 4: DETALLE_VENTAS
**Descripción:** Ítems individuales de cada venta (línea de detalle)

| Campo | Descripción | Tipo | Escala | Ejemplo |
|-------|-------------|------|--------|---------|
| id_venta | Referencia a la venta | Integer | Nominal | 1 |
| id_producto | Referencia al producto | Integer | Nominal | 90 |
| nombre_producto | Nombre del producto (desnormalizado) | String | Nominal | Toallas Húmedas x50 |
| cantidad | Unidades vendidas | Integer | Razón | 1 |
| precio_unitario | Precio unitario al momento de venta | Numeric | Razón | 2902 |
| importe | Total de la línea (cantidad × precio) | Numeric | Razón | 2902 |

**Cantidad de registros:** 343 ítems vendidos
**Estructura:** Datos Estructurados




### Información del Programa

**Objetivo:** Crear un sistema de análisis que permita generar reportes consolidados de ventas con las siguientes funcionalidades:

1. **Reporte de Ventas por Categoría:** Total vendido por cada categoría de producto
2. **Ranking de Productos:** Top productos más vendidos por cantidad y por monto
3. **Análisis por Ciudad:** Ventas totales y cantidad de clientes por ciudad
4. **Análisis de Medios de Pago:** Distribución de ventas según forma de pago
5. **Clientes Frecuentes:** Identificar clientes con más transacciones
6. **Análisis Temporal:** Ventas por período (mensual, trimestral)

### Pasos del Programa

#### PASO 1: Carga de Datos
1. Cargar archivo `clientes.xlsx` en estructura de datos
2. Cargar archivo `productos.xlsx` en estructura de datos
3. Cargar archivo `ventas.xlsx` en estructura de datos
4. Cargar archivo `detalle_ventas.xlsx` en estructura de datos
5. Validar integridad de datos (verificar que no haya valores nulos críticos)

#### PASO 2: Integración y Preparación
1. Crear índices para optimizar búsquedas (diccionarios por ID)
2. Enriquecer detalle_ventas con información de productos (categoría)
3. Enriquecer ventas con información de clientes (ciudad)
4. Calcular totales por venta (suma de importes de detalle)
5. Validar consistencia de datos entre tablas

#### PASO 3: Análisis y Cálculos
1. **Ventas por Categoría:**
   - Agrupar detalle_ventas por categoría de producto
   - Sumar importes por cada categoría
   - Calcular porcentajes sobre total

2. **Ranking de Productos:**
   - Agrupar detalle_ventas por id_producto
   - Sumar cantidades vendidas
   - Sumar importes totales
   - Ordenar descendente por cantidad y por importe
   - Seleccionar top 10

3. **Análisis por Ciudad:**
   - Agrupar ventas por ciudad del cliente
   - Contar cantidad de ventas por ciudad
   - Sumar importes totales por ciudad
   - Contar clientes únicos por ciudad

4. **Análisis de Medios de Pago:**
   - Agrupar ventas por medio_pago
   - Contar cantidad de transacciones
   - Sumar importes por medio de pago
   - Calcular porcentaje de uso

5. **Clientes Frecuentes:**
   - Contar ventas por id_cliente
   - Sumar importe total por cliente
   - Ordenar por cantidad de compras descendente
   - Seleccionar top 10

#### PASO 4: Generación de Reportes
1. Formatear resultados en tablas legibles
2. Generar visualizaciones (gráficos) si es necesario
3. Exportar reportes a archivos o mostrar en pantalla
4. Calcular métricas resumidas (totales, promedios, medianas)

#### PASO 5: Presentación
1. Mostrar dashboard con principales KPIs
2. Permitir interacción para filtros personalizados
3. Generar insights y recomendaciones automáticas
