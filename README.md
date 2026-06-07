# Sistema-de-Analisis-de-Inventario-y-Ventas---Retail
Proyecto de análisis de datos que incluye limpieza y preparación de datos en Python, carga automatica a PostgreSQL y un dashboard en Power BI.El proyecto simula un flujo de trabajo real: los datos crudos se procesan automáticamente con Python y quedan disponibles en un dashboard de Power BI actualizable.

## Herramientas utilizadas
 
- **Python** — limpieza de datos y pipeline automatizado
- **Pandas** — transformación y análisis de datos
- **SQLAlchemy** — conexión y carga automática a PostgreSQL
- **PostgreSQL** — almacenamiento de datos limpios
- **Power BI Desktop** — dashboard conectado a PostgreSQL

## Dataset
 
Dataset sintético de inventario retail con 74.500 registros originales, con 5 tiendas, 20 productos, 5 categorías, 4 regiones y otras variables como ventas, inventario, descuentos, clima y estación.

## Automatización

### **Limpieza**

 | Problema | Solución |
|----------|----------|
| Fechas en 3 formatos distintos (YYYY-MM-DD, DD/MM/YYYY, Mon DD YYYY) | `pd.to_datetime` con `format='mixed'` |
| Categorías con mayúsculas inconsistentes (Electronics, ELECTRONICS, electronics) | `str.strip().str.title()` |
| Espacios extra en columna Region | `str.strip()` |
| Valores "N/A" y símbolos "$" en columna Price | `str.replace` + `pd.to_numeric(errors='coerce')` |
| Descuentos como string ("20%") mezclados con enteros | `str.replace` + conversión a numérico |
| 1.439 filas duplicadas | `drop_duplicates()` |
| Nulos en Price, Units Sold e Inventory Level | Imputación con mediana por producto |
| Valores negativos en Units Sold e Inventory Level | Conversión a valor absoluto con `.abs()` |

### **Carga a SQL**

Los datos limpios se cargan directamente a PostgreSQL sin intervención manual usando SQLAlchemy. Cada vez que llega un nuevo archivo de datos, correr este notebook actualiza la base de datos automáticamente.

### **Actualización del Dashboard**

El dashboard esta conectado a la base de datos PostgreSQl por lo que con solo hacer un clic en "actualizar" el dashboard se refresca con todos los cambios que se hayan producido en la base de datos

## Dashboard Power BI
 
Una página con 4 visualizaciones orientadas a decisiones operativas y comerciales:
 
- **KPIs principales** — total ventas, unidades vendidas, inventario promedio y cantidad de productos
- **Ventas por categoría** — comparación de ingresos entre las 5 categorías
- **Ventas por mes** — evolución temporal con granularidad mensual para detectar variaciones estacionales
- **Riesgo de quiebre de stock** — scatter plot que relaciona nivel de inventario con unidades vendidas por producto, identificando productos en zona de alerta preventiva

## Principales hallazgos
 
- Las ventas se mantienen estables a lo largo del período con variaciones mensuales de entre $21M y $23,7M, sin tendencia clara de crecimiento ni caída.
- Las 5 categorías tienen volúmenes de venta similares, con Furniture liderando levemente ($110,8M) y Electronics en último lugar ($107,8M).
- El inventario promedio de 274 unidades es adecuado para el nivel de ventas actual, aunque algun producto muestra niveles de stock por debajo del promedio con ventas cercanas al máximo — zona de alerta que requiere seguimiento.

