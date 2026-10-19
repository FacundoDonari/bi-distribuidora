# Calidad de datos

Antes de interpretar un número hay que saber cuánto vale el dato que lo sostiene. Por eso, cada vez que se cargan datos nuevos, el warehouse corre una **auditoría de 63 controles automáticos**, organizados en cuatro ejes que califican la evidencia antes de mirar los indicadores y marcan dónde poner el juicio. La auditoría no reemplaza ese juicio: **da aviso**. Dice qué parte del dato es confiable, qué hay que mirar con cuidado y por qué, y el resultado se ve en la pestaña Confiabilidad del tablero.

**Resultado de la última auditoría: apto para interpretar los indicadores, con salvedades.**

| Eje | Qué mide | Pregunta de auditoría | Nota |
| --- | --- | --- | --- |
| **D1 · Aprovechamiento del sistema** (campos habilitados y vacíos) | De todo lo que el ERP permite registrar, cuánto se registra de verdad | ¿El dato que necesito para la prueba existe realmente? | 93,8 |
| **D2 · Cobertura de las operaciones** (universo alcanzado) | Qué parte de la actividad real quedó dentro del dato | ¿Sobre qué parte del negocio estoy opinando? | 90,6 |
| **D3 · Frescura** (rezago y anticipación) | La distancia entre el hecho y su registro, en los dos sentidos | ¿Este dato describe la empresa de hoy o la de hace dos meses? | 86,2 |
| **D4 · Veracidad** (consistencia interna) | Si el dato se sostiene contra sí mismo y contra las otras fuentes | ¿Dos caminos independientes llegan al mismo número? | 99,7 |

Los ejes van en ese orden porque cada uno se apoya en el anterior. No sirve comprobar que las ventas cierran con la contabilidad si antes no se sabe que el dato existe, que cubre toda la operación y que está al día. Es la misma secuencia que sigue un auditor contable con un saldo: existencia, integridad, oportunidad y exactitud.

**Cómo se pone la nota.** Cada control hace una pregunta concreta y se califica de 0 a 100. Por ejemplo: *¿qué parte de las facturas tiene vendedor asignado?* → 30.129 de 30.129 → nota **100**. La nota de cada eje es el promedio de sus controles, y cada nota tiene un estado: Bueno (más de 75), Parcial (de 50 a 75), Deficiente (de 25 a 50) o Crítico (25 o menos). Algunos controles son solo informativos: se muestran pero no suman.

## D1 · Aprovechamiento del sistema

*¿El dato que necesito para la prueba existe realmente?*

Busca campos que el sistema tiene habilitados pero vienen vacíos, y clasificaciones que no se usan. Cada hueco tiene un costo de oportunidad: el análisis que no se puede hacer.

| Qué se controla | Resultado | Qué se pierde si falta |
| --- | --- | --- |
| Vendedor y canal en cada factura | 100% | Ventas, margen y cobranza por vendedor y por canal |
| Costo en cada línea de venta | 100% | El margen bruto |
| Fecha prometida en cada pedido | 100% | La medición de entregas a tiempo |
| Pedido de origen en cada factura | 96% | Entregas a tiempo de esas ventas |
| Orden de compra en cada factura de proveedor | 98% | Los plazos de entrega de los proveedores |
| Artículos activos que se vendieron en el último año | 80% (286 sin venta) | Rotación, sobrestock y faltantes por producto |
| Fuentes que el ERP permite y tienen datos | 4 de 7 | Recepciones de mercadería, conciliación bancaria y presupuesto |
| Uso de descuentos (informativo) | No se usan | El efecto de los descuentos en el margen |

## D2 · Cobertura de las operaciones

*¿Sobre qué parte del negocio estoy opinando?*

Un dato puede estar bien cargado pero cubrir solo una parte. Si faltara un mes, un grupo de clientes o un talonario de facturas, los totales darían menos que la realidad y nadie lo notaría.

- **Entidades con movimiento real:** los 123 clientes compraron y los 15 proveedores vendieron; 1.184 de los 1.460 artículos tuvieron movimiento; de los 4 depósitos, se usa uno.
- **Períodos completos:** los 24 meses y los 616 días de atención (de lunes a sábado) tienen facturas.
- **Correlatividad de comprobantes:** 64.556 facturas, recibos, facturas de proveedores y pagos, sin saltos en la numeración. Solo 16 recibos tienen una fecha anterior a la del número previo.
- **Ventas con cliente identificado:** todas. No hay ventas anónimas.
- **Conciliación entre módulos:** casi todas las líneas del libro mayor tienen su comprobante de origen; el resto son asientos manuales y amortizaciones.

