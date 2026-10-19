# Catálogo de indicadores

El tablero está pensado para el dueño de una distribuidora mayorista de consumo masivo. Es un negocio de **margen bajo y mucho volumen**: compra mercadería, la tiene en el depósito, la vende con plazo y recién después cobra. Por eso la mayoría de los indicadores no miran solo cuánto se gana, sino **cuánta plata queda atrapada en el camino** (stock y cuentas por cobrar) y si la caja alcanza para sostenerlo.

Cada indicador responde una pregunta concreta del dueño. Todos salen de las vistas del esquema `kpi` del warehouse; ninguno se calcula en el tablero.

## Los 8 indicadores clave (pestaña Claves)

Son los que el dueño debería mirar todos los meses. Cada uno tiene una meta, un estado (en orden, alerta o crítico), la causa principal del resultado y una acción sugerida.

| Indicador | Pregunta del dueño | Fórmula | Meta y de dónde sale | Por qué importa en una distribuidora |
| --- | --- | --- | --- | --- |
| Ventas | ¿Vendo lo que esperaba? | Facturado en el período, sin IVA | Presupuesto 2026 (Excel) | Con margen bajo, el volumen es lo que paga la estructura. |
| Margen bruto | ¿Gano lo suficiente por cada venta? | (Ventas − costo de la mercadería) ÷ ventas | 22%, el margen del presupuesto 2026 | Un punto de margen en una distribuidora es una parte grande de la ganancia final. |
| Ganancia final | ¿Estoy ganando plata? | Ventas − costo − gastos − amortizaciones − intereses − impuestos | Presupuesto 2026 (Excel) | Es el resultado de todo lo anterior. |
| Días de caja | ¿Cuántos días aguanto sin cobrar nada? | Caja y bancos ÷ salidas promedio por día (últimos 90 días) | Más de 30 días | Si los clientes se atrasan, la caja es lo único que sostiene los pagos a proveedores y sueldos. |
| Deuda vencida de clientes | ¿Me pagan a tiempo? | Saldo vencido ÷ saldo a cobrar | Menos de 25% | Vender con plazo es financiar al cliente; lo vencido es financiamiento que no se pactó. |
| Días de stock | ¿Tengo plata parada en el depósito? | Stock promedio del mes ÷ costo de lo vendido en el mes × días del mes | Menos de 45 días | El stock es plata inmovilizada y riesgo de vencimiento o de obsolescencia. |
| Clientes en riesgo | ¿Estoy perdiendo clientes? | Clientes sin comprar hace más de 90 días ÷ clientes con historial | Menos de 20% | Recuperar un cliente que se fue cuesta más que retenerlo. |
| Entregas a tiempo y completas (OTIF) | ¿Cumplo con lo que prometo? | Pedidos entregados en fecha y completos ÷ pedidos | 90% | En distribución, la entrega es el servicio; fallar se paga con clientes que se van. |

## Indicadores por pestaña

| Pestaña | Indicador | Fórmula | Para qué sirve |
| --- | --- | --- | --- |
| Comercial | Margen por categoría | Margen bruto de cada familia de productos | Ver qué categoría arrastra el margen total hacia abajo. |
| Comercial | Unidades vendidas | Cajas vendidas | Separar si las ventas suben por precio o por volumen. |
| Económico | Margen operativo, EBITDA, margen neto | Márgenes del estado de resultados | Ver dónde se va la ganancia: estructura, intereses o impuestos. |
| Económico | Punto de equilibrio | Costos fijos ÷ contribución por peso vendido | Cuánto pueden caer las ventas antes de perder plata. |
| Económico | Carga tributaria sobre ventas | Ingresos Brutos, débitos y créditos, seguridad e higiene y Ganancias ÷ ventas | Dimensionar el peso de los impuestos en un negocio de margen bajo. |
| Financiero | Caja generada por el negocio | Cobranzas − pagos a proveedores − gastos e impuestos pagados | Si el negocio genera plata antes de cuotas del préstamo y retiros. |
| Financiero | Ciclo de la plata | Días de stock + días de cobro − días de pago | Cuántos días está la plata afuera entre que se paga la mercadería y se cobra la venta. |
| Financiero | Caja de las próximas semanas | Saldo de hoy + cobros que vencen − pagos que vencen, semana por semana | Anticipar en qué semana puede faltar plata, con fechas reales de sueldos, impuestos y cuotas. |
| Financiero | Antigüedad de la deuda | Lo que te debe cada cliente, por tramo de atraso | Decidir a quién reclamar primero (ordenable por monto, antigüedad o ambos). |
| Patrimonial | Deuda por cada peso propio | Pasivo ÷ patrimonio | Cuánto depende la empresa de la plata de terceros. |
| Patrimonial | Liquidez | Activo corriente ÷ pasivo corriente | Si lo que vence en el corto plazo se puede pagar. |
| Patrimonial | Capital de trabajo operativo | Lo que te deben + stock − proveedores | Cuánta plata necesita el negocio para funcionar. |
| Patrimonial | Retiros sobre ganancia | Retiros de los socios ÷ ganancia de los últimos 12 meses | Si los socios retiran más de lo que el negocio gana. |
| Presupuesto | Real contra presupuesto | Ventas, gastos y ganancia del período ÷ lo presupuestado | Si el año se está cumpliendo como se planeó. |

## Criterios comunes

- **Pesos de hoy, pesos históricos y dólares.** Con inflación, comparar pesos de distintos meses engaña. La vista principal reexpresa cada mes a pesos de agosto de 2026 con el IPC del INDEC. La vista histórica sirve para conciliar con el banco y los impuestos, y la de dólares usa el MEP del último día hábil de cada mes.
- **Comparaciones.** Cada número se compara con el mes anterior y con el mismo mes del año anterior. Los indicadores que ya son porcentajes se comparan en puntos, los de días en días y la caja generada en pesos.
- **Metas.** Ventas, ganancia y margen salen del presupuesto 2026 en Excel. El resto son parámetros del negocio guardados en `kpi.parametros`, con su descripción y quién los aprobó.
- **Estados.** Un indicador está en orden si cumple la meta, en alerta si queda a menos de 10% de cumplirla y en crítico si queda más lejos.
