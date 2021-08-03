# Contexto

Con cierta independencia de la industria en la que nos encontremos, es común dentro de la gestión comercial, o de atención a clientes, tener equipos de personas (ejecutivos) responsables de estar pendientes de las necesidades de los clientes en busca de nuevas oportunidades de cross y/o up selling. Para este propósito, los ejecutivos tienen una cartera de clientes asignada, la cual puede o no estar *"bien"* balanceada, en el sentido de que todos los ejecutivos tienen igual cantidad de clientes en su cartera.

La importancia de tener carteras balanceadas radica en entregarles a todas las personas igualdad de oportunidades para cross o up selling, ya que estas usualmente se encuentran asociadas a comisiones por venta. Si un ejecutivo tiene 10 clientes, y otro tiene 100, podemos suponer que no se encuentran en igualdad de condiciones para realizar su trabajo y, por lo tanto, tampoco las mismas oportunidades de generar ingresos por ventas. Sin embargo, que todos los ejecutivos tengan la misma cantidad de clientes en su cartera tampoco asegura igualdad de oportunidades de venta, por lo que es necesario considerar otros factores, donde uno de estos podría ser la facturación que acumula la cartera de cada ejecutivo. Si dos ejecutivos tienen 100 clientes cada uno en su cartera, pero lo que facturan los clientes de una asciende a $1 millón, mientras que la de otro a $10 millones, entonces parece que las carteras siguen sin estar balanceadas, suponiendo que lo que pagan los clientes es, en cierta medida, un indicador de intensidad de necesidades y/o disposición a pagar.

Estando de acuerdo con lo anterior, ahora el problema es **¿cómo asignamos los clientes a la cartera de cada ejecutivo, cuidando a la vez que todos tengan igual cantidad de clientes y facturación?** Si nos enfrentamos a una arbitrariamente pequeña cantidad de clientes y ejecutivos donde distribuirlos, quizás se puedan balancear las carteras manualmente sin mucho esfuerzo, pero en la medida que crecen la cantidad de clientes y ejecutivos esta tarea se vuelve virtualmente imposible de realizar a *"prueba y error"*.

Luego, lo que propone este proyecto es un modelo de optimización en base a programación lineal binaria que busca la mejor asignación de clientes, de tal forma que todos los ejecutivos tengan igual cantidad de clientes y facturación en sus carteras. Aunque cabe destacar que lo que se presenta como *"facturación"* puede ser, en realidad, sustituido por cualquier medida que se encuentre apropiada.

# Modelo de Optimización Homogénea

Este es el caso más *"simple"*, y es donde tenemos una lista de de *n* clientes que queremos asignar sobre *m* ejecutivos, bajo el supuesto de que ningún ejecutivo tiene clientes asignados previamente. Es decir, los *n* clientes serán asignados a los *m* ejecutivos con independencia de su cartera actual. A modo de ejemplo, si tenemos 7.500 clientes, que en total suman $6.000 millones, y los queremos asignar entre 5 ejecutivos, entonces el resultado sería como se muestra en la siguiente tabla:

<table>
    <tr>
        <th>Ejecutivo</th>
        <th>Clientes</th>
        <th>Facturación</th>
    </tr>
    <tr>
        <td>Ejecutivo 1</td>
        <td>1.500</td>
        <td>$1.200 millones</td>
    </tr>
    <tr>
        <td>Ejecutivo 2</td>
        <td>1.500</td>
        <td>$1.200 millones</td>
    </tr>
    <tr>
        <td>Ejecutivo 3</td>
        <td>1.500</td>
        <td>$1.200 millones</td>
    </tr>
    <tr>
        <td>Ejecutivo 4</td>
        <td>1.500</td>
        <td>$1.200 millones</td>
    </tr>
    <tr>
        <td>Ejecutivo 5</td>
        <td>1.500</td>
        <td>$1.200 millones</td>
    </tr>
    <tr>
        <th>Total</th>
        <th>7.500</th>
        <th>$6.000 millones</th>
    </tr>
</table>

Luego, en concreto, el modelo propuesto para este caso es el siguiente:

Sea $n$ la cantidad de clientes que queremos asignar entre $m$ ejecutivos:

<b><u>Variables de decisión</u></b>

