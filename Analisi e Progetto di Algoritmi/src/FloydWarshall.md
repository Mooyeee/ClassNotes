## FLOYD WARSHALL

Dato un grafo orientato e pesato G = (V, E) con W matrice dei pesi dove $\forall$ i, j	w<sub>i, j</sub> = $\begin{cases} 0 &se \space i = j \\ peso \space arco \space(i, j) &se \space (i, j) \in E \\ \infty &se \space (i, j) \notin E \end{cases}$

**PROBLEMA**: Calcolare $\forall$ (i, j) il peso di un cammino minimo da *i* a *j*.
**SOLUZIONE**: Algoritmo di FW che è un algoritmo di programmazione dinamica.

Di conseguenza dobbiamo definire i sotto problemi e le loro istanze.
Il sottoproblema k-esimo sarà definito come:
$\forall$ (i, j) tutti i cammini da *i* a *j* con vertici intermedi $\in$ {1, ..., k}
**ATTENZIONE**: ***NON*** significa con *k* vertici intermedi!

Soluzione sotto problema: D<sup>(k)</sup> = (d$_{i, j}^{(k)}$)<sub>i, j $\in$ V</sub>

Soluzione problema originale: D<sup>(n)</sup> = (d$_{i, j}^{(n)}$)<sub>i, j $\in$ V</sub>

**CASO BASE**
k = 0
$\forall$ (i, j) $\in$ V<sup>2</sup>	d =$_{i, j}^{(0)}$ w<sub>i, j</sub> = $\begin{cases} 0 &se \space i = j \\ peso \space arco \space(i, j) &se \space (i, j) \in E \\ \infty &se \space (i, j) \notin E \end{cases}$

Di conseguenza, D<sup>(0)</sup> = W.

**CASO PASSO**
k > 0
Assumiamo di aver già calcolato i sotto problemi più piccoli.

d$_{i, j}^{(k)}$ = min{ d$_{i, j}^{(k-1)}$,	d$_{i, k}^{(k-1)}$ + d$_{k, j}^{(k-1)}$ }























**ATTENZIONE**: Nello svolgimento degli esercizi di questo tipo spesso conviene pensare prima al caso passo, suddividendolo in due sotto casi; k $\in$ cammino minimo e k $\notin$ cammino minimo, unendoli poi alla fine in un'unica equazione ricorsiva e stabilendo poi il caso base.

<div style="page-break-after: always;"></div>

## ESERCIZI VISTI

### LUNGHEZZA $\leq$ L

Dato un grafo G = (V, E, W) pesato, orientato e senza cappi e dato un intero L > 0, calcolare $\forall$ (i, j) $\in$ V<sup>2</sup> il peso di un cammino minimo da *i* a *j* di lunghezza $\leq$ L

**INPUT**: G = (V, E, W)	L

**SOTTO PROBLEMA**: definito da k $\in$ {0, ..., n} e da l $\in$ {0, ..., L}

$\forall$ (i, j) $\in$ V<sup>2</sup> calcolare il peso di un cammino minimo da *i* a *j*

- utilizzando vertici intermedi appartenenti a {1, ..., k}
- di lunghezza $\leq$ l

**DEFINIZIONE VARIABILI**
Per ogni sotto problema avremo quindi una variabile $D^{(k, l)}$ = ($d_{i, j}^{(k, l)}$) dove $\forall$ (i, j) $d_{i, j}^{(k, l)}$ è il peso del cammino minimo da *i* a *j* con vertici intermedi $\in$ {1, ..., k} di lunghezza $\leq$ l.

**CASO BASE** (k, l) con k = 0

$d_{i, j}^{(0, l)}$ = $\begin{cases} 0 &se \space i = j \\
 w_{i, j} & se \space i \neq j \land (i, j)
 \in E \\ \infty &altrimenti \end{cases}$

**CASO PASSO** (k, l) con k > 0	$\forall$ l $\in$ {1, ..., L}

