# Buscaminas con Algoritmo Genético

**Trabajo Práctico N.º 2 — Inteligencia Artificial / Sistemas Inteligentes**

Resolución de estados parciales del Buscaminas mediante un Algoritmo Genético 

Ingeniería en Sistemas de Información — UTN Facultad Regional Buenos Aires  
**2.º cuatrimestre de 2026 | Grupo 24 | K5671**

## Descripción

Este proyecto implementa un **Algoritmo Genético (AG)** para resolver estados parciales del juego **Buscaminas**. A partir de las pistas numéricas visibles y de la cantidad total de minas, busca una distribución de minas en las casillas ocultas que respete las restricciones del tablero.

El problema es combinatorio: cada casilla oculta puede contener una mina o estar libre, y una misma casilla puede afectar a varias pistas. Se utiliza un AG para explorar posibles distribuciones sin enumerarlas exhaustivamente.

> **Importante:** una distribución que satisface todas las pistas es una solución válida, pero no necesariamente coincide con la distribución original. Puede haber varias soluciones compatibles con la información visible.

## Algoritmo implementado

- **Cromosoma:** vector binario, con un gen por casilla oculta (`1` = mina, `0` = libre).
- **Población inicial:** individuos generados aleatoriamente.
- **Selección:** torneo de tamaño 3.
- **Cruce:** de un punto, con probabilidad 0,8.
- **Mutación:** inversión independiente de cada gen, con probabilidad configurable.
- **Elitismo:** se conserva el mejor individuo entre generaciones.
- **Terminación:** penalización 0 o máximo de generaciones.

La función de penalización suma las diferencias absolutas entre las minas propuestas alrededor de cada pista y su valor, más la diferencia absoluta respecto del total conocido de minas:

$$
P(c) = \sum_{p \in \text{pistas}} |\text{valor}(p)-\text{minas vecinas}(p,c)| + |\text{minas propuestas}(c)-\text{total de minas}|
$$

La aptitud se calcula como:

$$
\operatorname{aptitud}(c)=\frac{1}{1+P(c)}
$$

Una solución con **penalización 0** tiene **aptitud 1** y satisface las restricciones modeladas.

## Experimentos y resultados

Se evaluaron dos tableros:

| Característica | Tablero pequeño | Tablero grande |
|---|---:|---:|
| Dimensiones | 6 × 6 | 16 × 16 |
| Minas | 8 | 40 |
| Casillas desconocidas | 20 | 127 |
| Corridas principales | 5 | 5 |
| Corridas con penalización 0 | 5 | 1 |

### Tablero 6 × 6

Las cinco corridas principales alcanzaron **penalización 0**. Además, se estudiaron cinco probabilidades de mutación con **30 semillas cada una** (150 ejecuciones): todas encontraron una solución válida, aunque la mutación de `0,20` requirió, en promedio, más generaciones.

Una enumeración exhaustiva encontró **16 distribuciones válidas** para este tablero. Por lo tanto, cumplir las pistas no implica reconstruir de manera única la ubicación original de las minas.

### Tablero 16 × 16

Se realizaron cinco corridas variando población y probabilidad de mutación, con un máximo de 1.500 generaciones. **Una corrida** alcanzó penalización 0 en la generación **471**; las otras finalizaron con penalizaciones **1, 3, 5 y 1**. Estos valores son penalizaciones de las restricciones, no necesariamente cantidades de casillas mal clasificadas.

Los resultados muestran que el AG puede encontrar distribuciones válidas en instancias más grandes, aunque **no garantiza convergencia** en todas las corridas.

## Cómo ejecutar

1. Abrir [`notebook/Buscaminas.ipynb`](notebook/Buscaminas.ipynb) en **Google Colab**.
2. En Colab, seleccionar **Entorno de ejecución → Ejecutar todas** (o reiniciar la sesión y ejecutar todas las celdas).
3. Revisar las tablas, los tableros propuestos y los gráficos de evolución.
4. Consultar los archivos exportados en `resultados/csv/` y `resultados/graficos/`.

**Tecnologías:** Python, `random`, `pandas` y `matplotlib`. La ejecución está pensada para Google Colab.

## Estructura del repositorio

```text
tp2-buscaminas-algoritmo-genetico/
├── README.md
├── notebook/
│   └── Buscaminas.ipynb
├── resultados/
│   ├── csv/
│   │   ├── resultados_corridas.csv
│   │   ├── historiales_corridas.csv
│   │   ├── historial_aptitud.csv
│   │   ├── resultados_corridas_grande.csv
│   │   ├── historiales_corridas_grande.csv
│   │   └── resumen_mutacion_30_semillas.csv
│   └── graficos/
│       ├── corrida_1_aptitud.png ... corrida_5_aptitud.png
│       └── grande_corrida_1.png ... grande_corrida_5.png
└── informe/
    └── TP2_Grupo24_Informe_Final.pdf
```

Los archivos CSV registran resultados e historiales de las corridas; las imágenes muestran la evolución del algoritmo. El informe desarrolla la metodología, el análisis comparativo y las conclusiones.

> La estructura anterior es la organización propuesta para el repositorio. Si se suben los archivos en otras rutas, actualizar los enlaces y nombres de esta sección.

## Limitaciones

- El AG encuentra configuraciones **compatibles con las pistas**, no necesariamente la distribución real de minas.
- El tablero pequeño puede resolverse mediante enumeración exhaustiva; se incluye para facilitar la verificación.
- En el tablero grande, una única corrida por configuración no permite establecer qué parámetros son estadísticamente superiores.
- Existen otros enfoques, como backtracking y solucionadores de restricciones, que no se implementaron para una comparación directa.

## Integrantes — Grupo 24

- Roberto Rodas
- Joaquín Bergara
- Luciano Zunino
- Nataly Rohling Insua
- Lucas Augusto Martin
- Santiago Perez
- Camila Lazzati
- Valentín Pino Suriano

## Referencias

El desarrollo teórico, la metodología, los resultados y la bibliografía se detallan en el informe final del trabajo práctico.
