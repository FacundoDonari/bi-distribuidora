# Modelo de datos

El warehouse sigue un modelo dimensional (Kimball): **13 tablas de hechos** que comparten **8 dimensiones conformadas**. Que las dimensiones sean las mismas para todos los hechos es lo que permite cruzar procesos distintos: por ejemplo, ventas y cobranzas del mismo cliente, o compras y stock del mismo producto, sin reconciliar a mano.

## Capas

| Capa | Rol | Qué se hace |
| --- | --- | --- |
| `raw` | Copia fiel del ERP | Las 145 tablas de webERP, sin transformar. Si algo se rompe más adelante, siempre se puede volver al dato original. |
| `staging` | Limpieza | Nombres de negocio (factura, cobro, pedido, asiento), tipos correctos, signos normalizados y claves de negocio. |
| `marts` | Modelo dimensional | Hechos y dimensiones listos para analizar. |
| `kpi` | Lógica de negocio | Una vista por indicador, con meta, estado y causa. El tablero solo lee esta capa. |
| `audit` | Calidad | Registro de cargas, controles, linaje y veredicto. |

Cada capa solo lee de la anterior. Así cualquier número del tablero se puede seguir hacia atrás hasta la fila del ERP que lo originó.

## El proceso central: ventas

![Modelo estrella de ventas](modelo_estrella_ventas.png)

## Matriz de procesos y dimensiones

Cada fila es un proceso del negocio (un hecho) y cada columna una dimensión. Una marca indica que ese hecho se puede analizar por esa dimensión.

| Hecho | Calendario | Cliente | Producto | Canal | Vendedor | Depósito | Proveedor | Cuenta |
| --- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| `fact_ventas` | ● | ● | ● | ● | ● | ● | | |
| `fact_pedidos` | ● | ● | ● | ● | ● | ● | | |
| `fact_cobranzas` | ● | ● | | | | | | |
| `fact_cuenta_corriente_cliente` | ● | ● | | ● | ● | | | |
| `fact_compras` | ● | | ● | | | ● | ● | |
| `fact_ordenes_compra` | ● | | ● | | | ● | ● | |
| `fact_pagos_proveedor` | ● | | | | | | ● | |
| `fact_cuenta_corriente_proveedor` | ● | | | | | | ● | |
| `fact_stock_movimiento` | ● | ● | ● | | | ● | | |
| `fact_stock_saldo` | ● | | ● | | | ● | | |
| `fact_mayor` | ● | | | | | | | ● |
| `fact_presupuesto` | mes | | | | | | | |
| `fact_metas_ventas` | mes | | | ● | ● | | | |

## Hechos: grano y tipo

El **grano** es la decisión más importante de cada hecho: define qué representa una fila y, por lo tanto, qué se puede sumar sin duplicar.

| Hecho | Una fila es… | Tipo | Medidas principales |
| --- | --- | --- | --- |
| `fact_ventas` | Una línea de factura de venta | Transaccional | Cantidad, importe neto, costo, margen bruto |
| `fact_pedidos` | Una línea de pedido | Snapshot acumulativo: se actualiza al despachar y facturar | Cantidad pedida y facturada, entrega a tiempo y completa, días de ciclo |
| `fact_cobranzas` | La aplicación de un cobro a una factura | Transaccional | Importe aplicado, días desde la factura, días contra el vencimiento |
| `fact_cuenta_corriente_cliente` | Una factura de venta con su estado de cobro | Snapshot acumulativo | Importe, cobrado, saldo, días vencida |
| `fact_compras` | Una línea de factura de compra | Transaccional | Cantidad, importe neto |
| `fact_ordenes_compra` | Una línea de orden de compra | Snapshot acumulativo | Cantidad ordenada, recibida y facturada, plazo de entrega |
| `fact_pagos_proveedor` | Un pago a un proveedor | Transaccional | Importe |
| `fact_cuenta_corriente_proveedor` | Una factura de compra con su estado de pago | Snapshot acumulativo | Importe, pagado, saldo, días vencida |
| `fact_stock_movimiento` | Un movimiento de stock | Transaccional | Entradas, salidas, stock resultante, valor |
| `fact_stock_saldo` | Producto × depósito × fecha de cierre | Snapshot periódico | Unidades y valor del stock |
| `fact_mayor` | Una línea de asiento contable | Transaccional | Debe, haber, importe |
| `fact_presupuesto` | Mes × rubro del presupuesto | Plan | Importe presupuestado |
| `fact_metas_ventas` | Mes × canal × vendedor | Plan | Meta en pesos y en unidades |

## Decisiones de modelado

- **Dimensiones conformadas.** Cliente, producto, canal, vendedor, depósito, proveedor, cuenta y calendario son una sola tabla cada una, compartida por todos los hechos.
- **Calendario propio.** `dim_calendario` tiene una fila por día, con año, trimestre, mes, semana ISO y dos marcas que usan todos los indicadores: `es_mes_abierto` (el mes todavía no cerró) y `es_hasta_corte` (la fecha está dentro del período analizado). Comparar contra el mismo mes del año anterior o excluir el mes en curso es un filtro, no una fórmula.
- **Dimensiones degeneradas.** El número de factura y el de pedido quedan en el hecho, sin tabla propia: no tienen atributos que valga la pena separar y sirven para trazar cada línea.
- **Medidas calculadas en el hecho.** El margen bruto se calcula una sola vez, en `fact_ventas`, para que todas las vistas usen el mismo número.
- **Saldos derivados de movimientos.** El stock y las cuentas corrientes se calculan desde los movimientos y no desde campos de saldo del ERP, que pueden quedar desactualizados. La auditoría verifica que ambos coincidan.
- **El libro mayor como referencia.** `fact_mayor` permite cruzar cada cifra comercial contra la contabilidad: ventas contra la cuenta de ventas, deudores contra la cuenta Deudores por ventas, stock contra la cuenta Mercaderías.
- **La lógica de negocio en `kpi`, no en el tablero.** El tablero no calcula nada: lee vistas que ya traen el indicador, su meta, su estado y su comparación. Si cambia una definición, se cambia en un solo lugar.
- **Inflación como dimensión de análisis.** Las series de IPC y dólar MEP viven en `kpi` y se aplican al final, así el mismo modelo se puede leer en pesos de hoy, en pesos históricos o en dólares.
