# Calidad de datos

Antes de mostrar un número, hay que saber si es confiable. Por eso, cada vez que se cargan datos nuevos, el warehouse corre **55 controles automáticos**, como un auditor que revisa los papeles antes de firmar. Si algo no cierra, el tablero no se publica.

**Resultado de la última revisión: apto para usar, con cuatro salvedades menores.**

## Cómo funciona, en cuatro pasos

**1. Cada control hace una pregunta concreta y se pone una nota de 0 a 100.**

> *¿Qué parte de las facturas tiene un vendedor asignado?* → el 100% → nota **100**.
> *¿Qué parte de los productos tuvo algún movimiento de stock?* → 1.184 de 1.460, el 81% → nota **81**.

**2. Los controles se agrupan en cuatro preguntas grandes.**

| Pregunta | Ejemplo de control | Nota |
| --- | --- | --- |
| ¿Se cargan los datos que hacen falta? | Cada venta tiene costo, porque sin costo no hay margen | 95 |
| ¿Los datos cubren toda la operación? | No hay meses sin ventas ni clientes sin movimiento | 87 |
| ¿Los datos están al día y completos? | Llegaron al warehouse las mismas filas que había en el ERP | 89 |
| ¿Los números cierran? | Las ventas del tablero coinciden con la contabilidad | 99 |

**3. Con las notas se decide si se puede usar el tablero, como un semáforo.**

| Nota del grupo | Estado | Qué se hace |
| --- | --- | --- |
| Más de 75 | Bueno | Se publica |
| 50 a 75 | Parcial | Se publica |
| 25 a 50 | Deficiente | Se publica, avisando qué falta |
| 25 o menos | Crítico | **No se publica** hasta corregir el origen |

**4. Cada revisión se compara con la anterior.** Si un control empeora, la actualización se frena y avisa cuál fue. Así un error en la carga nunca llega al tablero sin que nadie lo vea.

## El control más importante: cada número se calcula dos veces

La idea viene de la contabilidad: si un número se puede calcular por dos caminos distintos y los dos dan lo mismo, el número está bien. Si no dan lo mismo, uno de los dos tiene un error.

**Ejemplo con las ventas de todo el período:**

| Camino | Resultado |
| --- | --- |
| Sumando las facturas en el warehouse | $ 3.883.278.522 |
| Sumando las facturas en el ERP | $ 3.883.278.530 |
| Mirando la cuenta Ventas del libro mayor | $ 3.883.278.530 |
| **Diferencia** | **$ 7,77** en 3.883 millones (redondeo de centavos) |

Los tres caminos coinciden, así que las ventas son confiables. La diferencia de centavos baja la nota a 95 en lugar de 100, para que quede a la vista.

Lo mismo se hace con:

- **Lo que te deben los clientes:** la suma de facturas sin cobrar coincide con la cuenta Deudores por ventas.
- **Lo que le debés a proveedores:** la suma de facturas sin pagar coincide con la cuenta Proveedores.
- **El stock:** las unidades del warehouse coinciden con las del ERP, y su valor con la cuenta Mercaderías.
- **La partida doble:** cada asiento contable suma cero.
- **El balance:** el activo es igual al pasivo más el patrimonio, en todas las fechas.
- **La caja:** lo cobrado, menos lo pagado, menos los gastos, da exactamente el saldo de caja y bancos.

## Lo que hoy no da 100, y por qué no es grave

- **Un solo depósito tiene movimiento.** El control de depósitos da bajo porque el negocio usa uno solo. No es un error: el control queda listo para cuando haya más.
- **La carga es manual.** El control de frescura baja si pasan días sin cargar. Con una carga diaria, sube solo.
- **El ERP no tiene presupuesto ni conciliación bancaria.** El presupuesto viene de un Excel aparte.
- **El 3,2% de las ventas no tiene pedido.** Son ventas sin pedido previo. No es un error, pero esas ventas no entran en la medición de entregas a tiempo.
