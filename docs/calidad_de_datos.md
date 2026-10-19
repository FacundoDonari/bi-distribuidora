# Calidad de datos

Antes de interpretar un indicador hay que saber si los datos que lo alimentan son confiables. Por eso cada carga del warehouse pasa por una auditoría automática de **55 controles**, y los indicadores solo se usan si el veredicto lo permite.

**Última auditoría: "Apto para interpretar indicadores (con salvedades)".**

| Eje | Pregunta que responde | Controles | Nota |
| --- | --- | --- | --- |
| Aprovechamiento del sistema | ¿Se cargan en el ERP los datos que los indicadores necesitan? | 11 | 95,1 |
| Cobertura de las operaciones | ¿Los datos cubren toda la operación? | 8 | 86,5 |
| Frescura | ¿Los datos están al día y completos? | 7 | 89,1 |
| Consistencia | ¿Los números cierran entre sí y con la contabilidad? | 29 | 98,9 |

## Los pasos

1. **Medir.** Cada control mide algo concreto (por ejemplo, cuántas facturas tienen vendedor asignado), lo compara con lo esperado y le pone un puntaje de 0 a 100.
2. **Ponderar.** Cada eje recibe una nota: el promedio de sus controles, ponderado por la importancia de cada uno. Los controles informativos se muestran, pero no suman.
3. **Calificar.** Más de 75 es Bueno, de 50 a 75 Parcial, de 25 a 50 Deficiente y 25 o menos Crítico.
4. **Decidir.** Si algún eje queda Crítico, no se avanza: hay que corregir la fuente. Si alguno queda Deficiente, se avanza con salvedades explícitas. Si no, los indicadores son aptos. Todo control con menos de 100 se informa como salvedad.
5. **Comparar con la carga anterior.** Cada auditoría queda guardada. Si un control empeora respecto de la anterior, la actualización se detiene y el tablero no se publica.

## Qué se controla en cada eje

- **Aprovechamiento:** que cada factura tenga vendedor, canal y pedido de origen; que cada línea de venta tenga costo, porque sin costo no hay margen; que cada pedido tenga fecha prometida, porque sin ella no se pueden medir las entregas a tiempo.
- **Cobertura:** que los clientes, productos y proveedores del maestro tengan movimiento; que no haya meses sin datos; que cada asiento contable tenga su comprobante de origen.
- **Frescura:** cuánto tiempo pasó desde la última carga, cuánto tarda un hecho en registrarse y si cada tabla llegó completa (filas en el ERP = filas en el warehouse).
- **Consistencia:** el eje más importante, explicado abajo.

## Consistencia: cada cifra por dos caminos

La idea viene de la contabilidad: **cada cifra se calcula por al menos dos caminos independientes y se comparan.** Si un número sale mal por un lado, los otros lo delatan.

| Qué se verifica | Cómo |
| --- | --- |
| Partida doble | Cada asiento del libro mayor suma cero |
| Ventas | Facturas del modelo = facturas del ERP = cuenta de ventas del mayor |
| Lo que te deben | Cuenta corriente de clientes = saldos del ERP = cuenta Deudores por ventas |
| Lo que debés a proveedores | Cuenta corriente de proveedores = saldos del ERP = cuenta Proveedores |
| Costo de ventas | Costo de las líneas de venta = cuenta Costo de mercaderías vendidas |
| Stock | Stock valorizado = cuenta Mercaderías; unidades del modelo = unidades del ERP |
| Balance | Activo = pasivo + patrimonio en cada fecha |
| Flujo de fondos | Cobranzas − pagos − gastos acumulados = saldo de caja y bancos |
| Resultados | Suma de los resultados mensuales = resultados acumulados del balance |
| Tablero | Cada número del tablero = su indicador en el warehouse |

Una diferencia de centavos por redondeo baja el puntaje a 95 en lugar de 100, para que quede a la vista.

## Salvedades actuales

- **Un solo depósito con movimiento:** la apertura por depósito todavía no aporta.
- **Rezago de carga:** en el piloto la carga es manual. Con carga diaria, este control sube solo.
- **Fuentes que el ERP no tiene:** no hay presupuesto nativo, recepciones de compra ni conciliación bancaria. El presupuesto viene de un Excel.
- **Ventas directas:** 3,2% de las ventas no tienen pedido de origen. No es un error, pero esas ventas no entran en la medición de entregas a tiempo.
