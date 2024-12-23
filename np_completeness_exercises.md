# Problemas NP, NP-Completo, NP-Hard
Asumiendo que los siguientes problemas ya se han demostrado como NP-Completos:

- SAT
- 3-Sat
- Clique
- Conjunto independiente
- Vertex Cover
- Subset Sum
- Mochila
- Hamilton (Dirigido)
- Viajante

Demuestre que los siguientes problemas son NP-Hard o NP-Completos, según corresponda.

## Set cover
> Dado un conjunto $X$ y una colección S de subconjuntos de $X$, el problema consiste en determinar si existe un subcolector $S' \subseteq S$ tal que cada elemento de $X$ aparezca exactamente una vez en los subconjuntos de $S'$.

NP- HARD:

número cromático es 3 ∝ set cover(exact cover):

Dado un grafo G = (V,E) vamos a crear un conjunto X tal que por cada nodo $v \in V$ se añade a X: v, Rv,Gv y Bv. Por cada arista $<u,v> \in E$ se añade a X: Ruv, Guv y Buv.
Luego vamos a construir S tal que por cada nodo $v \in V$ se añaden Svr, Svg y Svb a S tal que $v \in Svg, Svr, Svb$ y por cada arista <v,u> en E, $Rvu \in Svr$, $Gvu \in Svg$ y $Bvu \in Svb$. Luego por cada arista $<v,u> \in E$ añadimos a S conjuntos unitarios con los elementos Rvu, Gvu y Bvu. Por la forma en que construimos X y S, si existe un exact cover entonces el grafo es 3 coloreable porque para que v esté en S' es necesario escoger Svg, Svr o Svb, lo cual representa asociarle ese color a v porque para añadir a S' algún nodo u que tenga una arista en común con v, hay que añadir Sug, Sur o Sub.
Para ver que determinar si un grafo tiene como número cromático 3 es np-hard leer la demostración de número cromático.

La construcción del conjunto X y la colección S se basa en añadir, para cada vértice y cada arista del grafo original, un número constante de elementos y conjuntos. Así, si |V| es el número de vértices y |E| el número de aristas, la creación de X y S se realiza en O(|V| + |E|) (cada vértice y arista del grafo produce un conjunto finito de elementos y subconjuntos). Por lo tanto, el tamaño total de la instancia resultante y el tiempo para construirla crecen polinomialmente respecto al tamaño de G.

NP:
Dado un conjunto S' se puede pasar por cada uno de sus elementos marcando los elementos de X que se han encontrado para garantizar que se pasa por todos una única vez en tiempo polinomial.

## Clique máximo
> Un clique es un subgrafo completo dentro de un grafo. Formalmente, un clique en un grafo $G=(V,E)$ es un subconjunto de vértices $C \subseteq V$, tal que todos los pares de vértices en $C$ están conectados directamente por una arista. En otras palabras, todos los vértices del clique están mutuamente conectados.

## Hallar el clique de mayor tamaño en un grafo.

Demostración de que clique es NP-Hard:
clique ∝ clique máximo
Si tengo el clique de mayor tamaño del grafo, entonces puedo saber en tiempo polinomial cuál es ese tamaño, al que llamaremos h. Si h es mayor o igual que k, entonces escogemos un subconjunto de vértices del clique(de tamaño k) y tenemos un clique de ese tamaño. Si h < k,entonces no existe un clique de tamaño k porque de existir ese sería el mayor clique, contradicción.

La reducción desde el problema de reconocimiento de cliques (decidir si existe un clique de tamaño al menos k) al problema de encontrar el clique máximo aprovecha la salida de la versión de optimización. Una vez construido el grafo original, el algoritmo que halla el clique máximo se ejecuta en un tiempo polinomial adicional para verificar si el tamaño del clique es al menos k. Esta verificación (comparación y selección de vértices) se realiza en un tiempo lineal o polinomial respecto al número de vértices, por lo que la transformación está acotada polinomialmente.


## Cobertura de Clique
> Dado un grafo $G=(V,E)$, una cobertura de cliques es un conjunto de cliques ${C_1,C_2,…,C_k}$ tal que cada vértice $v \in V$ pertenece a al menos uno de estos cliques.

El objetivo del problema de cobertura de cliques es encontrar el número mínimo de cliques necesarios para cubrir todos los vértices del grafo.