**Caso 1**: k $\notin$ cammino minimo $\rightarrow$ $d_{i, j}^{(k, l)}$ = $d_{i, j}^{(k - 1, l)}$

**Caso 2**: k $\in$ cammino minimo $\rightarrow$ Da *i* a *k* ho un cammino $\leq$ l<sub>1</sub>, da *k* a *j* $\leq$ l<sub>2</sub>, devo far in modo che il cammino che scelgo sia l<sub>1</sub> + l<sub>2</sub> $\leq$ l

*e<sub>1</sub>* : $d_{i, j}^{(k, l)}$ = min { $d_{i, k}^{(k-1, l_1)}$ + $d_{k, j}^{(k-1, l_2)}$ } con l<sub>1</sub> $\in$ {1, ..., l}, l<sub>2</sub> $\in$ {1, ..., l}	e	l<sub>1</sub> + l<sub>2</sub> $\leq$ l

Tuttavia questo vale solo per l > 1.

Se l = 1 e k $\in$ cammino minimo, $d_{i, j}^{(k, l)}$ = $\infty$ poiché non esiste un cammino di lunghezza 1 che abbia *k* > 0 vertici intermedi.

***Quindi nel caso 2 avremo:***

*e<sub>2</sub>* : $d_{i, j}^{(k, l)} = \begin{cases} \min\limits_{(l_1, l_2) \in \{0, \space ...,\space l\}^2 \space : \space l_1 + l_2 \leq l} \{d_{i, k}^{(k-1, l_1)} + d_{k, j}^{(k-1, l_2)}\} & se \space l > 1 \\
\infty &se \space l = 1\end{cases}$

Questo è ovviamente nel caso 2, noi dobbiamo prendere il migliore tra il caso 1 e il caso 2 *(in questo caso il minimo)*.

**EQUAZIONE DI RICORRENZA**

$d_{i, j}^{(k, l)}$ = min { *e<sub>1</sub>*, *e<sub>2</sub>* }

**SOLUZIONE**
La soluzione del problema originale sarà in $D^{(n,\space L)}$.



### R ARCHI ROSSI

Dato un grafo G = (V, E, W, col) senza cappi dove col: E $\rightarrow$ {Red, Blue} calcolare $\forall$ (i, j) $\in$ V<sup>2</sup> il peso di un cammino minimo da *i* a *j* con esattamente 3 archi rossi. *(R = 3, n = |V|)*.

**INPUT**: G = (V, E, W, col)	R

**SOTTOPROBLEMA**: definito da k $\in$ {0, ..., n} e r $\in$ {0, ..., R}

$\forall$ (i, j) $\in$ V<sup>2</sup> calcolare il peso di un cammino minimo da *i* a *j* con vertici intermedi $\in$ {0, ..., k} con esattamente *r* archi rossi.

**DEFINIZIONE VARIABILI**: per ogni sottoproblema abbiamo $D^{(k, r)}$ = ($d_{i, j}^{(k, r)}$).

**CASO BASE** k = 0 $\land$ r $\in$ {0, ..., R}

**Caso 1**: r = 0

$d_{i, j}^{0, 0}$ = $\begin{cases} 0 &se \space i = j \\
 w_{i, j} & se \space i \neq j \land (i, j)
 \in E \land col(i, j) \neq Red \\ \infty &altrimenti \end{cases}$

**ATTENZIONE**: varrà infinito anche se l'arco esiste ma è di colore rosso in questo caso.

**Caso 2**: r = 1

$d_{i, j}^{0, 1}$ = $\begin{cases} \infty &se \space i = j \\
 w_{i, j} & se \space i \neq j \land (i, j)
 \in E \land col(i, j) = Red \\ \infty &altrimenti \end{cases}$

**ATTENZIONE**: Voglio un cammino che abbia 1 arco rosso; se *i* = *j* l'arco non c'è, di conseguenza non posso avere un cammino con 1 arco rosso.

**Caso 3**: r > 1

$d_{i, j}^{(0, r)}$ = $\infty$ 

