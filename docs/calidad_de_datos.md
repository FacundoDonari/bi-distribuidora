# Calidad de datos

Antes de interpretar un número hay que saber cuánto vale el dato que lo sostiene. Cada vez que se cargan datos, el warehouse corre una **auditoría de 63 controles automáticos** y **da aviso** de qué parte del dato es confiable y qué hay que mirar con cuidado.

Esta auditoría vive en el warehouse (esquema `audit`). En el tablero, la sección **Origen de los datos** muestra de qué tablas sale cada indicador.

**Resultado de la última auditoría: apto para interpretar los indicadores, con salvedades.**

## Sobre qué datos se hace

| Dónde | Qué se revisa |
| --- | --- |
| Copia del ERP (`raw`) | Que las 145 tablas lleguen completas, y las de movimientos: facturas y recibos de clientes, facturas y pagos a proveedores, pedidos, órdenes de compra, movimientos de stock y libro mayor |
| Modelo (`marts`) | Los 12 hechos y las 8 dimensiones |
| Indicadores (`kpi`) | Que cada número del tablero sea igual al de su indicador |
| Registro de cargas | Cuándo se cargó cada tabla y con cuántas filas |

## Los cuatro ejes

Cada control se califica de 0 a 100 y la nota de cada eje es el promedio de sus controles.

| Eje | Pregunta | Qué controla, por ejemplo | Nota |
| --- | --- | --- | --- |
| **Aprovechamiento del sistema** | ¿El dato que necesito existe? | Que cada venta tenga costo, vendedor y canal, y que los artículos del catálogo se vendan | 93,8 |
| **Cobertura** | ¿Sobre qué parte del negocio estoy opinando? | Que no falten meses, días ni clientes, y que la numeración de facturas no tenga saltos | 90,6 |
| **Frescura** | ¿El dato es de hoy o de hace dos meses? | Cuántos días pasaron entre el último movimiento y la carga | 86,2 |
| **Veracidad** | ¿Dos caminos llegan al mismo número? | Que las ventas, el costo, los saldos y el stock coincidan con el libro mayor | 99,7 |

Van en ese orden porque cada eje se apoya en el anterior: no sirve comprobar que un número cierra si antes no se sabe que el dato existe, está completo y está al día.

## Los 63 controles

Cada control mide algo, lo compara con lo esperado y le pone un puntaje de 0 a 100. Si algún control empeora respecto de la carga anterior, `actualizar.py` frena y avisa que no hay que publicar hasta revisarlo.

### 1. Aprovechamiento del sistema (12): ¿el dato existe?

| Código | Qué controla | Puntaje |
| --- | --- | --- |
| D1.01 | Facturas de venta con vendedor asignado | 100 |
| D1.02 | Facturas de venta con canal asignado | 100 |
| D1.03 | Facturas de venta con el pedido del que salieron | 96 |
| D1.04 | Líneas de venta con costo cargado | 100 |
| D1.05 | Facturas de compra con su orden de compra | 98 |
| D1.06 | Clientes con canal, condición de pago y fecha de alta | 100 |
| D1.07 | Artículos con costo y categoría | 100 |
| D1.08 | Líneas de pedido con fecha prometida de entrega | 100 |
| D1.09 | Líneas de asiento con descripción | 100 |
| D1.10 | Fuentes esperadas que tienen datos | 57 |
| D1.11 | Uso de descuentos (informativo) | 0 |
| D1.12 | Artículos activos vendidos en los últimos 12 meses | 80 |

### 2. Cobertura (11): ¿está todo o falta algo?

| Código | Qué controla | Puntaje |
| --- | --- | --- |
| D2.01 | Clientes del maestro que tienen ventas | 100 |
| D2.02 | Artículos del maestro con movimiento de stock | 81 |
| D2.03 | Proveedores del maestro con compras | 100 |
| D2.04 | Depósitos con movimiento | 25 |
| D2.05 | Meses del período con actividad | 100 |
| D2.06 | Líneas del mayor con documento de origen | 100 |
| D2.07 | Ventas atribuibles a un cliente identificado | 100 |
| D2.08 | Tablas del ERP cargadas (informativo) | 81 |
| D2.09 | Numeración de comprobantes sin saltos | 100 |
| D2.10 | Cada número posterior tiene fecha igual o posterior | 100 |
| D2.11 | Días de atención con facturación | 100 |

### 3. Frescura (8): ¿el dato está al día?

| Código | Qué controla | Puntaje |
| --- | --- | --- |
| D3.01 | Horas desde la última carga | 80 |
| D3.02 | Días entre el último movimiento y la carga | 25 |
| D3.03 | Días entre el hecho y su registro en el sistema | 100 |
| D3.04 | Diferencia de fecha de corte entre fuentes | 100 |
| D3.05 | Registros con fecha posterior a la carga | 100 |
| D3.06 | Tablas de la copia del ERP con sello de carga | 99 |
| D3.07 | Tablas del último lote cargadas completas | 100 |
| D3.08 | Fuentes al día para la decisión que soportan (informativo) | 13 |

Los dos primeros dependen de la hora en que se corre la carga, así que `actualizar.py` los informa pero no frena por ellos.