NP- Hard:
número cromático ∝ cobertura de cliques
Sea G = (V,E) y G' su grafo complemento y sea k el número cromático de G; el número mínimo de cliques necesarios para cubrir todos los vértices del grafo G' que es k. En una k-coloración de de G los vértices de un mismo color no tienen ninguna arista entre sí, por lo que en G' formarán un clique. Luego en G' hay k cliques ${C_1,C_2,…,C_k}$ que cumplen que cada vértice $v' \in V'$ pertenece a al menos uno de estos cliques. Si en G' existiera un clique cover de tamaño h menor que k, cada uno de estos cliques formaría un conjunto independiente en G, por lo que coloreando del mismo color los vértices que pertenecen al mismo conjunto independiente se llega a una coloración válida con h colores, por lo que el número cromático de G no sería k, contradicción. Por lo tanto, el menor clique cover de G' es de tamaño k.

La construcción del grafo complemento G' se realiza en tiempo polinómico con respecto a |V| y |E| (señalar la no-arista como arista y viceversa). Posteriormente, la relación entre k-coloraciones en G y la cobertura de cliques en G' se establece mediante una equivalencia simple: cada color en G se traduce en un clique en G'. Todos estos pasos (construir G', verificar sus cliques y reducir al número cromático) implican operaciones polinómicas en el tamaño del grafo original.

## Numero Cromático
> El número cromático de un grafo es el número mínimo de colores necesarios para colorear los vértices del grafo de manera que dos vértices adyacentes no compartan el mismo color.

## Hallar el número cromático en un grafo.

NP-hard:
3-SAT ∝ número cromático
Sean x1, . . . , xn las variables de una instancia de 3-SAT y C1, . . . , Ck sus cláusulas, vamos a construir un grafo G = (V,E) que cumpla que es 3-coloreable si y solo si la instancia de 3-SAT es satisfacible. Para ello a partir de un grafo vacío le añadiremos 3 nodos con aristas entre ellos formando un triángulo, lo que garantiza que cuando se vayan a colorear cada uno reciba un color distinto. Llamaremos a estos nodos True, False y Base y nos referiremos al color que se le asocie a cada uno de la misma manera, por lo que si a un nodo del grafo le corresponde el mismo color que el nodo True, decimos que ese nodo tiene color True. Ahora, por cada variable xi con $i \in [1,n]$ se crean los nodos vi y !vi y se forma un triángulo con ellos y el nodo Base. De esta manera se garantiza que al colorear los nodos correspondientes a una variable no se le pueda asociar el color Base, pero sí True o False y que una variable y su negación nunca tengan el mismo color. Hasta este punto el grafo siempre será 3-coloreable, por lo que necesitamos añadir algo que represente a cada cláusula del 3-SAT y que fuerce al grafo a necesitar más de 3 colores para colorearse en caso de que alguna cláusula no se pueda satisfacer.

![graph_1](graph_1.png)

El grafo representado en la figura anterior garantiza que el nodo superior (nodo A) no podrá ser coloreado con ninguno de los colores Base, True o False en caso de que todos los nodos que correspondan a las variables(v1, !v2, v3) estén coloreados con False. Por lo tanto, en caso de que a todas las variables se les asigne False, el grafo no será 3-coloreable. En cualquier otro caso, el grafo de la figura anterior es 3 coloreable. Luego podemos decir que por cada cláusula Ci se crea un grafo como el de la figura anterior con las variables correspondientes a la cláusula. De esta manera se obtiene como grafo final uno que garantiza que una variable y su negación no reciban el mismo color y que para ser 3-coloreable requiere que en cada cláusula al menos a una variable se le asigne el color True.

Por lo tanto, para saber si la instancia de 3-SAT es satisfacible se puede crear el grafo G y hallar su número cromático. Si este es 3 entonces asignándole el valor true a las variables que corresponden a los nodos que tienen el color True se obtiene una asignación que garantiza que por cada cláusula haya al menos una variable true, lo que hace que la FNC de true. Por otra parte, si tenemos una asignación que satisface la FNC entonces coloreando los nodos que corresponden a cada variable con su valor de verdad se encuentra una manera de colorear el grafo con 3 colores por la manera en que construimos el grafo.

Es polinomial porque durante el proceso de construccion para cada variable xi se añaden un número fijo de vértices y aristas (construyendo el subgrafo que conecta xi, !xi y Base), para cada cláusula Cj, se conecta un conjunto fijo de nuevos vértices según la estructura mostrada en la figura (nodo A y la interacción con las variables incluidas en la cláusula). Además, el número de cláusulas y el número de variables determinan cuántos subgrafos se añaden, y las conexiones se realizan en un número constante de pasos por cada variable y cláusula. Dado que el tamaño del grafo crece de manera lineal o polinómica con respecto a n (variables) y k (cláusulas), la transformación de la instancia de 3-SAT en el grafo G se efectúa en tiempo polinomial.


