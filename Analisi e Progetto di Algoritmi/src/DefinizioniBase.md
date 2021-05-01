## DEFINIZIONI

**VERTICE RAGGIUNGIBILE** 
Dati due vertici *u* e *v* $\in$ V, il vertice *v* è raggiungibile dal vertice *u* *(v $\sim$ u)* SSE $\exist$ un ***cammino*** dal vertice *u* al vertice *v*.
Nel caso di grafi non orientati $\sim$ è una relazione d'equivalenza *(simmetrica, riflessiva e transitiva)*.



**CAMMINO**
Un cammino da *u* a *v* è una sequenza *(finita)* di vertici **u<sub>0</sub>, u<sub>1</sub>, ..., u<sub>k</sub>** dove u<sub>0</sub> = *u* e u<sub>k</sub> = *v* tale per cui $\forall$ i $\in$ {0, ..., k - 1}	(u<sub>i</sub>, u<sub>i + 1</sub>) $\in$ E.
Ovviamente un cammino può anche avere lunghezza zero.



**DISTANZA FRA VERTICI**
La distanza di un vertice *v* da un vertice *u* è il numero di archi su un cammino minimo da *u* a *v*.
La distanza tra *u* e *u* stesso vale 0.



**GRAFO CONNESSO**
Un grafo non orientato G è connesso SSE $\forall$ u, v $\in$ V, v è ***raggiungibile*** da u.
Formalmente G = (V, E) | $\forall$ u, v $\in$ V, v è raggiungibile da u.



**COMPONENTE CONNESSA**
Una componente connessa è un sotto insieme di vertici CC $\subseteq$ V tale che $\forall$ u, v $\in$ CC, v è ***raggiungibile*** da u.
Formalmente: CC $\subseteq$ V | $\forall$ u, v $\in$ CC : v è raggiungibile da u
e ogni altro sottoinsieme che contiene CC è esattamente uguale a CC.

Compattamente: Una CC è un elemento di V / $\sim$ *(insieme quoziente della relazione di raggiungibilità)*.
Dove $\sim$ è la relazione di raggiungibilità.



**ALBERO**
Un grafo G = (V, E) non orientato è un albero SSE

- è connesso e aciclico *(privo di cicli)*.
- è connesso e |E| = |V| - 1.
- è aciclico e |E| = |V| - 1.



**FORESTA**
Un grafo G = (V, E) non orientato è una foresta se due suoi vertici qualsiasi sono connessi da **al più** un cammino *(aciclico)*. Una foresta risulta quindi essere un'unione disgiunta di alberi.



**GRAFO COMPLETO**
Un grafo è completo quando ogni suo vertice è collegato ad ogni altro suo vertice.
Formalmente:

- PER GRAFI NON ORIENTATI:	$\forall$ v $\in$ V,	$\forall$ u $\in$ V - {v}		(u, v) $\in$ E
- PER GRAFI ORIENTATI:	$\forall$ v $\in$ V, $\forall$ u $\in$ V		(u, v) $\in$ E



**ALBERO BFS**
Denotiamo l'albero BFS con **G$_{\pi}$ = (V$_{\pi}$, E$_{\pi}$)** tale che:
V$_{\pi}$ contiene tutti i vertici raggiungibili dalla sorgente *s*.
Possiamo quindi definire V$_{\pi}$ in 3 modi equivalenti, ovvero:

- V$_{\pi}$ = { v $\in$ V | v.col $\neq$ WHITE } *(oppure v.col = BLACK)*
- V$_{\pi}$ = { v $\in$ V | v.d $\neq$ $\infty$ }
- V$_{\pi}$ = { v $\in$ V | v.$\pi$ $\neq$ NIL } $\cup$ {*s*}

E$_\pi$ sono gli archi dell'albero.
Definiamo E$_{\pi}$ come

- E$_{\pi}$ = { (v.$\pi$, v) | v $\in$ V$_\pi$ - {*s*} }
- E$_\pi$ = { (u, v) | u, v $\in$ V$_{\pi}$ - {s} : v.$\pi$ = u} $\cup$ { (*s*, w) | w $\in$ Adj[*s*] } *(non sono sicuro della correttezza di questa)*



**FORESTA DFS**
Denotiamo la foresta DFS con **G$_\pi$ = (V, E$_\pi$)** tale che:
V è l'insieme dei vertici del grafo originario, poiché DFS visita tutti i nodi.
E$_\pi$ rappresenta gli archi della foresta e lo definiamo come:
E$_\pi$ = { (v.$\pi$, v) | v $\in$ V $\land$ v.$\pi$ $\neq$ NIL }



**MST**
Sia $G = (V, E)$ un grafo non orientato e connesso.
Un albero di copertura di $G$ è un grafo $(V, T)$ con $T \subseteq E$ ***aciclico***.

Sia $G = (V, E)$ un grafo non orientato, connesso e pesato mediante la funzione $w: E \rightarrow \mathbb{R}$.
Un albero di copertura minimo di $G$ è un albero di copertura $(V, T)$ tale che $w(T) = \min\{w(A) \space | \space A \subseteq E \land (V, A) \space albero \space di \space copertura \space di \space G\}$.



**MATROIDI**
Un matroide è una coppia $<E, F>$ tale che

- $E$ sia un insieme finito ed $F$ una famiglia di sottoinsiemi di $E$ tale per cui $\forall A \in F$ e $B \subseteq A$ $\implies$ $B \in F$, ovvero che sia un sistema di indipendenza
- E tale che $\forall A,B \in F$, con $|B| = |A| +1$, $\exist b \in B - A$ tale che $A \cup \{b\} \in F$.

Il teorema di Rado ci dice inoltre che se $<E, F>$ è un matroide, allora qualsiasi funzione peso utilizziamo, l'algoritmo greedy standard ci darà la soluzione ottima.