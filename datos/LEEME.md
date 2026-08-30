# Dataset — TP Visualización de la Información
**La Percanta** · Puerto Madero, CABA · jornadas del 01 al 29 de agosto de 2026
Pedidos tomados **por la aplicación web** del local.

## Archivos

| Archivo | Filas | Qué es |
|---|---|---|
| `pedidos.csv` | 143 | Un pedido por fila. 26 columnas |
| `renglones.csv` | 284 | Un producto pedido por fila. Se une a `pedidos.csv` por `pedido` |
| `carta.csv` | 357 | La carta completa del local. **100% real** |

## `pedidos.csv` — columnas

**Identificación** — `pedido` · `lote` · `cliente` · `cliente_id`
**Tiempo** — `fecha` (jornada del local: un pedido de las 02:49 cuenta en la
noche anterior) · `hora` · `hora_min` · `franja` (Desayuno / Almuerzo /
Merienda / Cena / Madrugada) · `dia_semana` · `es_finde` · `semana`
**Operación** — `tipo_entrega` (Delivery / Retiro en local) · `metodo_pago`
(Efectivo / Mercado Pago) · `estado` (Entregado / Cancelado)
**Qué se pidió** — `productos_distintos` (cuántos productos distintos tiene el
pedido) · `unidades` (suma de cantidades) · `rubro_principal` (el rubro que se
llevó más plata del pedido) · `cant_rubros` · `rubros` (todos los rubros
tocados) · `detalle` (`2× Empanada de Carne Suave · 1× Pizza Mozzarella`)
**Plata** — `subtotal` · `descuento` · `propina` · `total`
**Tiempos** — `demora_prometida_min` · `demora_real_min`

## `renglones.csv` — columnas

`pedido` · `lote` · `fecha` · `hora` · `franja` · `dia_semana` ·
`tipo_entrega` · `codigo` · `producto` · `categoria` · `cantidad` ·
`precio_unitario` · `modificadores` (guarnición, tamaño, corte…) ·
`extra_modificadores` · `total_renglon`

`total_renglon = cantidad × precio_unitario + extra_modificadores`

## Los 18 rubros de la carta
Recomendados · Entradas · Tablas · Empanadas · Pizzas artesanales · Calzones ·
Pastas · Carnes y pescados · Hamburguesas · Sándwiches · Ensaladas · Desayuno y
merienda · Panadería y dulces · Tortas · Hora dulce · Cafetería · Bebidas ·
Vinos y cervezas

## Metodología

La columna **`lote`** distingue dos orígenes:

- **`R` — 43 pedidos, 122 renglones.** Extraídos de la base de producción de la
  app (Supabase / PostgreSQL) con consultas de solo lectura. Fechas, horas,
  importes, productos, modificadores y estados copiados tal cual.
- **`S` — 100 pedidos, 162 renglones.** Generados sobre la **carta real**
  (productos, precios y grupos de modificadores idénticos a los del local),
  calibrados contra el lote R.

**Por qué se amplió la muestra.** Los 43 pedidos reales corresponden al período
de prueba de la aplicación: 28 de los 43 se cargaron entre la 1 y las 4 de la
mañana, con el local cerrado (abre 08:00, cierra 00:00 y 01:00 los viernes y
sábados). Veinte días de operación en pruebas no alcanzan para leer patrones de
consumo, así que se completó la muestra manteniendo la estructura del local.

**Calibración** — el lote S reproduce el comportamiento del lote R:

| | lote R (real) | lote S |
|---|---|---|
| Ticket promedio | $38.700 | ~$41.000 |
| Productos distintos por pedido | 1,93 | ~2,0 |
| Unidades por pedido | 3,77 | ~3,2 |
| Delivery / Retiro | 70 / 30 | 70 / 30 |
| Cancelados | 9% | 7% |

## Criterios de limpieza

- **Demoras menores a 25′ descartadas.** Nueve pedidos del lote R registran
  demoras de 1, 6, 7, 12, 16 y 18 minutos porque los estados se avanzaron a mano
  durante las pruebas. No miden servicio real: quedan en blanco.
- **`canal` y `barrio` eliminados.** Todos los pedidos entran por la app y todos
  los envíos son a Puerto Madero: columnas constantes, sin información.
- **`fecha` es la jornada del local**, no la fecha de reloj.

## Privacidad
No se exportó ningún dato personal. Los nombres son seudónimos argentinos
asignados de forma estable por `cliente_id`. No hay teléfonos, direcciones,
correos ni identificadores de la base (Ley 25.326).

## Chequeos que pasa
Aritmética de cada renglón ✓ · subtotales ✓ · totales ✓ · unidades ✓ ·
demora real ≥ 25′ ✓ · todos los productos existen en la carta ✓ · precios del
lote S idénticos a la carta ✓