## Conjunto Dominante
> En un grafo $G=(V,E)$, un conjunto de vértices $D \subseteq V$ es un conjunto dominante si cada vértice de $V$ que no está en $D$ es adyacente a al menos un vértice en $D$.

> Una partición de los vértices $V$ en $k$ conjuntos $D_1,D_2,…,D_k$​ es una partición domática si cada $D_i$​ (para $i=1,2,…,k$) es un conjunto dominante. El numero dominante es la cardinalidad del menor conjunto dominante de $G$.

## Hallar el numero dominante de $G$.

NP - Hard:

Vertex Cover ∝ Número Dominante:

Sea G = (V,E) un grafo de entrada para Vertex Cover. Construiremos un grafo G' = (V',E') de la siguiente manera:
- Para cada arista e = {u,v} ∈ E, añadimos un nuevo vértice uv
- Conectamos uv con los vértices u y v
- V' contiene todos los vértices originales V más los nuevos vértices uv
- E' contiene las nuevas aristas descritas

Sea:
- k = tamaño del min vertex cover en G
- h = número dominante de G' (tamaño del min conjunto dominante)

Demostración k ≥ h:

Sup k < h:
Tenemos un vertex cover C = {v₁,...,vₖ} en G. Este conjunto C también es un conjunto dominante en G' porque:
- Cada vértice original v ∈ V-C es adyacente a algún vértice en C (por ser vertex cover)
- Cada vértice nuevo uv es adyacente a u o v, y al menos uno está en C
Por lo tanto, existe un conjunto dominante de tamaño k < h en G', contradicción con h ser el mínimo.
Luego k ≥ h

Sup k > h:

Si D es un conjunto dominante mínimo en G' de tamaño h, entonces los vértices de D correspondientes a vértices originales forman un vertex cover en G de tamaño ≤ h. Si hubiera una arista {u,v} en G no cubierta, su vértice uv en G' no estaría dominado, contradiciendo que D es dominante.
Luego existe un vertex cover de tamaño menor que k que es el mínimo, contradicción. Por lo tanto k ≤ h.

Como k ≥ h y k ≤ h, entonces k = h.

Por lo tanto, para hallar el menor vertex cover de G, basta calcular el número dominante de G'.

La reducción es polinomial y preserva la solución, demostrando que Número Dominante es NP-hard.

## Número Domatic
> El número de domatic de un grafo $G$, denotado como $domatic(G)$, es el número máximo $k$ tal que los vértices de $G$ pueden dividirse en $k$ conjuntos disjuntos $D_1,D_2,…,D_k$ donde cada $D_i$​ es dominante.

## Hallar el número de Domatic de un grafo.

NP - Hard:

3-COLORABILIDAD ∝ NÚMERO DOMÁTICO

Se hará una reducción polinómica g de 3-COLORABILIDAD a DNP (Problema del Número Domático), conociendo que el problema 3-COLORABILIDAD es NP-Hard(la demostración de que número cromático es NP-Hard se hace construyendo un grafo cuyo número cromático es exactamente 3 si y solo si existe una asignación de valores de verdad tal que se satisfaga la FNC que recibe 3-SAT. Ese grafo se garantiza que no es 2-coloreable porque se construye un triángulo entre los 3 nodos que representan a los colores: True, False, Base; luego si ese grafo es 3-coloreable podemos decir que se satisface la FNC, por lo tanto 3-coloreable también es NP-Hard), con las siguientes propiedades, donde δ(G) es el número domático de G:

- Si G ∈ 3-COLORABILIDAD → δ(G) = 3
- Si G ∉ 3-COLORABILIDAD → δ(G) = 2

Demostración:
Dado un grafo G, construimos un nuevo grafo H = g(G) de la siguiente manera:

V(H) = V(G) ∪ {ui,j | {vi, vj} ∈ E(G)} ;

E(H) = { {vi, ui,j} | {vi, vj} ∈ E(G)} ∪ { {vj, ui,j} | {vi, vj} ∈ E(G)} ∪ { {vi, vj} | 1 ≤ i, j ≤ n y i ≠ j } }

Por construcción, dado que min-deg(H) = 2 y H no tiene vértices aislados, la desigualdad δ(H) ≤ min-deg(H)+1 implica que 2 ≤ δ(H) ≤ 3.