**ATTENZIONE**: In questo caso viene sempre infinito poiché con 0 vertici intermedi posso avere al più un arco, di conseguenza non avrò mai un cammino con r > 1 archi rossi.

**CASO PASSO** k > 0 $\land$ r $\in$ {0, ..., R}

**Caso 1**: k $\notin$ cammino minimo
*e<sub>1</sub>* : $d_{i, j}^{(k, r)}$ = $d_{i, j}^{(k-1, r)}$

**Caso 2**: k $\in$ cammino minimo

*e<sub>2</sub>* : $d_{i, j}^{(k, r)}$ = $\min\limits_{(r_1,\space r_2) \space \in \space \{0, \space ..., \space r\}^2 \space : \space r_1 + r_2 = r} (d_{i,k}^{(k-1, r_1)} + d_{k, j}^{(k-1, r_2)})$

**EQUAZIONE DI RICORRENZA**

$d_{i, j}^{(k, r)}$ = min{ *e<sub>1</sub>, e<sub>2</sub>* }

Un ulteriore caso base potrebbe essere se r > k + 1, in quel caso sicuramente non posso avere r archi rossi, ma possiamo anche lasciare così le equazioni.

**SOLUZIONE**
La soluzione del problema originale sarà in $D^{(n,\space R)}$.



### ESISTE CAMMINO CON COLORI ALTERNATI

Dato un grafo orientato G = (V, E, col) senza cappi dove col: E $\rightarrow$ {Red, Blue} stabilire $\forall$ (i, j) $\in$ V<sup>2</sup> se $\exist$ un cammino da *i* a *j* nel quale non vi siano 2 archi consecutivi Red.

**INPUT**: G = (V, E, col)

**SOTTOPROBLEMA**: definito da k $\in$ {0, ..., n}

**DEFINIZIONE VARIABILI**: Per ogni sotto problema abbiamo $D^{(k)}$ = ($d_{i, j}^{(k)}$) dove $d_{i, j}^{(k)}$ vale TRUE sse $\exist$ un cammino da *i* a *j* senza 2 archi rossi consecutivi con vertici intermedi $\in$ {0, ..., k}.

Notiamo che per poter dire se possiamo unire due cammini *(nel caso in cui k $\in$ cammino minimo ad esempio)* dobbiamo sapere con che archi iniziano e finiscono i vari cammini; ci ***manca informazione***!
La soluzione è quindi introdurre un **problema ausiliario** leggermente modificato;
Dato un grafo orientato G = (V, E, col) senza cappi dove col: E $\rightarrow$ {Red, Blue} ***e data (a, b) $\in$ {Red, Blue}<sup>2</sup>*** stabilire ***$\forall$ (a, b) $\in$ {Red, Blue}<sup>2</sup>***, $\forall$ (i, j) $\in$ V<sup>2</sup> se $\exist$ un cammino da *i* a *j* nel quale non vi siano 2 archi consecutivi Red ***e con colore del primo arco = a e colore dell'ultimo arco = b***.

**SOTTOPROBLEMA AUSILIARIO**: definito da k $\in$ {0, ..., n} e da a, b $\in$ {Red, Blue}

**DEFINIZIONE VARIABILI**: Per ogni sotto problema abbiamo $D^{(k, \space a,\space b)}$ = ($d_{i, j}^{(k, \space a, \space b)}$) dove $d_{i, j}^{(k, \space a, \space b)}$ vale TRUE sse $\exist$ un cammino da *i* a *j* senza 2 archi rossi consecutivi con vertici intermedi $\in$ {0, ..., k} e con colore primo arco = a e con colore ultimo arco = b.

**CASO BASE**: k = 0

$\forall (a,b) \in \{R, B\}^2 \space \space : \space \space d_{i, j}^{(0, \space a, \space b)}$ = $\begin{cases} FALSE &se \space i = j \\
TRUE &se  \space i \neq j \land \space (i, j) \in E \land col(i,j) = a \land col(i,j) = b \\
FALSE &altrimenti
\end{cases}$

**CASO PASSO**: k > 0 $\forall$ (a, b)