### 4. Veracidad (32): ¿los números cierran por más de un camino?

| Código | Qué controla | Puntaje |
| --- | --- | --- |
| D4.01 | Cada asiento cierra la partida doble | 100 |
| D4.02 | No se pierden filas entre la copia del ERP y el modelo | 100 |
| D4.03 | Ventas: modelo = facturas del ERP = cuenta Ventas del mayor | 100 |
| D4.04 | Lo que deben los clientes: modelo = ERP = mayor | 100 |
| D4.05 | Lo que se debe a proveedores: modelo = ERP = mayor | 100 |
| D4.06 | Costo de ventas: modelo = mayor | 100 |
| D4.07 | Valor del stock: modelo = cuenta Mercaderías del mayor | 100 |
| D4.08 | Stock en unidades: modelo = ERP | 100 |
| D4.09 | Stock sin saldos negativos | 100 |
| D4.10 | Stock del ERP = suma de sus movimientos | 100 |
| D4.11 | Claves de negocio sin repetidos | 100 |
| D4.12 | IVA coherente con la tasa del 21% | 100 |
| D4.13 | Cobros totalmente aplicados a facturas | 100 |
| D4.14 | Pagos a proveedores: aplicado en facturas = pagos emitidos | 100 |
| D4.15 | Pedidos vs ventas: cantidades e importes facturados | 100 |
| D4.16 | Facturas cuyo pedido existe en el modelo | 97 |
| D4.17 | Cuentas corrientes sin saldos negativos | 100 |
| D4.18 | Columnas del modelo con su origen documentado | 93 |
| D4.19 | Líneas de venta sin cantidades ni importes negativos | 100 |
| D4.20 | Caja y bancos nunca negativos | 100 |
| D4.21 | Comparativos del tablero = indicadores base | 100 |
| D4.22 | El balance cuadra: activo = pasivo + patrimonio | 100 |
| D4.23 | Cuentas del mayor con movimiento y con rubro contable | 100 |
| D4.24 | El flujo de fondos concilia con el saldo de caja y bancos | 100 |
| D4.25 | El resultado acumulado coincide con el patrimonio | 100 |
| D4.26 | Ventas del mayor = ventas del módulo comercial | 100 |
| D4.27 | Cada indicador clave tiene su ficha completa | 100 |
| D4.28 | Facturas vencidas = vencido ÷ saldo a cobrar | 100 |
| D4.29 | Facturas de apertura fuera de las ventas (informativo) | 100 |
| D4.30 | Cada clave de los hechos existe en su dimensión | 100 |
| D4.31 | Márgenes razonables: líneas con margen entre 0% y 60% | 100 |
| D4.32 | Precios razonables: ninguna línea a menos del 30% del precio típico | 100 |

Veracidad tiene más controles porque es el eje que confirma que los números del tablero son correctos. Cada control cubre un error distinto que puede pasar en la vida real: una factura borrada, un costo sin cargar, una carga a medias o un asiento descuadrado.

### Los controles con menos de 80, en simple

Ninguno indica un error en los números del tablero. Muestran límites del sistema o de la carga:

- **D1.10 Fuentes esperadas con datos (57):** 3 de las 7 fuentes que se esperaban están vacías, porque la empresa no usa esos módulos del ERP: recepciones de mercadería, conciliación bancaria y planificación contable.
- **D1.11 Uso de descuentos (0, informativo):** ninguna venta tiene descuento. Puede ser que el negocio no los use, por eso no suma a la nota.
- **D2.04 Depósitos con movimiento (25):** el ERP tiene 4 depósitos cargados, pero se usa 1 solo. No es un error: es cómo trabaja la empresa.
- **D3.02 Días entre el último movimiento y la carga (25):** la carga se hizo 29 días después del último movimiento. Alcanza para un análisis mensual, pero no para decidir la cobranza del día. Mejora sola con cargas más frecuentes.
- **D3.08 Fuentes al día (13, informativo):** es el mismo atraso de D3.02, visto fuente por fuente. No suma a la nota para no contarlo dos veces.

## Ejemplo: las ventas calculadas por tres caminos

| Camino | Ventas de todo el período |
| --- | --- |
| Líneas de factura en el warehouse | $ 3.883.278.529,96 |
| Facturas en el ERP | $ 3.883.278.529,96 |
| Cuenta Ventas del libro mayor | $ 3.883.278.529,96 |

Los tres dan lo mismo, al centavo. Si alguno no coincidiera, la auditoría lo marcaría con su diferencia.

## Qué avisa

Si un eje queda en Crítico (25 puntos o menos), avisa que los indicadores que dependen de él no se deberían interpretar todavía. Si hay controles bajos, avisa cuáles son las salvedades. Y si un control empeora respecto de la auditoría anterior, avisa cuál fue.

## Salvedades de hoy

- **La carga llegó 29 días después del último movimiento.** Alcanza para el análisis mensual, no para decidir la cobranza del día.
- **Se usa un solo depósito de los cuatro.** No es un error.
- **El ERP no registra recepciones ni conciliación bancaria.**
- **286 artículos activos no se vendieron en el último año.** Conviene depurar el catálogo.