Esto ocurre por las siguientes razones:
1. Cada vértice uᵢⱼ está conectado exactamente a dos vértices (los extremos de su arista original), por lo que min-deg(H) = 2
2. El número domático δ(H) no puede ser mayor que min-deg(H)+1 porque si tuviéramos más conjuntos dominantes que min-deg(H)+1 existiría algún vértice v con grado min-deg(H), este vértice solo podría ser dominado por sí mismo y sus vecinos. Por tanto, como máximo podría ser dominado por min-deg(H)+1 conjuntos (él mismo y sus vecinos)
3. Por otro lado, δ(H) ≥ 2 porque H no tiene vértices aislados y podemos dividir siempre V(H) en dos conjuntos dominantes

Primero demostraremos que si tenemos una caja negra que nos dice que δ(H) = 3, entonces podemos construir una 3-coloración válida para G. Sea una partición de V(H) en tres conjuntos dominantes Ĉ₁, Ĉ₂, y Ĉ₃. Coloreamos los vértices de G de la siguiente manera: para cada vértice v en V(G), le asignamos el color k si v está en Ĉₖ. Esta coloración es válida porque:
- Cada vértice debe pertenecer a exactamente un conjunto dominante
- Para cada arista original (vᵢ,vⱼ), el vértice uᵢⱼ correspondiente forma un triángulo que debe ser dominado por los tres conjuntos
- Por lo tanto, vᵢ y vⱼ deben estar en diferentes conjuntos dominantes, lo que implica diferentes colores

Para la otra dirección, supongamos que G es 3-coloreable con clases de color C₁, C₂, y C₃. Construimos tres conjuntos dominantes en H de la siguiente manera: para cada k, definimos Ĉₖ como la unión de Cₖ con los vértices uᵢⱼ donde vᵢ y vⱼ no están en Cₖ. Estos conjuntos son dominantes porque:
- Cada Ĉₖ contiene vértices del clique original, por lo que domina todos los vértices originales
- Cada triángulo {vᵢ,uᵢⱼ,vⱼ} tiene un elemento de cada conjunto Ĉₖ
- Por lo tanto, cada conjunto domina todos los vértices nuevos uᵢⱼ

La reducción es polinómica porque la construcción de H requiere tiempo O(|E|) para crear los nuevos vértices y O(|V|²) para el clique, y las transformaciones de soluciones son lineales en ambas direcciones.

Por construcción, cuando G no es 3-coloreable, necesariamente δ(H) = 2.


## Ancho de Banda
> Dado un grafo $G=(V,E)$ y una disposición lineal de sus vértices representada como una función $f:V→{1,2,…,|V|}$, el ancho de banda de $G$ para esa disposición es:

>$$max⁡{|f(u)-f(v)| : (u,v) \in E}$$

>El problema consiste en encontrar una disposición $f$ que minimice este valor, es decir, reducir al mínimo la distancia máxima en la disposición lineal entre los extremos de las aristas del grafo.

## Retroalimentación de Vértices
> Dado un grafo $G=(V,E)$, un conjunto de retroalimentación de vértices es un subconjunto de vértices $F \subseteq V$ tal que al eliminar todos los vértices en $F$ (y sus aristas incidentes), el grafo resultante no contiene ciclos (es un grafo acíclico o un bosque, si es no dirigido).

El objetivo del problema es encontrar el conjunto de retroalimentación de vértices de tamaño mínimo.

NP- Hard:

vertex cover ∝ retroalimentación de vértices:

Sea $G=(V,E)$ un grafo, crearemos un grafo G' dirigido tal que para todo $v \in V$ se añaden 2 nodos: v0 y v1 con un arco de v0 a v1. Luego por cada arista $<u,v> \in E$ vamos a añadir en G' un arco de u1 a v0 y de v1 a u0. De esta manera cada arista en G va a estar representada en el grafo G' por un ciclo que solo contiene a los 4 vértices que corresponden a los vértices que toca esa arista en G.
tamaño del min vertex cover = k
tamaño del min conjunto de retroalimentación de vértices = h
Sup k < h:
Tenemos un conjunto de vértices {v1,v2,..., vk} que tocan a todas las aristas del grafo G; al eliminar en el grafo G' a vi0 para todo $i \in [1,k]$ deben quedar ciclos porque de lo contrario tendríamos un conjunto de retroalimentación de vértices menor que el mínimo. Ahora bien, por la manera en la que construimos el grafo G' todos los arcos van de un nodo con índice 0 a uno con índice 1, por lo tanto sin pérdida de generalidad podemos decir que el ciclo empieza en el nodo x0. Luego el ciclo tendría la forma <x0,x1,wo,...,x0> porque de x0 el único arco que sale va hacia x1. Pero de x1 solo se crea un arco hacia w0 si en G existía una arista entre los vértices x y w por lo tanto, x o w deben pertenecer al vertex cover de G, por lo que al eliminar los nodos con índice 0 correspondientes a los vértices del vertex cover el ciclo deja de existir. Por lo tanto no quedarían ciclos en G' después de eliminar k nodos, luego existe un conjunto de retroalimentación de vértices de tamaño k que es menor que el mínimo, contradicción.
Por esto k >= h

