# Dataset — TP Visualización de la Información
**La Percanta** · Puerto Madero, CABA · del 4 de agosto al 26 de septiembre de 2026
Pedidos tomados por la **aplicación web** del local. **Todos reales.**

## Universo
La base tiene **124 pedidos**: 119 entregados y 5 cancelados.
**Los cuatro gráficos del sitio usan solo los 119 entregados** — un pedido cancelado no
es trabajo hecho ni plata cobrada.

Todos entraron por la web: los pedidos por teléfono y los del salón no pasan por la
aplicación, así que el dataset describe lo que se pide por la app, no todo el restaurante.

## Archivos

| Archivo | Filas | Qué es |
|---|---|---|
| `pedidos.csv` | 124 | Un pedido por fila |
| `renglones.csv` | 304 | Un producto pedido por fila. Se une a `pedidos.csv` por `pedido` |
| `carta.csv` | 357 | La carta completa del local |
| `1-datawrapper-heatmap-dia-hora.csv` | 7 | Pedidos entregados por día de la semana × hora de entrada |
| `2-rawgraphs-alluvial-franja-rubro-entrega.csv` | 23 | Franja → rubro principal → forma de entrega |
| `3-flourish-carta.csv` | 71 | Facturación y unidades por producto vendido |
| `4-tableau-mapa-delivery.csv` | 81 | Un punto por pedido de delivery entregado, a nivel de edificio, con su ticket y rango |

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
  el resto va a «Otros rubros».
- **Treemap:** una pizza mitad y mitad reparte su facturación y su unidad en partes
  iguales entre los dos sabores.
- **Rangos de gasto:** hasta $15.000 · $15.000 a $25.000 · $25.000 a $35.000 ·
  $35.000 a $45.000 · más de $45.000. El CSV trae el rango de cada pedido
  (`rango_ticket`); el tablero de Tableau los aplica al ticket promedio de cada edificio,
  con el tamaño del círculo según la plata total. Ningún pedido cae justo en un límite.
- **Mapa:** calle y altura geocodificadas con el normalizador de la Ciudad (USIG). Juana
  Manso 1965 no existe en el callejero oficial y se ubicó en la última altura válida de
  la calle (1900). `Latitude`/`Longitude` son las del edificio; `lat_punto`/`lng_punto`
  abren los pedidos de un mismo edificio en una espiral de pocos metros, por si se quiere
  dibujar un punto por pedido sin que se superpongan.
- **Sin limpieza de pedidos:** no hay pruebas ni pedidos inventados en esta base; se
  publica todo tal cual.

## Chequeos que pasa
Aritmética de cada renglón ✓ · subtotales = suma de renglones ✓ · totales ✓ · 124
pedidos = 119 + 5 ✓ · los 81 deliveries entregados tienen dirección y quedaron
geocodificados ✓

## Privacidad
No se exportó ningún dato personal: ni nombres, ni teléfonos, ni correos, ni
departamentos. Los clientes son códigos estables. El dataset del mapa tiene coordenadas
de edificio sin el número de pedido ni la dirección, así que no se puede cruzar con lo
que se pidió (Ley 25.326). Extracción con consultas dentro de transacciones de solo
lectura sobre la base de producción.
