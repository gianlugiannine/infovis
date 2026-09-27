# Dos meses de pedidos en La Percanta

Trabajo práctico de **Visualización de la Información** (2.º cuatrimestre 2026).
Exploración visual de los pedidos reales que entraron por la aplicación web de La
Percanta, un restaurante de Puerto Madero cuya aplicación de pedidos es un proyecto propio.

**→ [gianlugiannine.github.io/infovis](https://gianlugiannine.github.io/infovis/)** · [ejercicios de clase](https://gianlugiannine.github.io/infovis/practica.html)

## Las cuatro preguntas

| # | Pregunta | Herramienta |
|---|---|---|
| 1 | ¿Cuándo se llena la cocina? | Datawrapper — tabla heatmap día × hora |
| 2 | ¿Se pide distinto según la hora del día? | RawGraphs — diagrama aluvial |
| 3 | ¿Cuánta carta trabaja de verdad? | Flourish — treemap jerárquico |
| 4 | ¿Desde dónde piden el delivery, y cuánto gastan? | Tableau Public — tablero de cuatro gráficos (dos mapas y dos barras) |

## Los datos

124 pedidos entre el 4 de agosto y el 26 de septiembre de 2026, **todos reales**, leídos
de la base de producción con consultas de solo lectura. **Los cuatro gráficos usan los
119 que se entregaron**, dejando afuera los 5 cancelados.

Ver `datos/LEEME.md` para el detalle de columnas y criterios.

Sin datos personales: los clientes son códigos, y no hay nombres, teléfonos, correos ni
direcciones (Ley 25.326).