**Caso 1**: k $\notin$ cammino minimo

*e<sub>1</sub>* = $d_{i, j}^{(k, \space a, \space b)}$ = $d_{i, j}^{(k - 1, \space a, \space b)}$

**Caso 2**: k $\in$ cammino minimo

*e<sub>2</sub>* = $d_{i, j}^{(k, \space a, \space b)}$ = $\lor_{(c, d) \in \{R, B\}^2 \space : \space (c, d) \neq \{R, R\}}$ ( $d_{i, j}^{(k - 1, \space a, \space c)} \land d_{i, j}^{(k - 1, \space d, \space b)}$ )

**EQUAZIONE DI RICORRENZA**:

$d_{i, j}^{(k, \space a, \space b)}$ = *e<sub>1</sub>* $\lor$ *e<sub>2</sub>*

**SOLUZIONE PB AUX**: $\forall$ (a, b) la soluzione del PB AUX definito da (a, b) è $D^{(n, \space a,\space b)}$

**SOLUZIONE PB DATO**: $D_{i, j} = \lor_{(a, b) \in \{R, B\}^2} (\space d_{i, j}^{(n, \space a, \space b)} \space)$

**ATTENZIONE**: Da notare come nella soluzione ***non*** escludiamo le coppie *{R, R}* come facciamo nel caso passo; in quel caso lo facciamo perché se unissimo un cammino che finisce col rosso e uno che inizia col rosso avremmo due archi rossi consecutivi, nella soluzione finale non ci importa se il cammino minimo inizi e finisca col rosso, basta che non ce ne siano di consecutivi!



## ESERCIZI HOMEWORK - NON SVOLTI A LEZIONE!

### LUNGHEZZA ESATTAMENTE L

Dato un grafo G = (V, E, W) pesato, orientato e senza cappi e dato un intero L > 0, calcolare $\forall$ (i, j) $\in$ V<sup>2</sup> il peso di un cammino minimo da *i* a *j* di lunghezza esattamente L

**INPUT**: G = (V, E, W)	L

**SOTTO PROBLEMA**: definito da k $\in$ {0, ..., n} e da l $\in$ {0, ..., L}

$\forall$ (i, j) $\in$ V<sup>2</sup> calcolare il peso di un cammino minimo da *i* a *j*

- utilizzando vertici appartenenti a {0, ..., k}
- di lunghezza esattamente l

**DEFINIZIONE VARIABILI**
Per ogni sotto problema avremo quindi una variabile $D_{i, j}^{(k, l)}$ = ($d_{i, j}^{(k, l)}$) dove $\forall$ (i, j) $d_{i, j}^{(k, l)}$ è il peso del cammino minimo da *i* a *j* con vertici intermedi $\in$ {0, ..., k} di lunghezza esattamente l.

**CASO BASE**: k = 0

$d_{i, j}^{(0, \space l)}$ = $\begin{cases}
\infty &se \space i=j \\
w_{i, j} &se \space i\neq j \land (i, j) \in E \space \land l=1 \\
\infty &altrimenti
\end{cases}$

**CASO PASSO**: k > 0

**Caso 1:** k $\notin$ cammino minimo

*e<sub>1</sub>* : $d_{i, j}^{(k, \space l)}$ = $d_{i, j}^{(k - 1, \space l)}$

**Caso 2:** k $\in$ cammino minimo

*e<sub>2</sub>* : $d_{i, j}^{(k, l)}$ = $\min \limits_{l_1, l_2 \in \{0, \space ..., \space L\} \space : \space l_1 + l_2 = l } \{d_{i, k}^{(k - 1, \space l_1)} + d_{k, j}^{(k-1, \space l_2)} \}$

In questo caso se l = 1 l'algoritmo arriverebbe al caso base con $d_{i, j}^{(0, 1)}$ e sceglierebbe il peso o $\infty$ a seconda del fatto se *i* e *j* siano collegati da un arco o meno.

**EQUAZIONE DI RICORRENZA**: min { *e<sub>1</sub>*, *e<sub>2</sub>* }

