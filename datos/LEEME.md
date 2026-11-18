# Dataset — TP Visualización de la Información
**La Percanta** · Puerto Madero, CABA · del 4 de agosto al 17 de noviembre de 2026
Pedidos tomados por la **aplicación web** del local. **Todos reales.**

Versión actualizada el 18 de noviembre de 2026. La entrega de septiembre (124 pedidos, hasta
el 26/09) quedó en el historial de este repositorio, en la etiqueta `entrega-2026-09-28`; al
rearmar los archivos con el script nuevo, esos 124 pedidos y sus 304 renglones dan idénticos.

## Universo
La base tiene **359 pedidos**: 352 entregados y 7 cancelados.
**Todos los gráficos del sitio usan solo los 352 entregados** — un pedido cancelado no
es trabajo hecho ni plata cobrada.

Todos entraron por la web: los pedidos por teléfono y los del salón no pasan por la
aplicación, así que el dataset describe lo que se pide por la app, no todo el restaurante.

## Archivos

| Archivo | Filas | Qué es |
|---|---|---|
| `pedidos.csv` | 359 | Un pedido por fila |
| `renglones.csv` | 815 | Un producto pedido por fila. Se une a `pedidos.csv` por `pedido` |
| `carta.csv` | 357 | La carta completa del local |
| `1-datawrapper-heatmap-dia-hora.csv` | 7 | Pedidos entregados por día de la semana × hora de entrada |
| `2-rawgraphs-alluvial-franja-rubro-entrega.csv` | 28 | Franja → rubro principal → forma de entrega |
| `3-flourish-carta.csv` | 97 | Facturación y unidades por producto vendido |
| `4-tableau-mapa-delivery.csv` | 245 | Un pedido de delivery entregado por fila, a nivel de edificio: ticket, rango, distancia al local, frecuencia del edificio, mes del pedido y mes del primer pedido del edificio |
| `5-mapa-crecimiento-edificios.csv` | 52 | Un edificio por fila: mes de su primer pedido, pedidos, plata, ticket promedio, distancia y pedidos por mes |
| `6-datawrapper-pedidos-por-semana.csv` | 15 | Pedidos entregados por semana completa, separados en primer pedido del cliente y cliente que vuelve |

## `pedidos.csv` — columnas

**Identificación** — `pedido` (número que emite la app) · `cliente` (código estable C01, C02…)
**Tiempo** — `fecha` (jornada del local) · `hora` · `hora_min` · `franja` · `dia_semana` ·
`es_finde` (sábado y domingo) · `semana` (ISO)
**Operación** — `tipo_entrega` (Delivery / Retiro en local) · `estado` (Entregado / Cancelado)
**Qué se pidió** — `productos_distintos` · `unidades` · `rubro_principal` (el rubro que se
llevó más plata del pedido) · `cant_rubros` · `rubros` · `detalle`
**Plata** — `subtotal` · `descuento` · `propina` · `total`
**Tiempos** — `demora_prometida_min` (50 delivery, 30 retiro) · `demora_real_min` (del
ingreso del pedido a la marca de entregado)

## `renglones.csv` — columnas

`pedido` · `estado` · `fecha` · `hora` · `franja` · `dia_semana` · `tipo_entrega` ·
`codigo` · `producto` · `mitad_2` (segunda mitad de una pizza mitad y mitad) · `rubro` ·
`cantidad` · `precio_unitario` · `modificadores` · `extra_modificadores` · `total_renglon`

`total_renglon = (precio_unitario + extra_modificadores) × cantidad`

## Criterios

- **Horas de Buenos Aires.** La base guarda UTC; se convirtió a UTC−3.
- **Franjas según el servicio:** Almuerzo hasta las 16:00 · Tarde de 16:00 a 19:30 ·
  Cena desde las 19:30, que es cuando la web empieza a tomar los pedidos del delivery de
  la noche.
- **«Recomendados» no es un rubro** sino una vidriera de la carta: cada plato se cuenta en
  su rubro real.
- **Aluvial:** nombre propio para los rubros que fueron principales en 3 pedidos o más;
  el resto (2 pedidos de hamburguesas) va a «Otros rubros».
