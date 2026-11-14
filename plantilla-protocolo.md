# Plantilla de protocolo — Experimento de ubicación de producto

**Equipo:** Business Intelligence · **Integrantes:** Orlin Emanuel Encalada Oña · **Fecha:** 13/11/2026

Completa cada sección. Usa commits por sección; la historia del repositorio se revisa.

## 1. Pregunta e hipótesis

¿Ubicar el producto estrella, el más vendido, de RetailAndina aumenta la venta semanal de dicho producto en comparación a mantenerlo en su lugar habitual?

H0: la venta promedio semanal del producto estrella ubicado en el pasillo principal es menor o igual que mantenerlo en su lugar habitual.

H1: la venta promedio semanal del producto estrella ubicado en el pasillo principal es mayor que mantenerlo en su lugar habitual.

## 2. Unidad experimental

*¿Qué recibe el tratamiento? ¿Cuántas unidades se incluyen?*

- Unidad: tienda - semana
- Número de unidades y justificación: 10 tiendas durante 4 semanas. Un total de 40.

Se eligen 10 para el experimento porque cambiar la ubicación en todas obligaría a mover inventario en toda la cadena antes de saber si el cambio produce un efecto. La muestra limita el esfuerzo operativo y permite comparar ambas ubicaciones dentro de cada tienda.

## 3. Factor, niveles y tratamiento

- **Factor** (variable manipulada): ubicación del producto estrella.
- **Niveles** (valores del factor): ubicación habitual, pasillo principal.
- **Tratamiento** (combinación asignada): asignar aleatoriamente la ubicación inicial del producto en cada tienda y alternar cada semana.

## 4. Grupo de control

*¿Cuál es la referencia sin tratamiento y qué rol cumple?*

Cada tienda - semana funciona como su propio grupo de control al permitir comprar el promedio de venta semanal del producto estrella mientras está ubicado en su lugar habitual frente al estar en el pasillo principal. Esto permite comparar las ventas entre una ubicación y otra independientemente de las diferencias que existan entre las tiendas; tamaño, clientela, etc.

## 5. Aleatorización

*¿Cómo se asignan las unidades a los grupos? ¿Por qué es necesaria?*

- **Línea de código de la asignación (Python o R):** escribe el comando que asigna al azar tus N unidades a los dos grupos (tratamiento y control) y ejecútalo con semilla. Incluye el resultado de una corrida y explica por qué el sorteo reparte los factores desconocidos entre los grupos.


```python
# Tiendas
tiendas=["Quito Norte", "Quito Sur", "Guayaquil Centro", "Guayaquil Norte", "Cuenca Centro", "Cuenca Sur", "Ambato Centro", "Manta", "Loja", "Machala"]

# Semanas
semanas = [1,2,3,4]

# Sorteo de la condicion inicial (aleatorización reproducible)
rng = np.random.default_rng(1234597)
primera = rng.choice(["pasillo_principal", "habitual"], size=len(tiendas))

# Tienda - Semana
filas = []
for tienda, cond in zip(tiendas, primera):
    otra = "habitual" if cond=="pasillo_principal" else "pasillo_principal"
    for s in semanas:
      filas.append({"tienda": tienda, "semana": s, "ubicacion":cond if s%2 == 1 else otra})
diseno = pd.DataFrame(filas)
```

Siendo que después de la ejecución se va a obtener la siguiente asignación de tiendas con la ubicación del producto inicializada de manera aleatoria y alternada con el paso de cada semana.

```python
              tienda  semana          ubicacion
0        Quito Norte       1           habitual
1        Quito Norte       2  pasillo_principal
2        Quito Norte       3           habitual
3        Quito Norte       4  pasillo_principal
4          Quito Sur       1  pasillo_principal
5          Quito Sur       2           habitual
6          Quito Sur       3  pasillo_principal
7          Quito Sur       4           habitual
8   Guayaquil Centro       1           habitual
9   Guayaquil Centro       2  pasillo_principal
10  Guayaquil Centro       3           habitual
11  Guayaquil Centro       4  pasillo_principal
12   Guayaquil Norte       1  pasillo_principal
13   Guayaquil Norte       2           habitual
14   Guayaquil Norte       3  pasillo_principal
15   Guayaquil Norte       4           habitual
16     Cuenca Centro       1  pasillo_principal
17     Cuenca Centro       2           habitual
18     Cuenca Centro       3  pasillo_principal
19     Cuenca Centro       4           habitual
20        Cuenca Sur       1           habitual
21        Cuenca Sur       2  pasillo_principal
22        Cuenca Sur       3           habitual
23        Cuenca Sur       4  pasillo_principal
24     Ambato Centro       1           habitual
25     Ambato Centro       2  pasillo_principal
26     Ambato Centro       3           habitual
27     Ambato Centro       4  pasillo_principal
28             Manta       1           habitual
29             Manta       2  pasillo_principal
30             Manta       3           habitual
31             Manta       4  pasillo_principal
32              Loja       1           habitual
33              Loja       2  pasillo_principal
34              Loja       3           habitual
35              Loja       4  pasillo_principal
36           Machala       1  pasillo_principal
37           Machala       2           habitual
38           Machala       3  pasillo_principal
39           Machala       4           habitual
```


## 6. Respuesta y medición

- Variable respuesta: ventas producto por tienda - semana medido en unidades monetarias.
- Período de medición: cuatro semanas consecutivas.
- Quién registra y cómo (¿ciego?): el responsable de caja podría ser el encargado de, tras terminar cada semana, recolectar los datos e indicar condición A o B. No tiene porque conocer el experimento. Sin embargo, los encargados de bodega o acomodadores de los productos si o sí sabrían la condición durante la semana en curso pues serían los encargados de acomodar los productos.

## 7. Factores a controlar (mínimo 4)

| Factor a controlar | Cómo se controla o registra |
|---|---|
| 1. Promociones | Evitar incluir al producto en promociones o registrar la tienda-semana en donde estuvo activa la promoción|
| 2. Precio | Mantenerlo constante o registrar alternaciones en la tienda-semana |
| 3. Festividades y eventos masivos | Verificar próximas festividades, feriados o eventos que podrían afectar la venta del producto e indicar en que semana suceden|
| 4. Visualización del producto | Constante durante el tiempo que dura el experimento en todas las tiendas|

## 8. Limitaciones del diseño

*¿Qué podría invalidar el experimento si no se controla? ¿Qué no se podrá concluir?*

El experimento podría qudar invalidado si, durante el tiempo que sucede no se mantiene el stock suficiente del producto, se activan promociones de tiempo limitado, se promociona un producto que resulta ser la competencia directa del cual se está experimentando, etc. Además, la cantidad de tiendas tomadas para el experimento podrían limitar la precisión y posibilidad de generalizar el resultado hacia toda la cadena. De forma que no se podría ser concluyente en cuanto a la generalización de la hipótesis se refiere.

## 9. Comparación en código (Python/R)

*Escribe `scripts/comparacion_experimento.py` (o `.R`): debe cargar `plantillas/registro_experimento.csv` y calcular el promedio de `ventas_producto` por `ubicacion`. Incluye la salida y responde:*

- ¿Qué grupo muestra mayor venta promedio en la plantilla?
- ¿Por qué con 4 filas no se puede concluir nada sobre el efecto de la ubicación?
- Si el experimento se ejecutara con tu protocolo, ¿qué le faltaría a este análisis para ser justo?