**SOLUZIONE**
La soluzione del problema originale sarà in $D^{(n,\space L)}$.

### VARIANTE ESISTENZIALE

**CASO BASE**: k = 0

$d_{i, j}^{(0, \space l)}$ = $\begin{cases}
FALSE &se \space i=j \\
TRUE &se \space i\neq j \land (i, j) \in E \space \land l=1 \\
FALSE &altrimenti
\end{cases}$

**CASO PASSO**: k > 0

**Caso 1:** k $\notin$ cammino minimo

*e<sub>1</sub>* : $d_{i, j}^{(k, \space l)}$ = $d_{i, j}^{(k - 1, \space l)}$

**Caso 2:** k $\in$ cammino minimo

*e<sub>2</sub>* : $d_{i, j}^{(k, l)}$ = $\lor_{l_1, l_2 \in \{0, \space ..., \space L\} \space : \space l_1 + l_2 = l } \{d_{i, k}^{(k - 1, \space l_1)} \land d_{k, j}^{(k-1, \space l_2)} \}$

**SOLUZIONE**
La soluzione del problema originale sarà in $D^{(n,\space L)}$.



### $\leq$ R ARCHI ROSSI

Dato un grafo G = (V, E, W, col) senza cappi dove col: E $\rightarrow$ {Red, Blue} calcolare $\forall$ (i, j) $\in$ V<sup>2</sup> il peso di un cammino minimo da *i* a *j* con un numero $\leq$ 3 archi rossi. *(R = 3, n = |V|)*.

**INPUT**: G = (V, E, W, col)	R

**SOTTOPROBLEMA**: definito da k $\in$ {0, ..., n} e r $\in$ {0, ..., R}

$\forall$ (i, j) $\in$ V<sup>2</sup> calcolare il peso di un cammino minimo da *i* a *j* con vertici intermedi $\in$ {0, ..., k} con un numero $\leq$ *r* di archi rossi.

**DEFINIZIONE VARIABILI**: per ogni sotto problema abbiamo $D^{(k, r)}$ = ($d_{i, j}^{(k, r)}$).

**CASO BASE**: k = 0

$d_{i, j}^{(0, \space r)}$ = $\begin{cases} 
0 &se \space i=j \\
w_{i, j} &se \space i \neq j \land (i, j) \in E \land r > 0 \\
\infty &altrimenti\end{cases}$

**CASO PASSO**: k > 0

**Caso 1**: k $\notin$ cammino minimo

$d_{i, j}^{(k, \space r)}$ = $d_{i, j}^{(k-1, \space r)}$

**Caso 2**: k $\in$ cammino minimo

$d_{i, j}^{(k, \space r)}$ = $\min\limits_{(r_1, r_2) \in \{0, ..., R\}^2 \space : \space r_1 + r_2 \leq r} \{d_{i, k}^{(k-1, \space r_1)} + d_{k, j}^{(k-1, \space r_2)} \}$

**SOLUZIONE**
La soluzione del problema originale sarà in $D^{(n,\space R)}$.

### VARIANTE ESISTENZIALE

**CASO BASE**: k = 0

$d_{i, j}^{(0, \space r)}$ = $\begin{cases} 
TRUE &se \space i=j \\
TRUE &se \space i \neq j \land (i, j) \in E \land r > 0 \\
FALSE &altrimenti\end{cases}$

**CASO PASSO**: k > 0

**Caso 1**: k $\notin$ cammino minimo

$d_{i, j}^{(k, \space r)}$ = $d_{i, j}^{(k-1, \space r)}$

**Caso 2**: k $\in$ cammino minimo

$d_{i, j}^{(k, \space r)}$ = $\lor_{(r_1, r_2) \in \{0, ..., R\}^2 \space : \space r_1 + r_2 \leq r} \{d_{i, k}^{(k-1, \space r_1)} \land d_{k, j}^{(k-1, \space r_2)} \}$

**SOLUZIONE**
La soluzione del problema originale sarà in $D^{(n,\space R)}$.