## D3 · Frescura

*¿Este dato describe la empresa de hoy o la de hace dos meses?*

Mide la distancia entre el hecho económico y su registro, y también lo contrario: registros que se adelantan al hecho.

- **Del hecho al registro:** cada operación se registra el mismo día en que ocurre.
- **Corte parejo:** las fuentes principales llegan hasta la misma fecha, con un día de diferencia como máximo.
- **Registros anticipados:** ninguno tiene fecha posterior a la carga.
- **Rezago de la carga:** los datos se cargaron 29 días después del último movimiento. Es la salvedad más fuerte de este eje.

La **matriz de frescura** muestra, para cada fuente, qué tan fresca tiene que estar según la decisión que soporta:

| Fuente | Último dato | Rezago | Necesita | Decisión que soporta |
| --- | --- | --- | --- | --- |
| Facturas de venta | 18/09/2026 | 29 días | 1 día | Ventas del mes contra el presupuesto y margen |
| Cobranzas | 17/09/2026 | 30 días | 1 día | A quién cobrar y cuánta plata entra |
| Pedidos de clientes | 18/09/2026 | 29 días | 1 día | Pedidos pendientes y entregas a tiempo |
| Movimientos de stock | 18/09/2026 | 29 días | 1 día | Reposición, faltantes y sobrestock |
| Facturas de proveedores | 18/09/2026 | 29 días | 7 días | Compras de la semana y costo de reposición |
| Pagos a proveedores | 17/09/2026 | 30 días | 7 días | Pagos de las próximas semanas |
| Órdenes de compra | 15/09/2026 | 32 días | 7 días | Seguimiento de entregas de proveedores |
| Libro mayor | 18/09/2026 | 29 días | 30 días | Cierre mensual: resultado y balance |

Con este rezago, el tablero sirve para el análisis mensual pero no para decidir la cobranza del día. Con el ERP en uso diario y una carga por día, todas las fuentes quedan al día.

## D4 · Veracidad

*¿Dos caminos independientes llegan al mismo número?*

La idea viene de la contabilidad: si un número se puede calcular por caminos distintos y todos dan lo mismo, el número está bien. Si no, alguno tiene un error.

**Ejemplo con las ventas de todo el período:**

| Camino | Resultado |
| --- | --- |
| Sumando las líneas de factura en el warehouse | $ 3.883.278.529,96 |
| Sumando las facturas en el ERP | $ 3.883.278.529,96 |
| Mirando la cuenta Ventas del libro mayor | $ 3.883.278.529,96 |

Los tres caminos dan exactamente lo mismo, al centavo. Lo mismo se controla con:

- **Integridad aritmética:** cada asiento cumple la partida doble, el balance cuadra en todas las fechas, el IVA de cada factura corresponde a su tasa y la caja concilia con lo cobrado, pagado y gastado.
- **Cruce con el libro mayor:** el costo de ventas, lo que te deben los clientes, lo que le debés a proveedores y el valor del stock coinciden con sus cuentas contables.
- **Integridad referencial:** las 44 claves del modelo apuntan a filas que existen. Sobre 2,2 millones de valores revisados, no hay ninguno huérfano. Los números de factura, recibo y pago no se repiten.
- **Razonabilidad de precios y márgenes:** de 88.540 líneas de venta, solo 3 se vendieron por debajo del costo, 2 con un margen mayor a 60% y 14 a un precio alejado más de 30% del precio típico del producto en ese mes. Quedan a la vista para revisar.
- **El tablero:** cada número que muestra es igual al de su indicador en el warehouse.

## Qué avisa la auditoría

- **Si un eje queda en Crítico**, avisa que los indicadores que dependen de él no se deberían interpretar hasta corregir el origen.
- **Si queda en Deficiente o hay controles bajos**, los indicadores se pueden usar con salvedades, y la auditoría dice cuáles.
- **Si un control empeora respecto de la auditoría anterior**, avisa cuál fue y cuánto bajó. Así, un error de carga nunca pasa sin que nadie lo vea.

## Las salvedades de hoy

- **Un solo depósito tiene movimiento.** No es un error: el control queda listo para cuando haya más.
- **La carga llegó 29 días después del último movimiento.** Con una carga diaria, el eje de frescura sube solo.
- **El ERP no registra recepciones, conciliación bancaria ni presupuesto.** El presupuesto se toma de un Excel aparte.
- **286 artículos activos no se vendieron en el último año.** Conviene depurar el catálogo para que la rotación por producto sea representativa.
- **El 3,2% de las ventas (924 facturas) no tiene pedido previo.** Son ventas directas: no es un error, pero no entran en la medición de entregas a tiempo.
