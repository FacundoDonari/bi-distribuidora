# Calidad de datos

Antes de interpretar un número hay que saber cuánto vale el dato que lo sostiene. Cada vez que se cargan datos, el warehouse corre una **auditoría de 63 controles automáticos** y **da aviso** de qué parte del dato es confiable y qué hay que mirar con cuidado.

Esta auditoría vive en el warehouse (esquema `audit`). En el tablero, la sección **Origen de los datos** muestra de qué tablas sale cada indicador.

**Resultado de la última auditoría: apto para interpretar los indicadores, con salvedades.**

## Sobre qué datos se hace

| Dónde | Qué se revisa |
| --- | --- |
| Copia del ERP (`raw`) | Que las 145 tablas lleguen completas, y las de movimientos: facturas y recibos de clientes, facturas y pagos a proveedores, pedidos, órdenes de compra, movimientos de stock y libro mayor |
| Modelo (`marts`) | Los 13 hechos y las 8 dimensiones |
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
- **El ERP no registra recepciones, conciliación bancaria ni presupuesto.** El presupuesto 2026 se carga desde un Excel aparte (el tablero no lo muestra).
- **286 artículos activos no se vendieron en el último año.** Conviene depurar el catálogo.
