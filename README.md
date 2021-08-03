# Contexto

Con cierta independencia de la industria en la que nos encontremos, es común dentro de la gestión comercial, o de atención a clientes, tener equipos de personas (ejecutivos) responsables de estar pendientes de las necesidades de los clientes en busca de nuevas oportunidades de cross y/o up selling. Para este propósito, los ejecutivos tienen una cartera de clientes asignada, la cual puede o no estar *"bien"* balanceada, en el sentido de que todos los ejecutivos tienen igual cantidad de clientes en su cartera.

La importancia de tener carteras balanceadas radica en entregarles a todas las personas igualdad de oportunidades para cross o up selling, ya que estas usualmente se encuentran asociadas a comisiones por venta. Si un ejecutivo tiene 10 clientes, y otro tiene 100, podemos suponer que no se encuentran en igualdad de condiciones para realizar su trabajo y, por lo tanto, tampoco las mismas oportunidades de generar ingresos por ventas. Sin embargo, que todos los ejecutivos tengan la misma cantidad de clientes en su cartera tampoco asegura igualdad de oportunidades de venta, por lo que es necesario considerar otros factores, donde uno de estos podría ser la facturación que acumula la cartera de cada ejecutivo. Si dos ejecutivos tienen 100 clientes cada uno en su cartera, pero lo que facturan los clientes de una asciende a $1 millón, mientras que la de otro a $10 millones, entonces parece que las carteras siguen sin estar balanceadas, suponiendo que lo que pagan los clientes es, en cierta medida, un indicador de intensidad de necesidades y/o disposición a pagar.

Estando de acuerdo con lo anterior, ahora el problema es **¿cómo asignamos los clientes a la cartera de cada ejecutivo, cuidando a la vez que todos tengan igual cantidad de clientes y facturación?** Si nos enfrentamos a una arbitrariamente pequeña cantidad de clientes y ejecutivos donde distribuirlos, quizás se puedan balancear las carteras manualmente sin mucho esfuerzo, pero en la medida que crecen la cantidad de clientes y ejecutivos esta tarea se vuelve virtualmente imposible de realizar a *"prueba y error"*.

Luego, lo que propone este proyecto es un modelo de optimización en base a programación lineal binaria que busca la mejor asignación de clientes, de tal forma que todos los ejecutivos tengan igual cantidad de clientes y facturación en sus carteras. Aunque cabe destacar que lo que se presenta como *"facturación"* puede ser, en realidad, sustituido por cualquier medida que se encuentre apropiada.

# Modelo de Optimización Homogénea

Este es el caso más *"simple"*, y es donde tenemos una lista de de *n* clientes que queremos asignar sobre *m* ejecutivos, bajo el supuesto de que ningún ejecutivo tiene clientes asignados previamente. Es decir, los *n* clientes serán asignados a los *m* ejecutivos con independencia de su cartera actual. A modo de ejemplo, si tenemos 7.500 clientes, que en total suman $6.000 millones, y los queremos asignar entre 5 ejecutivos, entonces el resultado sería como se muestra en la siguiente tabla:

<table align="center">
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

Sea <img src="https://render.githubusercontent.com/render/math?math=n&mode=inline"> la cantidad de clientes que queremos asignar entre <img src="https://render.githubusercontent.com/render/math?math=m&mode=inline"> ejecutivos:

<b><u>Variables de decisión</u></b>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;X_{ij}\left\{\begin{matrix}1&space;&&space;\text{Se&space;asigna&space;el&space;ejecutivo&space;i&space;al&space;cliente&space;j}\\0&space;&&space;\text{e.o.c}\end{matrix}\right.&space;" title="\bg_white X_{ij}\left\{\begin{matrix}1 & \text{Se asigna el ejecutivo i al cliente j}\\0 & \text{e.o.c}\end{matrix}\right. "/>
</p>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;i&space;\in&space;[1,m],\text{&space;}&space;j&space;\in&space;[1,n]&space;" title="\bg_white i \in [1,m],\text{ } j \in [1,n] " />
</p>

<b><u>Parámetros necesarios</u></b>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;f_{j}=\text{Facturaci\'on&space;del&space;cliente&space;j}&space;" title="\bg_white f_{j}=\text{Facturaci\'on del cliente j} " />
</p>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;F&space;=&space;\sum_{j}^{n}&space;f_{j}&space;=&space;\text{Facturaci\'on&space;total&space;de&space;los&space;clientes}&space;" title="\bg_white F = \sum_{j}^{n} f_{j} = \text{Facturaci\'on total de los clientes} " />
</p>
    
<b><u>Parámetros opcionales</u></b>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\begin{matrix}hc=\text{Holgura&space;de&space;clientes,&space;}0\leq&space;hc\leq&space;n&space;\\hf=\text{Holgura&space;de&space;facturaci\'on,&space;}0\leq&space;hf\leq&space;F\end{matrix}" title="\bg_white \begin{matrix}hc=\text{Holgura de clientes, }0\leq hc\leq n \\hf=\text{Holgura de facturaci\'on, }0\leq hf\leq F\end{matrix}" />
</p>

<b><u>Función objetivo</u></b>:

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;min&space;\sum_{i}^{m}&space;\sum_{j}^{n}&space;X_{ij}&space;f_{j}" title="\bg_white min \sum_{i}^{m} \sum_{j}^{n} X_{ij} f_{j}" />
</p>

<b><u>Sujeto a</u></b>:

1. Cantidad de clientes por ejecutivo

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\sum_{j}^{n}&space;X_{ij}&space;\geq&space;\left\lfloor&space;\frac{n}{m}&space;\right\rfloor&space;-&space;hc,\text{&space;}&space;\forall\text{&space;}&space;i" title="\bg_white \sum_{j}^{n} X_{ij} \geq \left\lfloor \frac{n}{m} \right\rfloor - hc,\text{ } \forall\text{ } i" />
</p>

2. Facturación total por ejecutivo

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\sum_{j}^{n}&space;X_{ij}f_{j}&space;\geq&space;\left\lfloor&space;\frac{F}{m}&space;\right\rfloor&space;-&space;hf,\text{&space;}&space;\forall\text{&space;}&space;i" title="\bg_white \sum_{j}^{n} X_{ij}f_{j} \geq \left\lfloor \frac{F}{m} \right\rfloor - hf,\text{ } \forall\text{ } i" />
</p>

3. Asignar cada cliente a un solo ejecutivo

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\sum_{i}^{m}&space;X_{ij}&space;\leq&space;1,\text{&space;}&space;\forall\text{&space;}&space;j" title="\bg_white \sum_{i}^{m} X_{ij} \leq 1,\text{ } \forall\text{ } j" />
</p>

4. Variables de decisión binarias

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;X_{ij}&space;\in&space;[0,1],\text{&space;}&space;\forall\text{&space;}&space;i,j" title="\bg_white X_{ij} \in [0,1],\text{ } \forall\text{ } i,j" />
</p>

<b><u>Consideraciones</u></b>

1. Si la cantidad de clientes <img src="https://render.githubusercontent.com/render/math?math=n&mode=inline"> no es múltiplo de la cantidad de ejecutivos <img src="https://render.githubusercontent.com/render/math?math=m&mode=inline">, entonces para las restricciones de clientes por ejecutivo *(1)* se toma el entero inferior de este cociente. Por lo tanto, puede que algunos clientes no sean asignados.

2. Si la facturación total de los clientes a asignar <img src="https://render.githubusercontent.com/render/math?math=F&mode=inline"> no es múltiplo de la cantidad de ejecutivos <img src="https://render.githubusercontent.com/render/math?math=m&mode=inline">, entonces también puede que algunos clientes queden sin asignar, ya que se tomará como parámetro de las restricciones de facturación total por ejecutivo *(2)* el entero inferior de este cociente.

3. Las <img src="https://render.githubusercontent.com/render/math?math=n&mode=inline"> restricciones que resultan de *(3)* permiten que algunos clientes queden sin asignar. Esto permite que el modelo se pueda ejecutar más rápido cuando la solución óptima es muy ajustada.

4. Los parámetros opcionales de holgura <img src="https://render.githubusercontent.com/render/math?math=hc&mode=inline"> y <img src="https://render.githubusercontent.com/render/math?math=\text{hf}&mode=inline"> son para poder relajar las restricciones *(1)* y *(2)*, permitiendo al modelo no asignar todos los clientes para mejorar su tiempo de ejecución. El valor de los parámetros se aplica sobre cada una de las restricciones, y es el mismo que se defina para todas. Esto quiere decir que, por ejemplo, el modelo no permite quitar arbitrariamente distinta cantidad de clientes a cada ejecutivo. Si se define <img src="https://render.githubusercontent.com/render/math?math=hc\geq0&mode=inline">, se aplica el mismo valor a las <img src="https://render.githubusercontent.com/render/math?math=m&mode=inline"> restricciones que resultan de *(1)*. Exactamente lo mismo aplica para el parámetro <img src="https://render.githubusercontent.com/render/math?math=\text{hf}&mode=inline">. Además, que se permita no asignar todos los clientes no implica necesesariamente que no todos sean asignados.

# Modelo de Optimización Heterogénea

Es una generalización del modelo homogéneo, donde se considera que los ejecutivos podrían tener una cartera previamente asignada, y se requiere asignar un pull de nuevos clientes a los ejecutivos, de tal manera que las carteras resultantes queden balanceadas.

Sea <img src="https://render.githubusercontent.com/render/math?math=c_{i}&mode=inline"> la cantidad de clientes que cada uno de los <img src="https://render.githubusercontent.com/render/math?math=m&mode=inline"> ejecutivos tenía previamente, <img src="https://render.githubusercontent.com/render/math?math=r_{i}&mode=inline"> la facturación total de la cartera de cada ejecutivo previamente y <img src="https://render.githubusercontent.com/render/math?math=n&mode=inline"> la cantidad de clientes nuevos que se quiere asignar.

<b><u>Variables de decisión</u></b>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;X_{ij}\left\{\begin{matrix}1&space;&&space;\text{Se&space;asigna&space;el&space;ejecutivo&space;i&space;al&space;cliente&space;j}\\0&space;&&space;\text{e.o.c}\end{matrix}\right.&space;" title="\bg_white X_{ij}\left\{\begin{matrix}1 & \text{Se asigna el ejecutivo i al cliente j}\\0 & \text{e.o.c}\end{matrix}\right. "/>
</p>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;i&space;\in&space;[1,m],\text{&space;}&space;j&space;\in&space;[1,n]&space;" title="\bg_white i \in [1,m],\text{ } j \in [1,n] " />
</p>

<b><u>Parámetros necesarios</u></b>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;f_{j}=\text{Facturaci\'on&space;del&space;cliente&space;j}&space;" title="\bg_white f_{j}=\text{Facturaci\'on del cliente j} " />
</p>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;F&space;=&space;\sum_{j}^{n}&space;f_{j}&space;=&space;\text{Facturaci\'on&space;total&space;de&space;los&space;clientes}&space;" title="\bg_white F = \sum_{j}^{n} f_{j} = \text{Facturaci\'on total de los clientes} " />
</p>

<b><u>Parámetros opcionales</u></b>

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\begin{matrix}hc=\text{Holgura&space;de&space;clientes,&space;}0\leq&space;hc\leq&space;n&space;\\hf=\text{Holgura&space;de&space;facturaci\'on,&space;}0\leq&space;hf\leq&space;F\end{matrix}" title="\bg_white \begin{matrix}hc=\text{Holgura de clientes, }0\leq hc\leq n \\hf=\text{Holgura de facturaci\'on, }0\leq hf\leq F\end{matrix}" />
</p>

<b><u>Función objetivo</u></b>:

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;min&space;\sum_{i}^{m}&space;\sum_{j}^{n}&space;X_{ij}&space;f_{j}" title="\bg_white min \sum_{i}^{m} \sum_{j}^{n} X_{ij} f_{j}" />
</p>

<b><u>Sujeto a</u></b>:

1. Cantidad de clientes por ejecutivo

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\sum_{j}^{n}&space;X_{ij}&space;\geq&space;\left\lfloor&space;\frac{\sum_{i}^{m}&space;c_{i}&plus;n}{m}&space;\right\rfloor&space;-&space;c_{i}&space;-&space;hc,\text{&space;}&space;\forall\text{&space;}&space;i" title="\bg_white \sum_{j}^{n} X_{ij} \geq \left\lfloor \frac{\sum_{i}^{m} c_{i}+n}{m} \right\rfloor - c_{i} - hc,\text{ } \forall\text{ } i" />
</p>

2. Facturación total por ejecutivo

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\sum_{j}^{n}&space;X_{ij}f_{j}&space;\geq&space;\left\lfloor&space;\frac{\sum_{i}^{m}&space;r_{i}&plus;n}{m}&space;\right\rfloor&space;-&space;r_{i}&space;-&space;hf,\text{&space;}&space;\forall\text{&space;}&space;i" title="\bg_white \sum_{j}^{n} X_{ij}f_{j} \geq \left\lfloor \frac{\sum_{i}^{m} r_{i}+n}{m} \right\rfloor - r_{i} - hf,\text{ } \forall\text{ } i" />
</p>

3. Asignar cada cliente a un solo ejecutivo

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;\sum_{i}^{m}&space;X_{ij}&space;\leq&space;1,\text{&space;}&space;\forall\text{&space;}&space;j" title="\bg_white \sum_{i}^{m} X_{ij} \leq 1,\text{ } \forall\text{ } j" />
</p>

4. Variables de decisión binarias

<p align="center">
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\bg_white&space;X_{ij}&space;\in&space;[0,1],\text{&space;}&space;\forall\text{&space;}&space;i,j" title="\bg_white X_{ij} \in [0,1],\text{ } \forall\text{ } i,j" />
</p>
