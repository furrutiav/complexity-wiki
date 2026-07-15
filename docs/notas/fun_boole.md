# Funciones Booleanas

$$
f : \{0,1\}^n \to \{0,1\}
$$

donde $n$ es la cantidad de bits (*bit sequence*) y usamos la convención $\text{FALSE}=0$, $\text{TRUE}=1$.

**Ejemplos:** $x \cdot y$, $x \vee y$, $\lnot x = 1-x$.

## Motivación

*"Lower bounds"*: ¿cuántas de estas operaciones "simples" necesitamos para computar $f$ para todo input?

## Historia

George Boole (1815–1864), matemático y filósofo, introdujo el concepto de función booleana.

## Conceptos básicos

!!! def "Def 1 — Valores booleanos o bits"
    $0$ y $1$.

!!! def "Def 2 — Función booleana"
    $$
    f(x) = f(x_1, x_2, \dots, x_n)
    $$

    de $n$ variables, $f : \{0,1\}^n \to \{0,1\}$.

    **Nociones:** aceptar / rechazar.

    $$
    f(a) = 1 \implies \text{"acepta"} \ a \in \{0,1\}^n
    $$

    $$
    f(a) = 0 \implies \text{"rechaza"}
    $$

!!! observation "Observación"
    $f$ no necesariamente depende de todas las variables.

!!! def "Def 3 — Depende de la $i$-ésima variable $x_i$"
    Si existen $a_1, \dots, a_{i-1}, a_{i+1}, \dots, a_n \in \{0,1\}$ tales que

    $$
    f(a_1, \dots, a_{i-1}, 0, a_{i+1}, \dots, a_n) \neq f(a_1, \dots, a_{i-1}, 1, a_{i+1}, \dots, a_n).
    $$

!!! example "Ejemplo"
    $f(x_1, x_2) = \lnot x_1$ depende de $x_1$, pero no de $x_2$.

!!! example "Otro ejemplo"
    $\text{FIRST}(x_1, \dots, x_n) = x_1$ solo depende de $x_1$ (Math).

## Conteo de funciones booleanas

**Q:** ¿Cuántas funciones a $n$ variables hay (distintas)?

**R:** $2^{2^n}$, ya que $\left|\{0,1\}^{\{0,1\}^n}\right| = 2^{2^n}$.

!!! example "Ejemplos básicos"
    - **Umbral:** $Th_k^n(x_1, \dots, x_n) = \big[x_1 + x_2 + \dots + x_n \geq k\big]$.
    - **Mayoría:** $Maj_n(x) = \big[x_1 + x_2 + \dots + x_n \geq \lceil n/2 \rceil\big]$.
    - **Paridad:** $\oplus_n(x) = x_1 + x_2 + \dots + x_n \mod 2$.
    - **Función módulo:** $Mod_k^n(x) = \big[x_1 + x_2 + \dots + x_n \equiv_k 0\big]$.

!!! observation "Observación"
    Estos ejemplos no dependen de la posición.

## Funciones simétricas

!!! def "Def 4 — Simétrica"
    $f(x_1, \dots, x_n) = f(x_{\pi(1)}, x_{\pi(2)}, \dots, x_{\pi(n)})$ para toda permutación $\pi \in S_n$.

**Q:** ¿Cuántas funciones simétricas de $n$ variables existen?

**R:** $2^{n+1}$. Hay $n+1$ posibles inputs distintos bajo permutación:

$$
00\dots0,\quad 10\dots0,\quad \dots,\quad 11\dots1
$$

y una función simétrica queda determinada por su valor en cada uno de ellos. Es decir, $f(0\dots010\dots0) = f(10\dots0)$.

## Motivación 2: ¿por qué pensar en funciones booleanas?

Propiedades se pueden codificar con funciones booleanas.

!!! example "Ejemplo — Primalidad"
    $\text{PRIME}(x)$, con $x$ en representación binaria (teoría de números):

    $$
    \text{PRIME}(x) =
    \begin{cases}
    1, & \sum_{i \geq 1} x_i 2^{i-1} \text{ es primo} \\
    0, & \text{en otro caso}
    \end{cases}
    $$

    **Q:** ¿Se podrá computar usando una cantidad $\text{poly}(n)$ de operaciones booleanas?

    **R:** Sí — Agrawal (2004).