k <= h:
Si enemos en G' un conjunto de retroalimentaciónde vértices S' de tamaño h . Entonces tomando los vértices que corresponden a los nodos de S' en G tenemos el conjunto S de tamaño h. Si existe alguna arista <x,v> en G tal que ni x ni v pertenecen a S, entonces en G' quedarían los 4 nodos correspondientes a estos vértices después de eliminar los que pertenecen al conjunto de retroalimentación, por lo que quedaría el ciclo formado por ellos. Luego S'no sería un conjunto de retroalimentación de vértices, por lo tanto, el conjunto S es un vertex cover de tamaño h, de ahí que k <=h.

Como k >= h y  k <= h entonces k = h.

Luego, al obtener el tamaño del menor conjunto de retroalimentación de G'tenemos el tamaño del menor vertex cover en G. Por lo tanto, para saber si el grafo G tiene vertex cover de tamaño k obtenemos el tamaño del menor conjunto de retroalimentación, si es menor o igual que k entonces se devuelve True, en caso contrario se devuelve False.

Es polinomial porque para cada vértice en (G) se agregan exactamente dos vértices en (G') y un arco entre ellos (con índice 0 y 1). Para cada arista en (G), se agregan solo dos arcos adicionales en (G'). El tamaño de (G') y el número de arcos crecen, por tanto, de forma lineal respecto a (|V|) y (|E|). Verificar y construir la correspondencia (vertex cover ↔ retroalimentación de vértices) no requiere más que iterar sobre vértices y aristas del grafo original.

## Retroalimentación de Arcos
>Dado un grafo $G=(V,E)$, un conjunto de retroalimentación de arcos es un subconjunto de arcos $F \subseteq E$ tal que al eliminar todos los arcos en $F$, el grafo resultante no contiene ciclos (es un grafo acíclico o un bosque, si es no dirigido).

>El objetivo del problema es encontrar el conjunto de retroalimentación de arcos de tamaño mínimo.

NP-Hard:
vertex cover ∝ retroalimentación de arcos

La entrada del problema de Cobertura de Vértices es un grafo no dirigido \( G = (V, E) \). Dado \( G = (V, E) \), creamos un grafo dirigido \( G' = (V', E') \) tal que por cada vértice v que pertenece a V, v y v' pertenecen a V' y E'= {(𝑣,𝑣′),(𝑣′,𝑢),(𝑢′,𝑣)∣⟨𝑢,𝑣⟩∈𝐸}.

Es polinomial porque para cada vértice (v\in V), se añaden en (G') exactamente dos nodos ((v) y (v')), así como un arco ((v,v')). Para cada arista (\langle u,v\rangle \in E), se añaden las aristas ((v',u)) y ((u',v)) en (G'). El tamaño del nuevo grafo (G') es proporcional a la suma de vértices y arcos del grafo original (G). La correspondencia entre “arcos de retroalimentación en (G')” y “conjunto vertex cover en (G)” se define mediante reglas directas (vértices vs. arcos derivados), lo cual implica que tanto la construcción como la verificación se llevan a cabo en tiempo acotado por un polinomio de (|V| + |E|).
## Correctitud
Existe una cobertura de vértices en \( G \) de tamaño \( k \) si y solo si existe un conjunto de retroalimentación de arcos en \( G' \) de tamaño \( k \).

### Demostración
(⇐)Sea \( S' \) un conjunto de retroalimentación de arcos de \( G' \) de tamaño \( k \). Si existe 𝑒∈𝑆' tal que \( e \) no es una arista \( (v', v) \), entonces \( e \) es una arista de la forma \( (v', u) \). Si \( e \) es una arista \( (v', u) \), como 𝑒∈𝑆', abarca todos los ciclos \( [v', u, ..., v'] \). Todos los caminos de u a v' pasan por \( v \), ya que el único arco incidente en \( v' \) es \( v \). Por tanto, si sustituimos \( (v', u) \) por \( (v, v') \), se abarcan los mismos ciclos de \( G' \). Por tanto, es posible crear un nuevo conjunto \( S' \) de tamaño \( k \) formado solamente por arcos de la forma \( (v, v') \). Dado el nuevo conjunto \( S' \), como \( S' \) abarca todos los ciclos, también abarca los ciclos \( c \) de la forma \( c = [v, v', u, u', v] \) en \( G' \). A cada ciclo \( c \) en \( G' \) se le asocia una arista ⟨𝑢,𝑣⟩∈𝐸 y viceversa. Como por cada ciclo \( c \) se cumple que (𝑣,𝑣′)∈𝑆′ y (𝑢,𝑢′)∈𝑆', entonces es posible crear un conjunto \( S \) formado por los vértices de \( V \) correspondientes a las aristas de \( S' \), el cual abarca todas las aristas de \( G \). Por tanto, \( S \) es una cobertura de vértices de \( G \).


