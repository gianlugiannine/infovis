# Un mes y medio en La Percanta

Trabajo práctico de **Visualización de la Información** (2.º cuatrimestre 2026).
Exploración visual de cinco semanas de pedidos de La Percanta, un restaurante de Puerto
Madero cuya aplicación de pedidos es un proyecto propio.

**→ [gianlugiannine.github.io/infovis](https://gianlugiannine.github.io/infovis/)** · [ejercicios de clase](https://gianlugiannine.github.io/infovis/practica.html)

## Las cuatro preguntas

| # | Pregunta | Herramienta |
|---|---|---|
| 1 | ¿Cuándo se llena la cocina? | Datawrapper — tabla heatmap |
| 2 | ¿Se pide distinto según la hora del día? | RawGraphs — diagrama aluvial |
| 3 | ¿Cuánta carta trabaja de verdad? | Flourish — treemap jerárquico |
| 4 | ¿El local cumple la demora que promete? | Tableau Public — barras de desvío |

## Los datos

158 pedidos entre el 1 de agosto y el 5 de septiembre de 2026. **Los cuatro gráficos usan
los 143 que se entregaron**, dejando afuera los 15 cancelados.

La carta es real: 357 productos con sus precios, códigos y modificadores, extraídos de la
base de producción con consultas de solo lectura. **De los pedidos, 58 son reales y 100
fueron generados** sobre esa misma carta, porque la aplicación se puso en marcha hace poco
y su volumen todavía no alcanza para leer patrones de consumo. La columna `lote` los
distingue (`R` real, `S` generado) y el sitio lo declara arriba de todo.

Ver `datos/LEEME.md` para el detalle de columnas, la metodología y los criterios de
limpieza.

Sin datos personales: los nombres de cliente son seudónimos, y no hay teléfonos,
direcciones ni correos (Ley 25.326).
