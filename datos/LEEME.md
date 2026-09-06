# Dataset — TP Visualización de la Información
**La Percanta** · Puerto Madero, CABA · del 1 de agosto al 5 de septiembre de 2026
Pedidos tomados por la **aplicación web** del local.

## Universo
La base tiene **158 pedidos**: 143 entregados y 15 cancelados.
**Los cuatro gráficos del sitio usan solo los 143 entregados** — un pedido cancelado no
es trabajo hecho ni plata cobrada. El de Tableau usa 136 de esos 143 (ver limpieza).

## Archivos

| Archivo | Filas | Qué es |
|---|---|---|
| `pedidos.csv` | 158 | Un pedido por fila. 26 columnas |
| `renglones.csv` | 326 | Un producto pedido por fila. Se une por `pedido` |
| `carta.csv` | 357 | La carta completa del local. **100% real** |

Los CSV numerados (`1-datawrapper…`, `2-rawgraphs…`, `3-flourish…`, `4-tableau…`) son
agregados ya con la forma que pide cada herramienta, todos calculados sobre los 143
entregados.

## `pedidos.csv` — columnas

**Identificación** — `pedido` · `lote` · `cliente` · `cliente_id`
**Tiempo** — `fecha` (jornada del local: un pedido de las 02:49 cuenta en la noche
anterior) · `hora` · `hora_min` · `franja` (Desayuno 07–11 / Almuerzo 12–15 /
Merienda 16–19 / Cena 20–00 / Madrugada 01–06) · `dia_semana` · `es_finde`
(viernes, sábado y domingo) · `semana`
**Operación** — `tipo_entrega` · `metodo_pago` · `estado`
**Qué se pidió** — `productos_distintos` · `unidades` · `rubro_principal` (el rubro que
más facturó dentro del pedido) · `cant_rubros` · `rubros` · `detalle`
**Plata** — `subtotal` · `descuento` · `propina` · `total`
**Tiempos** — `demora_prometida_min` · `demora_real_min`

## `renglones.csv` — columnas

`pedido` · `lote` · `fecha` · `hora` · `franja` · `dia_semana` · `tipo_entrega` ·
`codigo` · `producto` · `rubro` · `cantidad` · `precio_unitario` · `modificadores` ·
`extra_modificadores` · `total_renglon`

`total_renglon = cantidad × precio_unitario + extra_modificadores`

## Los 17 rubros
Entradas · Tablas · Empanadas · Pizzas artesanales · Calzones · Pastas · Carnes y
pescados · Hamburguesas · Sándwiches · Ensaladas · Desayuno y merienda · Panadería y
dulces · Tortas · Hora dulce · Cafetería · Bebidas · Vinos y cervezas

La carta tiene además una categoría **«Recomendados»** que **no es un rubro sino una
vidriera**: sus diez platos ya pertenecen a un rubro real y se cuentan ahí. Como en la
base tiene `sortOrder = 0`, le ganaba a todos los demás y se llevaba la Milanesa de
Ternera, el Ojo de Bife y el Lomo fuera de Carnes y pescados. Se excluye al elegir el
rubro de cada producto.

## Metodología

La columna **`lote`** distingue dos orígenes:

- **`R` — 58 pedidos.** Extraídos de la base de producción de la app (Supabase /
  PostgreSQL) con consultas de solo lectura. Fechas, horas, importes, productos,
  modificadores y estados copiados tal cual.
- **`S` — 100 pedidos.** Generados sobre la **carta real** (productos, precios y grupos
  de modificadores idénticos a los del local), calibrados contra el lote R.

**Por qué se amplió la muestra.** De los 58 pedidos reales, **41 se cargaron entre la 1 y
las 4 de la mañana con el local cerrado**: son las pruebas del propio desarrollo de la
aplicación. El volumen genuinamente comercial es todavía chico, y no alcanza para leer
patrones de consumo.

## Criterios de limpieza

- **Demoras menores a 25′ descartadas.** Siete pedidos entregados del lote R registran
  demoras de entre 1 y 18 minutos porque los estados se avanzaron a mano durante las
  pruebas. No miden servicio real: quedan en blanco. Por eso el gráfico de Tableau usa
  136 pedidos y no 143.
- **`fecha` es la jornada del local**, no la fecha de reloj.
- **`canal` y `barrio` no se incluyen.** Todos los pedidos entran por la app y todos los
  envíos son a Puerto Madero: serían columnas constantes.

## Privacidad
No se exportó ningún dato personal. Los nombres son seudónimos argentinos asignados de
forma estable por `cliente_id`. No hay teléfonos, direcciones, correos ni identificadores
de la base (Ley 25.326).

## Chequeos que pasa
Aritmética de cada renglón ✓ · subtotales ✓ · totales ✓ · unidades ✓ · demora real ≥ 25′
✓ · todos los productos existen en la carta ✓ · precios del lote S idénticos a la carta ✓
· ningún archivo contiene el rubro «Recomendados» ✓