(⇒) Sea \( S \) una cobertura de vértices de \( G \), entonces para todo ⟨𝑢,𝑣⟩∈𝐸 se cumple que u∈𝑆 o v∈𝑆. Sea \( S' \) un conjunto de arcos formado por los arcos \( (v', v) \) correspondientes a los vértices \( v \) que pertenecen a \( S \). Demostremos que \( G' - S' \) no tiene ciclos. Dado un ciclo \( c \) de \( G' \); si v'∈ c, entonces <v,v'> ∈c, ya que en \( v' \) solo incide \( v \); si v∈c, entonces <v,v'>∈c, ya que \( v \) solo incide en \( v' \). Por tanto, en cada ciclo de \( G' \) existe un arco de la forma \( (v, v') \), por lo que \( S' \) es una retroalimentación de vértices de \( G' \).

Luego para saber si existe un vertex cover de tamaño k en G, realizo la transformación y si el conjunto de retroalimentación de arcos de tamaño mínimo de G' es de tamaño h > k retorno False, porque de existir un vertex cover de tamaño k en G también habría un conjunto de retroalimentación de arcos de tamaño k en G', lo cual no pasa porque h es el mínimo. En otro caso retorno True.


## 3D Matching
> El problema se basa en encontrar un emparejamiento dentro de un conjunto tridimensional.

Supongamos que tienes tres conjuntos disjuntos: $X$, $Y$, y $Z$, cada uno de tamaño $n$. También tienes un conjunto $T$ de ternas de la forma $(x,y,z)$, donde $x \in X$, $y \in Y$, y $z \in Z$.

El objetivo es determinar si existe un subconjunto de $T$ de tamaño $n$ (es decir, nn ternas) tal que cada elemento de $X$, $Y$, y $Z$ aparezca exactamente una vez en las ternas seleccionadas.

Es polinomial porque cada vértice y arista del grafo original se mapea de forma directa y lineal a los conjuntos (X), (Y), (Z) y las ternas en (T). Para cada arista (\langle u, v \rangle) en (G), se crea una terna ((u, v, z)) en (T), donde (z) es un elemento único asociado. Este proceso implica recorrer todas las aristas una sola vez y generar un número de ternas proporcional al número de aristas, lo que asegura que la reducción crece linealmente con el tamaño de la entrada original.
## Dimensión Bipartita
> Para un grafo $G=(V,E)$, la dimensión bipartita $rb(G)$ es el mínimo número de subgrafos bipartitos completos (permitiendo repetición de aristas) cuya unión incluye todas las aristas de G.

> Determinar la dimensión bipartita de un grafo cualquiera.

NP - Hard:

- Sea P₀ Clique Cover.
- Sea P₁: Determinar el mínimo número de subgrafos bipartitos completos (CBS) de G que cubren un subconjunto específico H de aristas de G (donde G es bipartito).
- Sea P₂: Determinar RB(G), donde RB(G) es la dimensión bipartita de G, definida como el mínimo número de subgrafos bipartitos completos (permitiendo repetición de aristas) cuya unión incluye todas las aristas de G.

P₀ ∝ P₁ (Clique Cover se reduce a Cubrimiento con CBS)

Dado un grafo G con vértices v₁,...,vₙ, construimos G' con vértices x₁,...,xₙ y y₁,...,yₙ. Definimos las aristas de G' de manera que xᵢ → yᵢ para todo i de 1 a n, y si vᵢ → vⱼ en G, entonces xᵢ → yⱼ en G'. Sea H' el conjunto de aristas {(xᵢ,yᵢ) | i = 1,...,n}.

