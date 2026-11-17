# Catálogo de indicadores

El tablero está pensado para el dueño de una distribuidora mayorista de consumo masivo. Es un negocio de **margen bajo y mucho volumen**: compra mercadería, la tiene en el depósito, la vende con plazo y recién después cobra. Por eso los indicadores no miran solo cuánto se gana, sino también **cuánta plata queda atrapada en el camino** (stock y lo que deben los clientes) y si la caja alcanza para pagar.

Cada indicador responde una pregunta concreta del dueño. Salen de las vistas del esquema `kpi` y del modelo `marts` del warehouse.

## Período y comparaciones

- **Período:** el tablero muestra agosto de 2026.
- **Números del mes** (ventas, márgenes, ganancia, entregas): se comparan con julio de 2026 y con agosto de 2025.
- **Saldos** (caja, lo que deben los clientes, stock, proveedores): son al 31/08/2026 y se comparan con el 31/07/2026 y con el 31/08/2025.
- **Cómo se expresa el cambio:** los indicadores en pesos cambian en %, los que ya son porcentajes en puntos y los de días en días.
- **Colores:** verde es bueno para el negocio y rojo es malo. Por ejemplo, que bajen los días de cobro es verde.

## Moneda

- **Pesos de hoy** (vista por defecto): cada monto de un mes se multiplica por el IPC nacional del INDEC (nivel general) de agosto de 2026 dividido el IPC de ese mes. Es un análisis de gestión y no reemplaza el ajuste por inflación contable (RT 6).
- **Dólares MEP:** cada monto de un mes se divide por el dólar MEP (venta) del último día hábil de ese mes. Una variación en dólares mezcla lo que pasó con el negocio y lo que pasó con el tipo de cambio.

Los porcentajes, los días y las unidades dan igual en las dos monedas. El tablero muestra la tabla de inflación y de dólar MEP usada mes por mes.

## Claves: los 7 indicadores del mes

| Indicador | Pregunta del dueño | Cómo se calcula | Por qué importa en una distribuidora |
| --- | --- | --- | --- |
| Ventas | ¿Cuánto vendí? | Facturado en el mes, sin IVA | Con margen bajo, el volumen es lo que paga la estructura. |
| Margen bruto | ¿Cuánto me queda de cada venta? | (Ventas − costo de la mercadería vendida) ÷ ventas | Un punto de margen es una parte grande de la ganancia final. |
| Ganancia final | ¿Estoy ganando plata? | Ventas − costo − gastos − amortizaciones − intereses − impuestos | Es el resultado de todo lo anterior. |
| Entregas en tiempo y forma | ¿Cumplo con lo que prometo? | Pedidos despachados a tiempo y completos ÷ pedidos despachados | En distribución, la entrega es el servicio. Fallar se paga con clientes que se van. |
| Facturas vencidas sin cobrar | ¿Me pagan a tiempo? | Saldo a cobrar ya vencido ÷ saldo total a cobrar | Vender con plazo es financiar al cliente; lo vencido es financiamiento que no se pactó. |
| Caja y bancos hoy | ¿Cuánta plata tengo? | Saldo de las cuentas de caja y bancos del mayor al 31/08/2026 | Es lo que sostiene los pagos si los clientes se atrasan. |
| A pagar a proveedores en 30 días | ¿Cuánto tengo que pagar en el próximo mes? | Facturas de proveedores que vencen hasta el 30/09/2026, más las ya vencidas sin pagar | Comparado con la caja, muestra si alcanza la plata para cumplir. |

## Comercial

| Indicador | Cómo se calcula | Para qué sirve |
| --- | --- | --- |
| Ventas por mes | Ventas de cada mes, en plata o en unidades (cajas), con el mismo mes del año anterior | Ver la estacionalidad y separar si las ventas suben por precio o por volumen. |
| Ventas, unidades y margen por categoría | Ventas, cajas vendidas y margen bruto de cada categoría de artículo, con el margen del mes anterior y del mismo mes del año anterior, y una fila de total | Ver qué categoría arrastra el margen total hacia abajo. |
| Clientes que dejaron de comprar | Clientes que no compran hace más de 90 días, con lo que compraban por año | Saber a quién llamar antes de que se acostumbre a otro proveedor. |
| Días de stock, mes a mes | Stock promedio del mes ÷ costo de lo vendido por día | Ver si la mercadería parada viene creciendo o bajando. |
| Mercadería que sobra | Por producto: el stock que excede 90 días de venta (todo el stock si no se vendió en 90 días), valuado al costo | Saber cuánta plata está parada en el depósito y en qué productos. El tablero trae un ejemplo paso a paso. |
| Lo que falta o se está por terminar | Productos sin stock que se vendieron en el mes, y los más vendidos con stock para menos de 15 días | Evitar perder ventas por falta de mercadería. |

## Económico

| Indicador | Cómo se calcula | Para qué sirve |
| --- | --- | --- |
| Márgenes bruto, operativo y neto, mes a mes | Utilidad bruta, operativa y neta ÷ ventas, de los últimos 12 meses | Ver dónde se va la ganancia: mercadería, estructura, intereses o impuestos. |
| Punto de equilibrio | Costos fijos ÷ (1 − costos variables ÷ ventas) | Cuánto hay que vender para no perder plata y cuánto pueden caer las ventas antes de perder. |
| Estado de resultados mes a mes | Ventas − costo de ventas = utilidad bruta; − gastos operativos = utilidad operativa; − gastos financieros = utilidad antes de impuestos; − impuestos = utilidad neta | Ver la evolución de cada renglón, en millones o en % de las ventas, y abrir el detalle por cuenta. |

## Financiero

| Indicador | Cómo se calcula | Para qué sirve |
| --- | --- | --- |
| Ciclo de la plata | Días de stock + días de cobro − días de pago | Cuántos días pasan desde que le pagás al proveedor hasta que te paga el cliente. Menos días es menos caja necesaria. |
| Días de cobro | Saldo a cobrar promedio ÷ ventas con IVA × días | Cuánto tardan los clientes en pagar. |
| Días de pago | Saldo a pagar promedio ÷ compras con IVA × días | Cuánto tardás en pagar a los proveedores. |
| Detalle de clientes | Lo vencido de cada cliente, con sus días de atraso y su plazo | Decidir a quién reclamar primero (por monto, por antigüedad o por las dos cosas). |
| Detalle de proveedores | Lo que vence con cada proveedor en los próximos 30 días, factura por factura | Decidir a quién pagar primero. |

## Criterio para los textos

Cada gráfico tiene debajo un texto **"Qué pasó"** que se arma con los datos (no está escrito a mano) y explica el resultado en palabras simples. Cada pestaña empieza con un **resumen** de una línea por gráfico.
