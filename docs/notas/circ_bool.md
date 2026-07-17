# Circuitos Booleanos

!!! def "Circuitos booleanos"
    Los circuitos booleanos son modelos computacionales que computan funciones booleanas.

!!! def "Def 1 — Circuito"
    Sea $\Phi$ un conjunto de funciones booleanas. Un **circuito de $n$ variables sobre la base $\Phi$** es una secuencia $g_1, \dots, g_t$ de $t \geq n$ funciones booleanas tales que $g_1 = x_1, \dots, g_n = x_n$ y

    $$
    \forall i > n, \quad g_i = \varphi(g_{i_1}, \dots, g_{i_d})
    $$

    donde $\varphi \in \Phi$ e $i_1, \dots, i_d < i$.

    Un circuito en general se representa de forma *"bottom-up"*.

!!! def "Def 2 — Tamaño (size)"
    El **tamaño** de un circuito es el número total $t-n$ de *gates* (no se cuentan las variables input).

!!! def "Def 3 — Profundidad (depth)"
    La **profundidad** de un circuito es el largo del camino más extenso entre una variable input y el nodo output.

!!! def "Def 4 — Computar"
    Decimos que un circuito **computa** una función booleana (o un conjunto de ellas) si esta está entre las $g_i$.

## Circuitos como DAGs

!!! observation "Observación"
    Todo circuito puede ser visto como un DAG con nodos de fan-in $0$ (grado $0$) que corresponden a las variables, y donde cada otro nodo $v$ corresponde a una función $\varphi \in \Phi$. Uno o más nodos son outputs.

!!! example "Ejemplo 1 — Circuito para $Maj_3$"
    Un circuito sobre $B = \{\wedge, \vee, \lnot\}$ que computa

    $$
    Maj_3(x,y,z) = 1 \iff x+y+z \geq 2.
    $$

    ```mermaid
    graph TD
        x --> AND1["∧"]
        y --> AND1
        x --> OR1["∨"]
        y --> OR1
        AND1 --> NOT1["¬"]
        NOT1 --> AND2["∧"]
        OR1 --> AND2
        z --> AND3["∧"]
        AND2 --> AND3
        AND1 --> OR2["∨"]
        AND3 --> OR2
        OR2 --> Z["Z (output)"]
    ```

    $$
    \begin{aligned}
    g_4 &= x \wedge y, & g_5 &= x \vee y,\\
    g_6 &= \lnot g_4, & g_7 &= g_6 \wedge g_5,\\
    g_8 &= z \wedge g_7, & g_9 &= g_4 \vee g_8.
    \end{aligned}
    $$

    Con $n=3$ variables input y $t=9$, el tamaño es $t-n=6$ y la profundidad es $5$ (camino $x \to g_4 \to g_6 \to g_7 \to g_8 \to g_9$).

!!! def "Def 5 — Fórmula"
    Una **fórmula** es un circuito en el cual todos los nodos tienen fan-out (outdegree) a lo más $1$.

!!! observation "Observación"
    El grafo de una fórmula es un **árbol**. La diferencia crucial entre una fórmula y un circuito es que en el circuito un resultado puede ser usado múltiples veces sin necesidad de recomputarlo.

## Circuitos De Morgan

!!! def "Def 6 — Circuito De Morgan"
    Un **circuito De Morgan** es un circuito sobre la base $\{\wedge, \vee\}$ donde los inputs pueden ser las variables y sus negaciones.

!!! observation "Observación"
    Usando las leyes de De Morgan

    $$
    \lnot(x \vee y) = \lnot x \wedge \lnot y, \qquad \lnot(x \wedge y) = \lnot x \vee \lnot y,
    $$

    se puede mostrar que cualquier circuito sobre $\{\wedge, \vee, \lnot\}$ puede ser reducido a la forma De Morgan, a lo más doblando el número de nodos (la profundidad se mantiene).

## Circuitos probabilísticos

!!! def "Def 7 — Circuito probabilístico"
    Un circuito se dice **probabilístico** si, además de tener las variables input estándar $x_1, \dots, x_n$, tiene algunos inputs $r_1, \dots, r_m$ llamados **inputs aleatorios**. Cuando estos inputs aleatorios son escogidos de una distribución uniforme en $\{0,1\}$, el output $C(x)$ del circuito es una variable $0$-$1$ aleatoria.

!!! def "Def 8 — Computar (probabilístico)"
    Un circuito probabilístico $C(x)$ **computa** una función booleana $f(x)$ si

    $$
    \text{Prob}[C(x) = f(x)] \geq 3/4, \qquad \forall x \in \{0,1\}^n.
    $$

!!! observation "Observación"
    Podemos usar $3/4$ o cualquier probabilidad $> 1/2$.