- **Treemap:** una pizza mitad y mitad reparte su facturación y su unidad en partes
  iguales entre los dos sabores.
- **Clientes:** la app identifica a cada cliente por su teléfono. Un pedido es «primer
  pedido del cliente» si ese cliente no tenía otro pedido entregado antes.
- **Semanas:** de lunes a domingo, solo las completas. La del 16 de noviembre (dos días,
  12 pedidos) queda afuera del gráfico de semanas.
- **Rangos de gasto (3):** hasta $30.000 · $30.000 a $45.000 · más de $45.000 (un pedido
  de $30.000 justos cuenta en el primero y uno de $45.000 justos, en el del medio). En el
  mapa de Tableau se aplican al ticket promedio de cada edificio (11 / 19 / 22
  edificios); en las barras de distancia, a cada pedido (`rango_ticket`: 97 / 70 / 78
  pedidos).
- **Distancia:** `distancia_m` es la distancia en línea recta (haversine, radio
  6.371.000 m) desde el local hasta el edificio; `banda_distancia` la agrupa en menos de
  200 m, de 200 a 400 m y más de 400 m (60 / 91 / 94 pedidos).
- **Frecuencia del edificio:** `frecuencia_edificio` agrupa según `pedidos_edificio`
  (pedidos de delivery del edificio en todo el período): una sola vez, de 2 a 4, o 5 o
  más (14 / 18 / 20 edificios).
- **Mes del primer pedido:** `primer_mes_edificio` (y `primer_pedido` en el archivo 5) es
  el mes del primer delivery entregado a ese edificio: agosto 14, septiembre 23, octubre
  11 y noviembre 4 edificios. Los meses llevan número adelante («1 · Agosto») para que se
  ordenen bien también como texto.
- **Mapa:** calle y altura geocodificadas con el normalizador de la Ciudad (USIG). Antes
  se corrigieron a mano los errores de tipeo en el nombre de la calle («Marta Salotti»,
  «Olga Cosettini», «Rosario Vera Peñalosa»…). Juana Manso 1965 no existe en el
  callejero oficial y se ubicó en la última altura válida de la calle (1900). Benjamín
  Lavaisse 1056 no la ubica la USIG por altura y se ubicó con el georreferenciador
  nacional (apis.datos.gob.ar/georef). Un edificio es un punto: dos direcciones que el
  normalizador ubica en el mismo lugar cuentan como un solo edificio. `Latitude`/`Longitude`
  son las del edificio; `lat_punto`/`lng_punto` abren los pedidos de un mismo edificio en
  una espiral de pocos metros, por si se quiere dibujar un punto por pedido sin que se
  superpongan.
- **Sin limpieza de pedidos:** no hay pruebas ni pedidos inventados en esta base; se
  publica todo tal cual.

## Cómo se arma (scripts en la carpeta local del TP, no publicados)

1. `node scripts/extraer_pedidos.cjs <salida.json>` — lee la base en una transacción de
   solo lectura. El JSON tiene calles y alturas: se guarda **fuera** de la carpeta del TP.
2. `python3 scripts/armar_datos.py <salida.json>` — escribe `pedidos.csv`,
   `renglones.csv` y `carta.csv`.
3. `python3 scripts/construir_mapa.py <salida.json>` — geocodifica y escribe los archivos
   4 y 5 (guarda las respuestas de la USIG al lado del JSON).
4. `python3 scripts/construir_graficos.py` — escribe los archivos 1, 2, 3 y 6.

## Chequeos que pasa
Aritmética de cada renglón ✓ · subtotales = suma de renglones ✓ · totales ✓ · 359
pedidos = 352 + 7 ✓ · los 245 deliveries entregados tienen dirección y quedaron
geocodificados ✓ · los 124 pedidos de la versión de septiembre dan idénticos ✓

## Privacidad
No se exportó ningún dato personal: ni nombres, ni teléfonos, ni correos, ni
departamentos. Los clientes son códigos estables. Los datasets de los mapas tienen
coordenadas de edificio sin el número de pedido ni la dirección, así que no se pueden
cruzar con lo que se pidió (Ley 25.326). Extracción con consultas dentro de transacciones
de solo lectura sobre la base de producción.