Sea C' un subgrafo bipartito completo en G' que incluye las aristas (x₁,y₁),...,(xₖ,yₖ). Entonces en G, los vértices (v₁,...,vₖ) forman necesariamente un clique. Esto es porque si dos vértices están en C', sus correspondientes vértices en G deben estar conectados por definición de nuestra construcción.

En la otra dirección, si C es un clique en G que incluye el vértice vᵢ, entonces podemos construir un subgrafo bipartito completo en G' que incluye la arista (xᵢ,yᵢ), tomando todos los vértices correspondientes al clique. Por tanto, el mínimo número de cliques necesarios para cubrir los vértices en G es igual al mínimo número de subgrafos bipartitos completos necesarios para cubrir las aristas de H' en G'.

P₁ ∝ P₂ (Cubrimiento con CBS se reduce a Dimensión Bipartita)

Dado un grafo bipartito G con vértices {x₁,...,xₛ,y₁,...,yₜ} y un subconjunto H de aristas, construimos G' donde G es un subgrafo de G'. Para cada arista (xᵢ,yⱼ) que está en G pero no en H, añadimos dos nuevos vértices xᵢⱼ y yᵢⱼ, junto con las aristas xᵢⱼ → yᵢⱼ, xᵢⱼ → yⱼ, y xᵢ → yᵢⱼ.

Por la construcción realizada, cada arista (xᵢⱼ,yᵢⱼ) pertenece a un único subgrafo bipartito completo maximal que también incluye la arista (xᵢ,yⱼ). Debido a la estructura especial creada con los vértices adicionales, este subgrafo bipartito completo debe aparecer en cualquier cubierta mínima, ya que es la única forma de cubrir la arista (xᵢⱼ,yᵢⱼ) eficientemente.