!!! motivation "Pregunta"
    ¿Pueden los circuitos probabilísticos computar una función usando un tamaño mucho menor que un circuito determinístico?

!!! prop "Lema (Majority trick)"
    Si $x_1, \dots, x_m$ son variables aleatorias Bernoulli independientes con probabilidad de éxito $1/2 + \varepsilon$, entonces

    $$
    \text{Prob}[Maj(x_1, \dots, x_m) = 0] \leq e^{-2\varepsilon^2 m}.
    $$

!!! proof "Demostración"
    Sea $\mathcal{F}$ la colección de subconjuntos de $[m]$ de tamaño $> m/2$. Entonces

    $$
    \text{Prob}[Maj(x_1,\dots,x_m)=0] = \sum_{S \in \mathcal{F}} \text{Prob}[x_i = 0,\ \forall i \in S] \cdot \text{Prob}[x_i = 1,\ \forall i \notin S]
    $$

    $$
    = \sum_{S \in \mathcal{F}} (1/2-\varepsilon)^{|S|}(1/2+\varepsilon)^{m-|S|} \leq \sum_{S \in \mathcal{F}} (1/2-\varepsilon)^{m/2}(1/2+\varepsilon)^{m/2} = \sum_{S \in \mathcal{F}} (1/4-\varepsilon^2)^{m/2},
    $$

    usando que $(1/2-\varepsilon) < (1/2+\varepsilon)$ y $|S| > m/2$. Como $|\mathcal{F}| \leq 2^m$ (total de subconjuntos de $[m]$),

    $$
    \leq 2^m \cdot (1/4-\varepsilon^2)^{m/2} = 2^{m/2}\, 2^{m/2}\, (1/4-\varepsilon^2)^{m/2} = (1-4\varepsilon^2)^{m/2} \leq e^{-2\varepsilon^2 m}. \qquad \blacksquare
    $$

## Teorema de Adleman

!!! prop "Teorema (Adleman, 1978)"
    Si una función booleana $f$ de $n$ variables puede ser computada por un circuito probabilístico de tamaño $M$, entonces $f$ puede ser computada por un circuito determinístico de tamaño a lo más $8 \cdot n \cdot M$.

!!! author "Leonard Adleman"
    Leonard Adleman (n. 1945), informático y bioquímico estadounidense —también la "A" de RSA— demostró este resultado de derandomización de circuitos en 1978.

