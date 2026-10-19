# Calidad de datos

Antes de mostrar un número hay que saber si se puede confiar en él. Por eso, cada vez que se cargan datos nuevos, el warehouse corre una **auditoría de 55 controles automáticos**, como un auditor que revisa los papeles antes de firmar. La auditoría no reemplaza el criterio de quien mira el tablero: **da aviso**. Dice qué parte del dato es confiable, qué hay que mirar con cuidado y por qué, y ese resultado se ve en la pestaña Confiabilidad del tablero.

**Resultado de la última auditoría: apto para interpretar los indicadores, con salvedades.**

## Por qué estas cuatro preguntas

Para que un número del tablero sea confiable tienen que cumplirse cuatro condiciones. Si falla cualquiera, el número puede estar mal aunque la cuenta esté bien hecha. Son las mismas que revisa un auditor contable cuando controla un saldo: que exista, que esté completo, que corresponda al período y que sea exacto.

| Pregunta | Por qué importa | Ejemplo de control | Nota |
| --- | --- | --- | --- |
| **1. ¿El dato existe?** (uso del sistema) | Lo que el sistema no registra no se puede medir. Sin costo en cada venta no hay margen; sin fecha prometida no se sabe si se entregó a tiempo. | Las 88.540 líneas de venta tienen costo | 95,1 |
| **2. ¿Cubre toda la operación?** (cobertura) | Un dato puede estar bien cargado pero cubrir solo una parte. Si faltara un mes o un grupo de clientes, los totales darían menos que la realidad y nadie lo notaría. | Los 24 meses tienen ventas y los 123 clientes compraron | 86,5 |
| **3. ¿Está al día y llegó completo?** (frescura) | Un número correcto pero viejo hace decidir tarde, y una carga cortada a la mitad muestra totales incompletos. | Las 145 tablas llegaron con las mismas filas que tiene el ERP | 86,2 |
| **4. ¿Los números cierran?** (consistencia) | Es la prueba final: el mismo número, calculado por caminos distintos, tiene que dar lo mismo. | Las ventas del warehouse, del ERP y del libro mayor son iguales | 99,6 |

Van en ese orden porque cada una se apoya en la anterior. No sirve comprobar que las ventas cierran con la contabilidad si antes no se sabe que están todas cargadas y al día.

## Cómo se pone la nota

1. **Cada control hace una pregunta concreta y se califica de 0 a 100.** Por ejemplo: *¿qué parte de las facturas tiene vendedor asignado?* → 30.129 de 30.129 → nota **100**. *¿Qué parte de los productos tuvo algún movimiento de stock?* → 1.184 de 1.460 → nota **81**.
2. **La nota de cada pregunta es el promedio de sus controles.** Algunos controles son solo informativos y no suman: por ejemplo, si las ventas tienen descuentos. Que el negocio no haga descuentos no es un error.
3. **Cada nota tiene un estado:** Bueno (más de 75), Parcial (de 50 a 75), Deficiente (de 25 a 50) o Crítico (25 o menos).

## Qué avisa la auditoría

- **Si una de las cuatro preguntas queda en Crítico**, avisa que los indicadores que dependen de ella no se deberían interpretar hasta corregir el origen.
- **Si queda en Deficiente o hay controles bajos**, los indicadores se pueden usar, pero con salvedades, y la auditoría dice cuáles.
- **Si un control empeora respecto de la auditoría anterior**, avisa cuál fue y cuánto bajó. Así, un error de carga nunca pasa sin que nadie lo vea.

## El control más importante: cada número se calcula por más de un camino

La idea viene de la contabilidad: si un número se puede calcular por caminos distintos y todos dan lo mismo, el número está bien. Si no dan lo mismo, alguno tiene un error.

**Ejemplo con las ventas de todo el período:**

| Camino | Resultado |
| --- | --- |
| Sumando las líneas de factura en el warehouse | $ 3.883.278.529,96 |
| Sumando las facturas en el ERP | $ 3.883.278.529,96 |
| Mirando la cuenta Ventas del libro mayor | $ 3.883.278.529,96 |

Los tres caminos dan exactamente lo mismo, al centavo: las ventas son confiables. Lo mismo se controla con:

- **El costo de lo vendido:** el costo de cada venta suma lo mismo que la cuenta Costo de ventas.
- **Lo que te deben los clientes:** las facturas sin cobrar suman lo mismo que la cuenta Deudores por ventas.
- **Lo que le debés a proveedores:** las facturas sin pagar suman lo mismo que la cuenta Proveedores.
- **El stock:** las unidades coinciden con las del ERP y su valor con la cuenta Mercaderías.
- **La partida doble:** cada asiento contable suma cero.
- **El balance:** el activo es igual al pasivo más el patrimonio, en todas las fechas.
- **La caja:** lo cobrado, menos lo pagado, menos los gastos, da exactamente el saldo de caja y bancos.
- **El tablero:** cada número que muestra es igual al de su indicador en el warehouse.

## Las salvedades de hoy

- **Un solo depósito tiene movimiento.** El control de depósitos da bajo porque el negocio usa uno solo. No es un error: el control queda listo para cuando haya más.
- **Los datos se cargaron 29 días después del último movimiento.** Por eso el control de frescura baja. Con el ERP en uso diario y una carga por día, da 100.
- **El ERP no registra presupuesto ni conciliación bancaria.** El presupuesto se toma de un Excel aparte.
- **El 4,2% de las facturas no tiene pedido previo.** No es un error, pero esas ventas no entran en la medición de entregas a tiempo.
- **276 de los 1.460 productos nunca tuvieron movimiento.** Están en el catálogo pero no se compraron ni se vendieron en el período.