Como esta construcción se realiza para cada arista que no está en H, garantizamos que todas las aristas fuera de H quedan cubiertas por subgrafos bipartitos completos de manera única y necesaria. Por lo tanto, el número mínimo de subgrafos bipartitos completos necesarios para cubrir H es exactamente RB(G') - |H^c|, donde H^c son las aristas de G que no están en H.

Esto completa la cadena de reducciones, demostrando que determinar la dimensión bipartita es NP-Hard.

## Numero de Intersección
> Sea $G=(V,E)$ un grafo no dirigido con $V$ como el conjunto de vértices y $E$ como el conjunto de aristas. El número de intersección de $G$, denotado como $int(G)$, es el mínimo entre las cardinalidades de una colección de conjuntos ${S_v​:v \in V}$, tal que:

> A cada vértice $v \in V$ se le asigna un conjunto $S_v$​.
> Existe una arista $(u,v) \in E$ si y solo si $S_u \cap S_v \neq \emptyset$.
> En otras palabras, el número de intersección mide cuántos conjuntos son necesarios para representar todas las relaciones (aristas) entre los vértices mediante intersecciones de conjuntos.

## Máximo Corte
> Sea $G=(V,E)$ un grafo con aristas ponderadas. Un corte es una division de los vertices en dos conjuntos $T$ y $V-T$. El costo de un corte es la suma de los pesos de las aristas que van de $T$ a $V-T$. El problema trata de encontrar el corte de mayor costo de un grafo.

NP-Hard:

MAX-2-SAT:
Dado un conjunto de m cláusulas disyuntivas con a lo sumo 2 literales y un entero k, decir si existe una manera de asignar valores de verdad a las variables tal que se cumplan al menos k cláusulas.

Máximo corte simple:
> Sea $G=(V,E)$ un grafo con aristas ponderadas y W un entero. Un corte es una division de los vertices en dos conjuntos $T$ y $V-T$. El costo de un corte es la suma de los pesos de las aristas que van de $T$ a $V-T$. Todas las aristas tendrán peso = 1. Se debe decir si existe un corte cuyo costo sea mayor o igual que W.
>
Primero demostraremos que 3-SAT se reduce a MAX-2-SAT, y luego que MAX-2-SAT se reduce a MÁXIMO CORTE Simple.

3-SAT ∝ MAX-2-SAT

Sea S un conjunto de m cláusulas disyuntivas, cada una con a los sumo 3 literales (podemos asumir que tienen exactamente 3 literales sin pérdida de generalidad repitiendo literales si es necesario). Etiquetamos las cláusulas como (aᵢ ∨ bᵢ ∨ cᵢ), donde i va de 1 a m, y cada aᵢ, bᵢ, y cᵢ representa una variable o su negación.

Para cada cláusula i, construimos el siguiente conjunto de cláusulas S' con dos literales máximo:
- Cláusulas unitarias: (aᵢ), (bᵢ), (cᵢ), (dᵢ)
- Cláusulas binarias: (¬aᵢ ∨ ¬bᵢ), (¬aᵢ ∨ ¬cᵢ), (¬bᵢ ∨ ¬cᵢ), (aᵢ ∨ ¬dᵢ), (bᵢ ∨ ¬dᵢ), (cᵢ ∨ ¬dᵢ)

Establecemos k = 7m.

La construcción es correcta porque:
1. Si S es satisfacible, entonces exactamente 7 cláusulas de cada grupo de 10 en S' pueden ser satisfechas simultáneamente.
2. Si S no es satisfacible, entonces a lo sumo 6 cláusulas de cada grupo pueden ser satisfechas.
3. Por lo tanto, S' tiene una asignación que satisface 7m o más cláusulas si y solo si S es satisfacible.

MAX-2-SAT ∝ MÁXIMO CORTE simple

Dado un conjunto de p cláusulas (aᵢ ∨ bᵢ) y un entero k para MAX-2-SAT, construimos un grafo G de la siguiente manera:

1. Conjunto de vértices N:
   - Vértices Tᵢ y Fᵢ para 0 ≤ i ≤ 3p
   - Vértices tᵢⱼ y fᵢⱼ para cada variable i y 0 ≤ j ≤ 3p
   - Vértices xᵢ y x̄ᵢ para cada variable i

2. Conjunto inicial de aristas A₁:
   - {Tᵢ,Fᵢ} para todo i
   - {tᵢⱼ,fᵢⱼ} para toda variable i y todo j
   - {xᵢ,fᵢⱼ} y {x̄ᵢ,tᵢⱼ} para toda variable i y todo j

3. Conjunto adicional de aristas A₂:
   - {aᵢ,bᵢ} para cada cláusula i donde aᵢ ≠ bᵢ
   - {aᵢ,F₂ᵢ₋₁} y {bᵢ,F₂ᵢ} para cada cláusula i

El problema del MÁXIMO CORTE simple se formula con el grafo G = (N, A₁ ∪ A₂) y W = |A₁| + 2k.


La construcción es correcta porque:
1. Una asignación que satisface k o más cláusulas induce una partición con al menos |A₁| + 2k aristas cruzando el corte.
2. Una partición con |A₁| + 2k o más aristas cruzando el corte induce una asignación que satisface al menos k cláusulas.
3. La estructura de A₁ fuerza una correspondencia entre particiones válidas y asignaciones de verdad consistentes.

Luego determinando si existe un máximo corte simple con costo mayor o igual que W se determina si se pueden asignar valores de verdad a las cláusulas que recibió MAX-2-SAT como entrada tal que den True al menos k.

Como el problema de máximo corte consiste en hallar el corte con mayor costo, si el costo es menor que W podemos devolver False en el problema de decisión (máximo corte simple), en otro caso devolvemos True. Luego máximo corte es NP-Hard.

## Subgrafo Máximo Bipartito
> El problema consiste en encontrar dado un grafo $G=(V,E)$ el subgrafo $G'=(V',E')$ con $V' \subseteq V$ y $E' \subseteq E$ de forma que $G'$ sea bipartito y $|E'|$ es máximo.

NP-Hard:

Máximo Corte simple ∝ Subgrafo Máximo Bipartito

Sea una instancia de Máximo Corte simple consistente en un grafo G = (V,E) y un entero k. La correspondiente instancia del problema del Máximo Subgrafo Bipartito utilizará el mismo grafo G.

Primero, supongamos que G' = (V',E') es un subgrafo bipartito de G con |E'| ≥ k. Como G' es bipartito, existe una bipartición (X,Y) de V' tal que cada arista en E' tiene un extremo en X y otro en Y. Esta bipartición puede extenderse a una partición (V₁,V₂) de todo V asignando los vértices en V-V' arbitrariamente: V₁ = X ∪ A y V₂ = Y ∪ B, donde A ∪ B = V-V' es cualquier partición del resto de vértices. Por construcción, todas las aristas en E' cruzan entre V₁ y V₂, lo que garantiza un corte de tamaño al menos k en G.

En la otra dirección, si existe una partición (V₁,V₂) de V que produce un corte de costo al menos k en G, entonces podemos construir un subgrafo bipartito G' tomando V' = V y E' como el conjunto de todas las aristas que cruzan entre V₁ y V₂. Por definición, G' es bipartito con bipartición (V₁,V₂) y contiene al menos k aristas.