!!! proof "Demostración"
    Sea $C$ un circuito probabilístico que computa $f$, es decir $\text{Prob}[C(x)=f(x)] \geq 3/4$, $\forall x \in \{0,1\}^n$.

    Tomamos $m$ copias independientes de $C$: $C_1, \dots, C_m$. Consideramos el circuito probabilístico $C'$ que computa la función *Majority* de los outputs de los $m$ circuitos $C_1, \dots, C_m$.

    Fijemos $a \in \{0,1\}^n$ y definamos, para cada $i$, la variable indicatriz $x_i := \mathbb{1}[C_i(a) = f(a)]$. Cada $x_i$ satisface $\text{Prob}[x_i=1] = 1/2+\varepsilon$ con $\varepsilon = 1/4$. Por el lema del Majority trick,

    $$
    \text{Prob}[Maj(x_1,\dots,x_m)=0] \leq e^{-2\varepsilon^2 m}.
    $$

    En otras palabras, $C'$ se equivoca en el input $a \in \{0,1\}^n$ con probabilidad a lo más $e^{-2\varepsilon^2 m} = e^{-m/8}$.

    Por *union bound*, la probabilidad de que $C'$ se equivoque en **algún** input de $\{0,1\}^n$ es

    $$
    \text{Prob}\Big[\bigcup_{a \in \{0,1\}^n} B_a\Big] \leq \sum_{a \in \{0,1\}^n} e^{-m/8} = 2^n \cdot e^{-m/8}, \qquad B_a := \{C' \text{ se equivoca en } a\}.
    $$

    Tomando $m = 8n$ se tiene $2^n \cdot e^{-m/8} < 1$, luego

    $$
    \text{Prob}[C' \text{ es correcto } \forall w \in \{0,1\}^n] > 0.
    $$

    Por lo tanto existe un set de inputs aleatorios que hace que $C'$ dé respuestas correctas en todos los inputs de $\{0,1\}^n$; fijando esos inputs, $C'$ se vuelve un circuito **determinístico**, de tamaño a lo más $8nM$. $\blacksquare$

## Tiempo promedio de computación

!!! def "Def 9 — Nodo de parada y tiempo de computación"
    Sea $C = (g_1, \dots, g_s)$ un circuito computando una función booleana $f(x)$, es decir $g_s(x) = f(x)$. Introducimos una variable booleana $Z$ (la *variable output*), que es un resultado parcial en el circuito, $Z = g_i(a)$. El objetivo es interrumpir la secuencia de cómputo $g_1(a), \dots, g_s(a)$ apenas $Z$ tenga el valor correcto $f(a)$.

    Para esto declaramos algunos nodos como **nodos de parada**. Dado $a \in \{0,1\}^n$, una computación $g_1(a), \dots, g_s(a)$ continúa hasta que el primer nodo $g_i$ que es nodo de parada cumple $g_i(a) = 1$; ahí la computación en $a$ termina y el output $C(a)$ es el valor actual de $Z$.

    El **tiempo de computación** $t_C(a)$ de un circuito en el input $a$ es el número $i$ de nodos evaluados hasta que el valor es computado. El **tiempo promedio** de un circuito $C$ es

    $$
    t(C) = 2^{-n} \sum_{a \in \{0,1\}^n} t_C(a).
    $$

!!! example "Ejemplo — Tres circuitos para $OR_4$"
    Tres circuitos que computan $OR_4(x) = x_1 \vee x_2 \vee x_3 \vee x_4$.

    **Circuito $C_1$** (evalúa cada variable en orden, deteniéndose ante el primer $1$):

    $$
    g_1=x_1 \ (\text{stop}),\quad g_2=x_2\ (\text{stop}),\quad g_3=x_3\ (\text{stop}),\quad g_4=x_4\ (\text{stop}),
    $$

    con $Z=1$ si algún nodo de parada da $1$, y $Z=0$ en otro caso.

    **Circuito $C_2$:**

    $$
    g_1 = x_1 \vee x_2 \ (\text{stop}), \qquad g_2 = x_3 \vee x_4.
    $$

    **Circuito $C_3$** (fórmula, sin parada anticipada):

    $$
    g_1 = x_1 \vee x_2, \qquad g_2 = x_3 \vee x_4, \qquad g_3 = g_1 \vee g_2.
    $$

    Sobre $a=(0,1,0,0)$: el circuito $C_1$ toma $t_{C_1}(a)=3$, el $C_2$ toma $t_{C_2}(a)=1$ y el $C_3$ toma $t_{C_3}(a)=3$.

    El tiempo promedio del circuito $C_3$ es siempre $t(C_3)=3$ (nunca hay parada anticipada). En cambio, para $C_2$:

    $$
    t(C_2) = \frac{1}{16}\big(12 \cdot 1 + 4 \cdot 2\big) = \frac{5}{4},
    $$

    ya que $g_1 = x_1 \vee x_2 = 1$ en $12$ de los $16$ inputs (parada en $1$ paso), y en los $4$ restantes se necesita evaluar también $g_2$ (parada en $2$ pasos).

!!! def "Def 10 — Tiempo promedio de una función"
    El **tiempo promedio** de una función booleana $t(f)$ es el tiempo promedio mínimo de un circuito que computa $f$.

!!! observation "Observación"
    Siempre se tiene $t(f) \leq C(f)$, donde $C(f)$ es el tamaño mínimo de un circuito que computa $f$.

!!! author "Chashkin (1997)"
    A. V. Chashkin demostró que existen funciones booleanas $f$ de $n$ variables que requieren $t(f) = \Omega(2^n/n)$.

## Ejemplo: la función umbral $Th_2^n$

!!! example "$C(Th_2^n) \geq n-1$ pero $t(Th_2^n) = O(1)$"
    Toda función booleana que depende de sus $n$ variables requiere al menos $n-1$ gates, luego $C(Th_2^n(x)) \geq n-1$.

    Por otro lado, se puede probar que $t(Th_2^n(x)) = O(1)$. Particionamos las variables en bloques de $3$: $\{x_1,x_2,x_3\}, \{x_4,x_5,x_6\}, \dots$ Cada bloque $Th_2^3$ puede computarse con $6$ gates (en tiempo $6$):

    $$
    Z = Th_2^3(x_1,x_2,x_3), \quad Z = Th_2^3(x_4,x_5,x_6), \quad \dots
    $$

    Primero computamos $Z=Th_2^3(x_1,x_2,x_3)$, luego $Z=Th_2^3(x_4,x_5,x_6)$, y así sucesivamente; cada nodo que resetea $Z$ es un nodo de parada.

    De esta forma, las computaciones sobre $4 \cdot 2^{n-3} = 2^{n-1}$ inputs se detienen en $6$ pasos (el primer bloque ya satisface el umbral). Las computaciones sobre $4^2 \cdot 2^{n-6} = 2^{n-2}$ inputs restantes se detienen en $6 \cdot 2 = 12$ pasos, y en general las computaciones sobre $4^t \cdot 2^{n-3t} = 2^{n-t}$ inputs se detienen después de $6t$ pasos.

    Por lo tanto, el tiempo de computación promedio es a lo más

    $$
    \sum_{t=1}^{n/3} 6t \cdot 2^{-t} = O(1).
    $$