<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;X_{ij}\left\{\begin{matrix}1&space;&&space;\text{Se&space;asigna&space;el&space;ejecutivo&space;i&space;al&space;cliente&space;j}&space;\\0&space;&&space;\text{e.o.c}&space;\\\end{matrix}\right." title="X_{ij}\left\{\begin{matrix}1 & \text{Se asigna el ejecutivo i al cliente j} \\0 & \text{e.o.c} \\\end{matrix}\right." />

$$i \in [1,m],\: j \in [1,n]$$

<b><u>Parámetros necesarios</u></b>

$$f_{j} = Facturación\: del\: cliente\: j$$

$$F = \sum_{j}^{n} f_{j} = Facturación\: total \: de\: los\: clientes$$

<b><u>Parámetros opcionales</u></b>

$$
\begin{array}{ll}
    hc = Holgura\: de\: clientes,\: 0 \leq hc \lt n\\
    hf = Holgura\: de\: facturación,\: 0 \leq hf \lt F
\end{array}
$$

<b><u>Función objetivo</u></b>:

$$min \sum_{i}^{m} \sum_{j}^{n} X_{ij} f_{j}$$

<b><u>Sujeto a</u></b>:

1. Cantidad de clientes por ejecutivo

$$\sum_{j}^{n} X_{ij} \geq \left\lfloor \frac{n}{m} \right\rfloor - hc,\: \forall\: i$$

2. Facturación total por ejecutivo

$$\sum_{j}^{n} X_{ij}f_{j} \geq \left\lfloor \frac{F}{m} \right\rfloor - hf,\: \forall\: i$$

3. Asignar cada cliente a un solo ejecutivo

$$\sum_{i}^{m} X_{ij} \leq 1,\: \forall\: j$$

4. Variables de decisión binarias

$$X_{ij} \in [0,1],\: \forall\: i,j$$

<b><u>Consideraciones</u></b>

1. Si la cantidad de clientes $n$ no es múltiplo de la cantidad de ejecutivos $m$, entonces para las restricciones de clientes por ejecutivo (1) se toma el entero inferior de este cociente. Por lo tanto, puede que algunos clientes no sean asignados.

2. Si la facturación total de los clientes a asignar $F$ no es múltiplo de la cantidad de ejecutivos $m$, entonces también puede que algunos clientes queden sin asignar, ya que se tomará como parámetro de las restricciones de facturación total por ejecutivo (2) el entero inferior de este cociente.

3. Las $n$ restricciones que resultan de (3) permiten que algunos clientes queden sin asignar. Esto permite que el modelo se pueda ejecutar más rápido cuando la solución óptima es muy ajustada.

4. Los parámetros opcionales de holgura $hc$ y $hf$ son para poder relajar las restricciones (1) y (2), permitiendo al modelo no asignar todos los clientes para mejorar su tiempo de ejecución. El valor de los parámetros se aplica sobre cada una de las restricciones, y es el mismo que se defina para todas. Esto quiere decir que, por ejemplo, el modelo no permite quitar arbitrariamente distinta cantidad de clientes a cada ejecutivo. Si se define $hc \geq 0$, se aplica el mismo valor a las $m$ restricciones que resultan de (1). Exactamente lo mismo aplica para el parámetro $hf$. Además, que se permita no asignar todos los clientes no implica necesesariamente que no todos sean asignados.

# Modelo de Optimización Heterogénea

Es una generalización del modelo homogéneo, donde se considera que los ejecutivos podrían tener una cartera previamente asignada, y se requiere asignar un pull de nuevos clientes a los ejecutivos, de tal manera que las carteras resultantes queden balanceadas.

Sea $c_{i}$ la cantidad de clientes que cada uno de los $m$ ejecutivos tenía previamente, $r_{i}$ la facturación total de la cartera de cada ejecutivo previamente y $n$ la cantidad de clientes nuevos que se quiere asignar.

<b><u>Variables de decisión</u></b>

$$X_{ij} \left\{
    \begin{array}{ll}
        1 \; Se \: asigna\: el\: ejecutivo\: i\: al\: cliente\: j\\
        0 \; e.o.c
    \end{array}
    \right.$$

$$i \in [1,m],\: j \in [1,n]$$

<b><u>Parámetros necesarios</u></b>

$$f_{j} = Facturación\: del\: nuevo\: cliente\: j$$

$$F = \sum_{j}^{n} f_{j} = Facturación\: total \: de\: los\: nuevos\: clientes$$

<b><u>Parámetros opcionales</u></b>

$$
\begin{array}{ll}
    hc = Holgura\: de\: clientes,\: 0 \leq hc \lt n\\
    hf = Holgura\: de\: facturación,\: 0 \leq hf \lt F
\end{array}
$$

<b><u>Función objetivo</u></b>:

$$min \sum_{i}^{m} \sum_{j}^{n} X_{ij} f_{j}$$

<b><u>Sujeto a</u></b>:

1. Cantidad de clientes por ejecutivo

$$\sum_{j}^{n} X_{ij} \geq \left\lfloor \frac{\sum_{i}^{m} c_{i}+n}{m} \right\rfloor - c_{i} - hc,\: \forall\: i$$

2. Facturación total por ejecutivo

$$\sum_{j}^{n} X_{ij}f_{j} \geq \left\lfloor \frac{\sum_{i}^{m} r_{i}+n}{m} \right\rfloor - r_{i} - hf,\: \forall\: i$$

3. Asignar cada cliente a un solo ejecutivo

$$\sum_{i}^{m} X_{ij} \leq 1,\: \forall\: j$$

4. Variables de decisión binarias

$$X_{ij} \in [0,1],\: \forall\: i,j$$