!!! example "Ejemplo — Grafos"
    Sea $[n] = \{1, \dots, n\}$ el conjunto de vértices, y $x_{ij} \in \{0,1\}$ indicando la potencial arista $(i,j)$.

    Luego, cualquier vector $0$-$1$ $x$ de largo $\binom{n}{2}$ nos da un potencial grafo $G_x$, donde existe la arista $i-j$ ssi $x_{ij}=1$.

    Con esto, $f(x) = 1$ ssi $G_x$ tiene cierta propiedad.

    - $\text{CLIQUE}(n,k)$ determina si $G_x$ posee un clique de $k$ vértices: $\exists H$ completo de tamaño $k$ tal que $H \leq G_x$ (subgrafo).

    **Q:** ¿Se podrá computar esta función usando $\text{poly}(n)$ operaciones?

    **R:** Abierto.

    !!! observation "Observación"
        Si **no**, entonces $P \neq NP$.

### Motivación 3: comparar dificultad de problemas

¿Un problema es más fácil que otro?

1. $G_x$ ¿contiene $\binom{k}{2}$ aristas? — $Th^{\binom{n}{2}}_{\binom{k}{2}}$.
2. $G_x$ ¿posee clique de tal tamaño? — $\text{CLIQUE}(n,k)$.

Debería ser que (1) $\leq$ (2), (1) más fácil.

## Matrices booleanas

!!! def "Matriz booleana"
    Es una matriz cuyas entradas tienen $0$s y $1$s.

Si $f(x,y)$ es una función booleana de $2n$ variables, se puede visualizar como una matriz $A$ de tamaño $2^n \times 2^n$, con filas indexadas por $x$ y columnas por $y$:

$$
A_{xy} = f(x,y), \qquad x,y \in \{0,1\}^n.
$$

!!! example "Ejemplo ($n=1$)"
    Tabla de $\lnot x$:

    | $x$ | $\lnot x$ |
    | --- | --- |
    | 0 | 1 |
    | 1 | 0 |

    Tabla de $x \wedge y$, $x \vee y$, $x \to y$:

    | $x$ | $y$ | $\wedge$ | $\vee$ | $x \to y$ |
    | --- | --- | --- | --- | --- |
    | 0 | 0 | 0 | 0 | 1 |
    | 0 | 1 | 0 | 1 | 1 |
    | 1 | 0 | 0 | 1 | 0 |
    | 1 | 1 | 1 | 1 | 1 |

## Funciones booleanas básicas

| Operación | Definición | Nombre |
| --- | --- | --- |
| NOT | $\lnot x = 1-x$ | Negación |
| AND | $x \wedge y = x \cdot y$ | Conjunción |
| OR | $x \vee y = 1-(1-x)(1-y)$ | Disyunción |
| XOR | $x \oplus y = x(1-y) + y(1-x) = (x+y) \bmod 2$ | Paridad |
| Implicancia | $x \to y = \lnot x \vee y$ | |

!!! prop "Reglas de De Morgan"
    $$
    \lnot(x \vee y) = \lnot x \wedge \lnot y
    $$

    $$
    \lnot(x \wedge y) = \lnot x \vee \lnot y
    $$

!!! prop "Distributividad"
    $$
    x \wedge (y \vee z) = (x \wedge y) \vee (x \wedge z)
    $$

    $$
    x \vee (y \wedge z) = (x \vee y) \wedge (x \vee z)
    $$

## Descomposición de matrices

**Motivación:** sea $A \in \{0,1\}^{m \times n}$. Representa la siguiente pregunta:

$$
y_i = \bigvee_{j \,:\, A_{ij}=1} x_j.
$$

!!! example "Ejemplo"
    $$
    A =
    \begin{pmatrix}
    1 & 0 & 1 & 1 & 0 \\
    0 & 0 & 0 & 0 & 0 \\
    1 & 0 & 0 & 0 & 0 \\
    0 & 0 & 1 & 1 & 0
    \end{pmatrix}
    $$

    Entonces $y_1 = x_1 \vee x_3 \vee x_4$ e $y_3 = y_1$ son lo mismo.

    ¿Qué nos cuesta computar? Guardamos $|C|$ columnas. En el ejemplo $|C|=3$ columnas distintas; digamos $z = x_1 \vee x_3 \vee x_4$, la función que guarda para $y_1$ e $y_3$. Nos cuesta además almacenar $|R|$ asignaciones. En total necesitamos $|C| + |R|$ de almacenamiento.

!!! def "Matriz primitiva"
    Una matriz $B \in \{0,1\}^{m \times n}$ se dice primitiva si $\text{rank}(B) = 1$: tiene una columna no trivial que se repite $\leq n$ veces (en filas distintas).

!!! def "Peso"
    $$
    w(B) = |R| + |C|
    $$

    donde $R$ son las filas no nulas y $C$ las columnas no nulas de $B$.

!!! example "Ejemplo"
    En el ejemplo anterior, $|R|=2$ y $|C|=3 \implies w(B) = 5$.

!!! def "Descomposición de una matriz $A$"
    $B_1, \dots, B_r$ primitivas de $n \times n$ tales que $A = B_1 + B_2 + \dots + B_r$, con:

    - $\forall (i,j)$ con $A_{ij}=1$: $\exists! \, k$ tal que $(B_k)_{ij} = 1$ (partición de los $1$s).
    - $\forall (i,j)$ con $A_{ij}=0$: $\forall k$, $(B_k)_{ij} = 0$ (los $0$s son fondo).

!!! def "Peso de la descomposición"
    $$
    \sum_{i=1}^{r} w(B_i)
    $$

!!! def "$DEC(A)$"
    $$
    DEC(A) = \min_{B_1, \dots, B_r \text{ desc. de } A} \sum_{i=1}^{r} w(B_i).
    $$

!!! def "$|A|$"
    $$
    |A| = \#\{(i,j) : A_{ij} = 1\}.
    $$

!!! observation "Observación"
    $DEC(A) \leq m + m \cdot n$, ya que $w(B_j) \leq 1+n \implies \sum w(B_j) \leq m + m \cdot n$.

## Teorema de Lupanov

!!! prop "Lema 1.2 (Lupanov, 1956)"
    $$
    DEC(A) \leq (1+o(1)) \, \frac{m \cdot n}{\log(m \cdot n)}.
    $$

!!! proof "Demostración"
    **Por demostrar:** $DEC(A) \leq \dfrac{m \cdot n}{k} + n \, 2^{k-1}$ para todo $k=1,\dots,n$ (tomando $k = \log m - 2\log\log m$).

    **Caso $k=n$.**

    *Paso 1:* Agrupar las filas de $A$ tales que las filas de cada grupo tengan los mismos valores. ¿Cuántas primitivas? $t \leq 2^n$ filas distintas. Cada fila no nula de $A$ cae en alguna primitiva $B_i = r_i \times c_i$, con $w(B_i) = r_i + c_i$ (filas y columnas distintas).

    $$
    \sum_{i=1}^{t} w(B_i) = \sum_{i=1}^{t} r_i + \sum_{i=1}^{t} c_i \leq m + \sum_{j=0}^{n} \mathbb{1}(c_i = j)\, j \leq \sum_{j=0}^{n} \binom{n}{j} j = m + n\, 2^{n-2}.
    $$

    (¿Cuántas formas hay de elegir $j$? $\#(S,a)$ tal que $S \subseteq \{1,\dots,n\}$, $a \in S$, $|S|=j$: elegir $a \in \{1,\dots,n\}$ ($n$ formas), elegir el resto $\binom{n}{j}$, total $\leq n\, 2^{n-1}$.)

    *Paso 2:* Para $k=1,\dots,n-1$: dividir $A$ en $n/k$ submatrices con $k$ columnas (queda una con residuo). Son $n/k$ matrices tales que $n + k\, 2^{k-1} \geq DEC(A_i)$, luego

    $$
    \sum_i DEC(A_i) \leq m \cdot \frac{n}{k} + \frac{n}{k} \cdot k\, 2^{k-1}. \qquad \blacksquare
    $$
