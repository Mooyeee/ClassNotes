# RIPASSO

Fissata una funzione *g(n)* possiamo dire:

O(g(n)) = { f(n) | $\exist$ c $\in$ $\mathbb{R}$, n<sub>0</sub> $\in$ $\mathbb{N}$		  :	$\forall$ n $\geq$ n<sub>0</sub>	f(n) $\leq$ c * g(n) }

Una funzione **f(n)** è detta *O grande di g(n)* se esiste una costante reale *c* tale che, per ogni punto successivo ad un certo punto n<sub>0</sub>, **f(n)** assume valori minori o uguali ai valori assunti da *g(n) moltiplicata per c*.



$\Omega$(g(n)) = { f(n) | $\exist$ c $\in$ $\mathbb{R}$, n<sub>0</sub> $\in$ $\mathbb{N}$		  :	$\forall$ n $\geq$ n<sub>0</sub>	c * g(n) $\leq$ f(n) }

Una funzione **f(n)** è detta *Omega grande di g(n)* se esiste una costante reale *c* tale che, per ogni punto successivo ad un certo punto n<sub>0</sub>, **f(n)** assume valori maggiori o uguali ai valori assunti da *g(n) moltiplicata per c*.



*E ancora*
$\Theta$(g(n)) = { f(n) | $\exist$ c<sub>1</sub>, c<sub>2</sub> $\in$ $\mathbb{R}$, n<sub>0</sub> $\in$ $\mathbb{N}$	:	$\forall$ n $\geq$ n<sub>0</sub>	c<sub>1</sub> * g(n) $\leq$ f(n) $\leq$ c<sub>2</sub> * g(n) } 

Una funzione **f(n)** è detta *Theta di g(n)* se esistono due costanti reali c<sub>1</sub> e c<sub>2</sub> tali che, per ogni punto successivo ad un certo punto n<sub>0</sub>, **f(n)** assume contemporaneamente

- valori maggiori o uguali ai valori assunti da *g(n) moltiplicata per c<sub>1</sub>*
- valori minori o uguali ai valori assunti da *g(n) moltiplicata per c<sub>2</sub>*

In parole povere, una funzione **f(n)** è detta *Theta di g(n)* se **f(n)** è contemporaneamente sia *Omega grande di g(n)* che *O grande di g(n)*.



# PROGRAMMAZIONE DINAMICA

Abbiamo visto nel corso precedente come spesso cerchiamo la soluzione migliore di un algoritmo provando sia una soluzione iterativa che una ricorsiva, ma che facciamo quando nessuna di queste due da una soluzione accettabile?
L'idea su cui si basa la programmazione dinamica è evitare di calcolare più volte lo stesso risultato *(come accade a volte negli algoritmi ricorsivi, ad esempio con Fibonacci)* calcolandolo una sola volta per poi memorizzarlo per quando sarà necessario nuovamente.



## WEIGHTED INTERVAL SCHEDULING

Consideriamo un insieme di n attività
{ 1, ..., n}	n $\in$ $\mathbb{N}$	$\forall$ attività i $\in$ { 1, ..., n } abbiamo $\begin{cases}s_{i} & \text{tempo inizio attività i} \\ f_{i} & \text{tempo fine attività i} \\ v_{i} & \text{valore/peso attività i} \end{cases}$

Consideriamo due attività *i* e *j* compatibili se queste non si sovrappongono, ovvero se **[ s<sub>i</sub>, f<sub>i</sub> ) $\cap$ [ s<sub>j</sub>, f<sub>j</sub> ) = $\empty$**

Definiamo le seguenti funzioni:

1. C : $\wp$( {1, ..., n} ) $\rightarrow$ { true, false }
   $\forall$ A $\subseteq$ {1, ..., n}     C(A) = $\begin{cases} true & \text{se A contiene attività fra loro mutualmente compatibili} \\ false & \text{altrimenti} \end{cases}$
2. $\mathcal{v}$ : $\wp$( {1, ..., n} ) $\rightarrow$ $\mathbb{R}$
   $\forall$ A $\subseteq$ {1, ..., n}     $\mathcal{v}$(A) = $\begin{cases} \sum_{i \in A} v_{i} & \text{se A} \neq \empty \\ 0 & \text{se A} = \empty \end{cases}$

Il problema WIS ci richiede di trovare, fra tutti i sottoinsiemi di attività mutualmente compatibili, quello con valore $\mathcal{v}$ massimo *(è dunque un problema di ottimizzazione)*.

**ISTANZA**
Un'istanza di tale problema è data dal numero di attività *n* e dalle triple associate ad ognuna delle *n* attività.

**SOLUZIONE:** S $\subseteq$ {1, ..., n} t.c. C(S) = true $\land$ $\mathcal{v}$(S) = max{ $\mathcal{v}$(A) } con A $\subseteq$ {1, ..., n}

Per risolvere un problema di programmazione dinamica dobbiamo innanzitutto individuare i sotto problemi più piccoli. Il generico sotto problema di WIS, identificato da *i : 0 $\leq$ i $\leq$ n* è nella forma:

**Dato X<sub>i</sub> = {1, ..., i} trovare S<sub>i</sub> $\subseteq$ X<sub>i</sub> t.c. C(S<sub>i</sub>) = ture $\land$ $\mathcal{v}$(S<sub>i</sub>) = max{ $\mathcal{v}$(A) } con A $\subseteq$ X<sub>i</sub>**

Assumiamo ora di aver già risolto tutti i sotto problemi più piccoli di *i* e cerchiamo di capire come calcolare S<sub>i</sub> e $\mathcal{v}$(S<sub>i</sub>).

Assumiamo anche che le attività siano ordinate per tempo di fine e definiamo
***p(i) = max{ j | j < i $\land$ j è compatibile con i }***

*Esempio:*
1		——												
2	—————										 
3				———									 
4						————						 
5				————————				 
6										———			 

$\mathcal{v}$<sub>i</sub>	   =	10, 2, 8, 1, 1, 3
p(i)	=	  0, 0, 1, 2, 1, 4

Per calcolare S<sub>i</sub> avremo quindi due casi:

1. i $\in$ S<sub>i</sub>     allora     S<sub>i</sub> = S<sub>p(i)</sub> $\cup$ { i }
   $\mathcal{v}$(S<sub>i</sub>) =  $\mathcal{v}$(S<sub>i</sub>) + $\mathcal{v}$<sub>i</sub>
2. i $\notin$ S<sub>i</sub>     allora     S<sub>i</sub> = S<sub>i - 1</sub>
   $\mathcal{v}$(S<sub>i</sub>) = $\mathcal{v}$(S<sub>i - 1</sub>)

Possiamo ora definire le equazioni di ricorrenza.


#### CASO BASE

i = 0
X<sub>0</sub> = $\empty$	$\implies$	S<sub>0</sub> = $\empty$ e $\mathcal{v}$(S<sub>0</sub>) = 0

#### CASO PASSO

i > 0
$\mathcal{v}$(S<sub>i</sub>) = max{ $\mathcal{v}$(S<sub>p(i)</sub>) ; $\mathcal{v}$(S<sub>i - 1</sub>) }

S<sub>i</sub> = $\begin{cases} S_{p(i)} \space \cup \space \{i\} & \text{se } v(S_{p(i)}) \space + \space v_{i} \geq v(S_{i - 1}) \\ S_{i-1} & \text{altrimenti} \end{cases}$

Detto questo, è facile creare un algoritmo ricorsivo basato su queste equazioni, il problema di tale algoritmo però è che finirebbe per calcolare più volte lo stesso risultato portando il tempo ad essere esponenziale. La programmazione dinamica ci permette di memorizzare i risultati parziali dei sotto problemi più piccoli in modo da riutilizzarli senza doverli calcolarli nuovamente.

#### DEFINIZIONE VARIABILI

Definiamo quindi M[i] $\forall$ i t.c. 0 $\leq$ i $\leq$ n come la soluzione del sotto problema i-esimo, ovvero come il massimo valore che posso ottenere prendendo un insieme di attività mutualmente incompatibili considerando solo le prime *i* attività.

#### ALGORITMO

Scriviamo quindi l'algoritmo WIS definito come

WIS(){
	M[0] = 0
	for i = 0 to n
		M[i] = max(M[p[i]] + v[i], M[i-1])
	return M
}

Nell'esempio fatto prima l'algoritmo farebbe i seguenti passaggi
M[1] = max(M[0] + 10, M[0])
M[2] = max(M[0] + 2, M[1])
M[3] = max(M[1] + 8, M[2])
M[4] = max(M[2] + 1, M[3])
M[5] = max(M[1] + 1, M[4])
M[6] = max(M[4] + 3, M[5])

**M**

|  0   |  1   |  2   |  3   |  4   |  5   |  6   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  0   |  10  |  10  |  18  |  18  |  18  |  21  |

Possiamo poi stampare gli intervalli attraverso il seguente algoritmo

PrintW(i)
	if i != 0
		if v\[i\] + M\[p\[i\]\] > M\[i - 1\]
			print i
			PrintW(p\[i\])
		else
			PrintW(i - 1)


## LONGEST COMMON SUBSEQUENCE

Per alcuni esercizi è possibile trovare immediatamente una soluzione in programmazione dinamica, mentre per altri bisogna risolvere un problema associato che ci possa consentire di arrivare alla soluzione.


### SEQUENZE
Definiamo **X = <x<sub>1</sub>, ..., x<sub>n</sub>>** una sequenza.
Una sottosequenza **Z = <z<sub>1</sub>, ..., z<sub>n</sub>>** con k $\leq$ n è una sequenza costituita dagli indici presi in ordine strettamente crescente dei caratteri di X.

*Esempio:*
X = < A, C, D, B, A >

Z<sub>0</sub> = < A, B> = < 1, 4 > è una sottosequenza di X
Z<sub>1</sub> = < A, B, B, A > = < 1, 4, 4, 5 > non è una sottosequenza di X, in quanto X non contiene due B
Z<sub>2</sub> = < A, B, C, D > = < 1, 4, 2, 3 > non è una sottosequenza di X, in quanto gli indici non sono presi in ordine strettamente crescente.

Consideriamo ora due sequenze X = <x<sub>1</sub>, ..., x<sub>n</sub>> e Y = <y<sub>1</sub>, ..., y<sub>m</sub>>;
diremo che Z è ***sottosequenza comune*** se è contemporaneamente sottosequenza di X e di Y.

*Esempio:*
X = < A, C, D, B >		Y = < D, C, B, A >

Z<sub>0</sub> = < C, B > è sottosequenza comune a X e Y.


### LCS

Il problema della LCS vuole trovare una sottosequenza comune a due sequenze date che sia la più lunga possibile *(dunque anche questo è un problema di ottimizzazione)*.

Proviamo a fare un ragionamento di tipo ricorsivo

Confrontiamo, nel caso generico, x<sub>i</sub> con y<sub>j</sub>, caratteri i-esimo e j-esimo delle sequenze X e Y

- Se x<sub>i</sub> = y<sub>j</sub>     LCS( < x<sub>1</sub>, ..., x<sub>i - 1</sub> >,   < y<sub>1</sub>, ..., y<sub>j - 1</sub> > ) + 1
  *se i caratteri sono uguali è ovvio che mi conviene considerarli come facenti parte di Z* 
- Se x<sub>i</sub> $\neq$ y<sub>j</sub>     max{ LCS( < x<sub>1</sub>, ..., x<sub>i - 1</sub> >,   < y<sub>1</sub>, ..., y<sub>j</sub> > ), LCS( < x<sub>1</sub>, ..., x<sub>i</sub> >,   < y<sub>1</sub>, ..., y<sub>j - 1</sub> > ) }
  *se sono diversi, cerco di capire quale dei due caratteri mi conviene scartare*

Definiamo quindi il problema:
**ISTANZA:** X = <x<sub>1</sub>, ..., x<sub>n</sub>>, Y = <y<sub>1</sub>, ..., y<sub>m</sub>>

**SOLUZIONE:** La sottosequenza comune ad X e Y che sia la più lunga possibile.

#### SOTTOPROBLEMI

Cerchiamo di suddividere il problema in sottoproblemi di taglia minore, definendo quindi il generico sottoproblema (i, j) come tale: la lunghezza della più lunga sottosequenza comune considerando solo i primi *i* caratteri di X ed i primi *j* caratteri di Y.
*Avremo quindi (m + 1)(n + 1) sottoproblemi.*

#### DEFINIZIONE VARIABILI

Definiamo M\[i\]\[j\] come la soluzione al sottoproblema di taglia (i, j), ovvero come la lunghezza massima della LCS che riesco ad ottenere considerando solo i primi *i* caretteri di X ed i primi *j* caratteri di Y.
Abbiamo detto di avere (n + 1)(m + 1) sottoproblemi, di conseguenza la dimensione della nostra matrice sarà M\[n + 1\]\[m + 1\].

#### CASO BASE

Se i = 0 $\lor$ j = 0
LCS(x<sub>i</sub>, y<sub>j</sub>) = 0

Ho in totale (m + n + 1) casi base.


#### CASO PASSO

Se i > 0 $\land$ j > 0

LCS(x<sub>i</sub>, y<sub>j</sub>) = $\begin{cases} LCS(x_{i-1},\space y_{j-1}) + 1 & se  \space x_{i}  = y_{j} \\ max\{LCS(x_{i-1}, y_{j}),\space LCS(x_{i}, y_{j-1})\} & se \space x_{i} \neq y_{j} \end{cases}$

**SOLUZIONE:** La soluzione *(parziale)* la troveremo nella cella M\[n\]\[m\]
La soluzione è parziale nel caso ci interessi sapere anche quale sia la LCS oltre alla sua lunghezza, in tal caso possiamo o salvarci in un'altra matrice (n + 1)(m + 1) gli spostamenti fatti nella prima matrice (se il risultato (i,j) proviene dalla diagonale o da riga/colonna) per poi usarla per sapere quali caratteri abbiamo usato, o ripercorrere la matrice originale per trovare i simboli (operazione più costosa in termini computazionali).

#### ALGORITMO

LCS(X, Y) {
*CASO BASE*
	for i = 0 to n
		M\[i\]\[0\] = 0
	for j = 0 to m
		M\[0\]\[j\] = 0

*CASO PASSO*
​	for i = 1 to n
​		for j = 1 to m
​			if x\[i\] == y\[j\]
​				M\[i\]\[j\] = M\[i - 1\]\[j - 1\] + 1
​			else
​			M\[i\]\[j\] = max( M\[i\]\[j - 1\], M\[i - 1\]\[j\] )

​	return M\[n\]\[m\]
}

**TEMPO E SPAZIO**

Il tempo di calcolo T(n, m) sarà
T(n, m) = (n + m) + (n * m) = $\Theta$(n * m)
Se includessimo anche la risalita per trovare la sequenza il tempo asintomatico non cambierebbe *( ... + (n + m))*.

Lo spazio S(n, m) sarà
S(n, m) = (n * m) = $\Theta$(n * m)
Se includessimo anche la seconda matrice lo spazio asintotico non cambierebbe *(... + (n \* m))*.
Notiamo però come l'algoritmo usi sempre solo due righe della colonna per eseguire i suoi calcoli, se ci interessasse solo la lunghezza della LCS potremmo ridurre lo spazio a solo due righe rendendolo lineare.



## LONGEST INCREASING SUBSEQUENCE

Abbiamo visto come identificare una sottosequenza comune a due sequenze che sia la più lunga possibile possa essere non immediato, ma concentriamoci ora su una sola sequenza: una sua sottosequenza è un qualuque suo carattere e la più lunga è, banalmente, la sequenza stessa.
Ma non appena introduciamo ulteriori vincoli il problema può diventare complesso anche considerando una sola sequenza.

Il problema LIS consiste nel trovare una sottosequenza della sequenza X che sia la più lunga sottosequenza possibile prendendo solo ***caratteri strettamente crescenti*** *(quindi ad esempio < A, C, B, D > non va bene perché C > B)*.
Il classico approccio è provare tutte le possibili combinazioni, ma questo richiederebbe un tempo troppo elevato.


Definiamo quindi il problema come segue:
**ISTANZA:** X = <x<sub>1<sub>, ..., 	x<sub>n</sub>>

**SOLUZIONE:** La più lunga sottosequenza di X strettamente crescente.

#### SOTTOPROBLEMI

Cerchiamo quindi i sottoproblemi avendo in input una sequenza **X = <x<sub>1</sub>, ..., x<sub>2</sub>>**;
Il sottoproblema di taglia *i* è: trovare la più lunga sottosequenza strettamente crescente che termini con l'*i*-esimo carattere di X.

È importante notare come, a differenza del problema LCS, nel LIS, per dar risposta ai sottoproblemi, dobbiamo considerare tutti i risultati già calcolati; difatti se aggiungessi dei caratteri ad X potrei migliorare una LIS che magari prima non era ottimale e farla diventare la più lunga.

Ragionando allora potremo segnarci, per ogni carattere, quale sia la più lunga sottosequenza strettamente crescente che riusciamo a trovare che finisca con quello stesso carattere.
Avremo quindi che per il primo carattere il massimo è 1, in quanto il sottoproblema di taglia 1 considera solo un carattere, mentre, ipotizzando di aver già risolto i sottoproblemi più piccoli, ci basta vedere quale sia la massima lunghezza di una LIS compatibile con il carattere x<sub>i</sub> *(e quindi che sia minore di x<sub>i</sub>)* e aggiungerci 1 *(relativo al carattere x<sub>i</sub>)*.



#### DEFINIZIONE VARIABILI

Definiamo L\[i\] come la soluzione al sottoproblema di taglia *i*, ovvero come la lunghezza della più lunga sottosequenza strettamente crescente che termini con l'*i*-esimo carattere di X.
**Attenzione:** considero solo la sottosequenza più lunga che ***termini*** con il carattere *i*-esimo, perché devo controllare quale sia il miglior risultato che posso ottenere in ogni posizione, questo perché non so quali siano i prossimi caratteri e come possano influenzare il mio ottimo, potenzialmente cambiandolo totalmente *(mentre nella LCS i risultati successivi potevano solo incrementare l'ottimo già trovato)*.

#### CASO BASE

i = 1
L[1] = 1	*il massimo che posso fare con una sottosequenza di X che termini col suo primo carattere è certamente 1*
Ho dunque un solo caso base.

#### CASO PASSO

1 < i $\leq$ n
L[i] = 1 + max{ L[j] | 0 $\leq$ j $\lt$ i	|	x<sub>j</sub> $\lt$ x<sub>i</sub> }

**SOLUZIONE:** max{ L[i] | 0 $\le$ i $\leq$ n }
Questa volta la soluzione non la troveremo sempre nella stessa posizione come nella LCS, ma ci sarà data dal massimo tra le soluzione ai sottoproblemi calcolati.

#### ALGORITMO

LIS(X) {
*CASO BASE*
		L\[1\] = 1

*CASO PASSO*
	for i = 2 to n
		max = 0
		for j = 0 to i
			if L\[j\] > max AND X\[j\] < X\[i\]
				max = L\[j\]

​		L\[i\] = max + 1

​	return max(L)
}

**TEMPO E SPAZIO**

Il tempo dell'algoritmo è facilmente calcolabile ed è pari a **T(n) = $\Theta$(n<sup>2</sup>)**.
Per evitare di scansionare nuovamente l'array per trovare il massimo totale, potremmo calcolarlo mentre costruiamo l'array controllando ad ogni fine ciclo interno se L\[i\] sia maggiore del massimo totale trovato fino ad allora, e nel caso aggiornarlo.

Lo spazio è dato dall'array L ed è pari a **S(n) = $\Theta$(n)**.

Per risolvere il problema originale *(fornire la sottosequenza)*, potremmo ripercorrere l'array L all'indietro controllando, ogni volta che troviamo un valore minore di quello analizzato, se il carattere nella posizione di tale valore sia compatibile con quello nella posizione che stiamo analizzando.
Si potrebbe anche salvare in un altro array S di dimensione n quale carattere ha determinato il massimo nella posizione i, a seconda di cosa abbiamo bisogno di ottimizzare, spazio o tempo.



## GROWING LCS

Abbiamo fino ad ora visto i problemi della LCS e della LIS e ci chiediamo ora se sia possibile stabilire, tra due sequenze ***X = <x<sub>1</sub>, ..., x<sub>n</sub>>*** e ***Y = <y<sub>1</sub>, ..., y<sub>n</sub>>*** una *più lunga* sottosequenza comune che sia anche *crescente*, combinando quindi i due problemi precedenti.

Cerchiamo ora di definire il valore di un generico sottoproblema ipotizzando di aver già risolto tutti i sottoproblemi di taglia minore, ovvero i problemi di taglia *(h, k)* con:
0 $\leq$ h $\lt$ n		e		0 $\leq$ k $\lt$ m
Consideriamo **Z<sub>h, k</sub>** come la soluzione del problema *(h, k)*, se la conoscessi potrei decidere se aggiungere o meno il carattere, purtroppo però non so come sia fata **Z<sub>h, k</sub>** perché mi manca dell'informazione.

#### PROBLEMA AUSILIARIO
Introduciamo quindi un problema ausiliario che ci permetta di ricostruire poi la soluzione del problema originale formulato come segue:
Date due sequenze X e Y, la soluzione è data dalla lunghezza della più lunga sottosequenza comune crescente che termini con x<sub>n</sub> = y<sub>m</sub>.

**ISTANZA:** X = <x<sub>1</sub>, ..., x<sub>n</sub>>			Y = <y<sub>1</sub>, ..., y<sub>m</sub>>
**SOLUZIONE:** Lunghezza della più lunga sottosequenza comune e strettamente crescente di X e Y che ***termini con x<sub>n</sub> e y<sub>m</sub> se questi coincidono***



#### SOTTOPROBLEMI

Identifichiamo ora il sottoproblema generico del nostro problema ausiliario denotandolo con *(i, j)* definito come tale:
Date X e Y, determinare la lunghezza della più lunga sottosequenza crescente che termini coi caratteri **x<sub>i</sub>** e **y<sub>j</sub>** se questi coincidono.

Avremo quindi in totale **(n + 1)(m + 1)** sottoproblemi.



#### DEFINIZIONE VARIABILI

Definiamo quindi **C<sub>i, j</sub>** come la soluzione al sottoproblema di taglia *(i, j)*, ovvero come la lunghezza della più lunga sottosequenza comune ad X e Y che termini con i caratteri x<sub>i</sub> e y<sub>j</sub> se questi coincidono.
Avendo (n + 1)(m + 1) sottoproblemi, C sarà definita come una matrice di dimensione **(n + 1)(m + 1)**.

Ora, cerchiamo di calcolare C<sub>i, j</sub> con i, j $\gt$ 0 assumendo di aver già risolto i sottoproblemi di taglia minore,
ovvero quelli identificati da *(h, k)* con	0 $\leq$ h $\lt$ i		e		0 $\leq$ k $\lt$ j:

Avremo dunque che ogni qual volta le x<sub>i</sub> e y<sub>j</sub> considerati siano diverse, la risposta al sottoproblema è 0
*(poiché non esiste una sottosequenza comune che termini sia con x<sub>i</sub> che con y<sub>j</sub>)*

Mentre quando sono uguali, le aggiungo al meglio che sono riuscito ad ottenere i sottoproblemi precedenti.


#### CASO BASE

$\forall$ (i, j)	con	x<sub>i</sub> $\neq$ y<sub>j</sub>	C<sub>i, j</sub> = 0
*In questo caso il numero di casi base dunque dipende dall'input.*



#### CASO PASSO

$\forall$ (i, j)	con	x<sub>i</sub> = y<sub>j</sub>
C<sub>i, j</sub> = 1 + max{ C<sub>h, k</sub>	|	0 $\leq$ h $\lt$ i		e		0 $\leq$ k $\lt$ j	|	x<sub>h</sub> $\lt$ x<sub>i</sub> }		*con max($\empty$) = 0*
*(potrei aggiungere 'AND y<sub>k</sub> $\lt$ y<sub>j</sub>' alla condizione, ma sarebbe ridondante in quanto x<sub>i</sub> = y<sub>j</sub>)*

**SOLUZIONE PROBLEMA ORIGINALE:** max{C<sub>i, j</sub>} con 1 $\leq$ i $\leq$ n	e	1 $\leq$ j $\leq$ m

#### ALGORITMO

C\[N\]\[M\]{
	for i = 0 to n
		for j = 0 to m
			if X\[i\] != Y\[j\]
				C\[i\]\[j\] = 0			*CASO BASE*
			else
				max = 0			*CASO PASSO*
				for h = 0 to i - 1
					for k = 0 to j -1
						if X\[h\] < X\[i\] AND C\[h\]\[k\] > max
							max = C\[h\]\[k\]
					
					C\[i\]\[j\] = 1 + max
	
	return Max(C)
}

**TEMPO E SPAZIO**

Si può subito notare come l'algoritmo ci impieghi **T(n, m) = $\Theta$(m<sup>2</sup> * n<sup>2</sup>)**
Mentre lo spazio è pari a **S(n, m) = $\Theta$(m * n)**

Potremmo ridurre un po' il tempo *(anche se quello asintotico non cambierebbe)* calcolando il massimo totale mentre costruiamo la matrice invece che riscansionarla alla fine per trovare il massimo.













## LCS LUNGHEZZA $\leq$ L

**ISTANZA**
X = <x<sub>1</sub>, x<sub>2</sub> ... x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... y<sub>n</sub>> n
L $\in$ $\mathbb{N}$

**SOLUZIONE:** true SSE $\exist$ una LCS(X, Y) di lunghezza $\geq$ L

sotto problema: (i, j, l)	con i $\in$ {0, 1, ... m},	j $\in$ {0, 1, ... n},	l $\in$ {0, 1, ... L}
*istanza X<sub>i</sub> = <x<sub>1</sub> ... x<sub>i</sub>>	Y<sub>j</sub> = <y<sub>1</sub> ... y<sub>j</sub>> l*
*soluzione true SSE $\exist$ LCS(X<sub>i</sub>, Y<sub>j)</sub> di lunghezza $\geq$ l*

In totale ho ***(m + 1)(n + 1)(l + 1)*** sotto problemi


#### DEFINIZIONE VARIABILI COINVOLTE

$\forall$ (i, j, l) con i $\in$ {0, 1, ... m} , j $\in$ {0, 1, ... n}, l $\in$ {0, 1, ... L}
***C<sub>i,j,l</sub>*** è definita come la soluzione del sotto problema (i, j, l) $\implies$ vale true SSE una LCS(Xi, Yi) con lunghezza $\geq$ l



#### CASO BASE

C<sub>i,j,l</sub> = $\begin{cases} True & \text{se l = 0 qualunque sia il valore di i e j} \\ False & \text{se l > 0 } \and \text{(i < l } \or \text{j < l)} \end{cases}$

***Quanti sono i casi base?***

- True: (i, j, 0) $\rightarrow$ (m+1)(n+1)
- False: (i, j, l) $\rightarrow$ homework



#### CASO RICORSIVO

(i, j, l) l > 0 $\and$ ( i $\geq$ l $\or$ j $\geq$ l )

Dato (i, j, l) assumo di aver già risolto i sotto problemi più piccoli di (i, j, l)

C<sub>i,j,l</sub> = $\begin{cases}
C_{i-1,j-1,l-1} & \text{se } x_{i} = y_{j} \\
C_{i-1,j,l-1} \or C_{i,j-1,l-1} & \text{se } x_{i} \neq y_{j}
\end{cases}$

***Soluzione problema di partenza = C[m]\[n]\[l]***









#### ALGORITMO

Algo(X, Y, L) {

​	*Caso base*
​	for i = 0 to m
​		for j = 0 to n
​			C[ i ]\[ j ]\[ l ] = True

​	*Caso ricorsivo*
​	for l = 1 to L
​		for i = 1 to m
​			for j = 0 to n
​				if (i < l) $\or$ (j < l)
​					C[ i ]\[ j ]\[ l ] = False
​				else
​					if x<sub>i</sub> = y<sub>j</sub>
​						C[ i ]\[ j ]\[ l ] = C[ i-1 ]\[ j-1 ]\[ l-1 ]
​					else
​						C[ i ]\[ j ]\[ l ] = (C[ i-1 ]\[ j ]\[ l ]) $\or$ (C[ i ]\[ j-1 ]\[ l ])
​	Return C[M]\[N]\[L];
}



## LUNGHEZZA LCS CON COLORI

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... ,x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... ,y<sub>n</sub>> n
K = 3
Ad ogni carattere di $\Sigma$ è associato il colore rosso o blu mediante *col $\Sigma$ $\rightarrow$ {R, B}*

**SOLUZIONE:** Lunghezza di LCS nella quale ci sono al massimo K caratteri a cui è associato il colore rosso

Sotto problema (i, j, k)	con i $\in$ {0, 1, ... m},	j $\in$ {0, 1, ... n},	k $\in$ {0, 1, ... K}
*istanza X<sub>i</sub> = <x<sub>1</sub> ... x<sub>i</sub>>	Y<sub>j</sub> = <y<sub>1</sub> ... y<sub>j</sub>>	k*
*soluzione: lunghezza di una LCS(x<sub>i</sub>, y<sub>j</sub>) nella quale vi sono al massimo k caratteri rossi*

#### DEFINIZIONE VARIBILI COINVOLTE

$\forall$ (i, j, l) con i $\in$ {0, 1, ... m} , j $\in$ {0, 1, ... n}, l $\in$ {0, 1, ... L}
***C<sub>i,j,k</sub>*** è definita come la soluzione del sotto problema (i, j, k) $\implies$ Lunghezza LCS con al massimo k rossi

<div style="page-break-after: always; break-after: page;"></div>

#### CASO BASE

Ci,j,k = 0		per i = 0 $\or$ j = 0	$\forall$ k $\in$ {0, ... K}

#### CASO RICORSIVO

(i, j, k)			i > 0 $\and$ j > 0 $\and$ $\forall$ k $\in$ {0, ... K}

C<sub>i,j,k</sub> = $\begin{cases}
1 + C_{i-1, \space j-1, \space k-1} & \text{Se } x_{i} = y_{j} \space \and Col(x_{i}) = Red \and k>0 \\
C_{i-1, \space j-1, \space k} & \text{Se } x_{i} = y_{j} \space \and Col(x_{i}) = Red \and k=0 \\
1 + C_{i-1, \space j-1, \space k} & \text{Se } x_{i} = y_{j} \space \and Col(x_{i}) = Blue \\
max(C_{i-1, \space j, \space k}, \space C_{i, \space j-1, \space k}) & \text{Se } x_{i} \neq y_{j}
\end{cases}$

***Soluzione problema di partenza = C[M]\[N]\[K]***

*Da fare: invece che "al massimo"*

- ***almeno*** *K rossi*



## ESATTAMENTE K

#### CASO BASE

Ci,j,k = 0		per (i = 0 $\or$ j = 0) $\and$ k = 0
Ci,j,k = -$\infty$ 	per (i = 0 $\or$ j = 0) $\and$ k > 0 *(Non c'è nessuna LCS con k rossi, nemmeno quella vuota)*
Dove -$\infty$ è un simbolo tale che qualsiasi cosa sommata ad esso dia sempre -$\infty$

*Tutto sto casino perché se prendo ad esempio X = \<A\> e Y = \<A\> con col(A) = Blue e K > 0, l'algoritmo di prima darebbe 1, mentre in questo caso la LCS con K rossi non esiste*

#### CASO RICORSIVO

(i, j, k)			i > 0 $\and$ j > 0 $\and$ $\forall$ k $\in$ {0, ... K}

C<sub>i,j,k</sub> = $\begin{cases}
1 + C_{i-1, \space j-1, \space k-1} & \text{Se } x_{i} = y_{j} \space \and Col(x_{i}) = Red \and k>0 \\
C_{i-1, \space j-1, \space k} & \text{Se } x_{i} = y_{j} \space \and Col(x_{i}) = Red \and k=0 \\
1 + C_{i-1, \space j-1, \space k} & \text{Se } x_{i} = y_{j} \space \and Col(x_{i}) = Blue \\
max(C_{i-1, \space j, \space k}, \space C_{i, \space j-1, \space k}) & \text{Se } x_{i} \neq y_{j}
\end{cases}$

<div style="page-break-after: always; break-after: page;"></div>

## SEQUENZA INTERLEAVING

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... y<sub>n</sub>> n
W = <w<sub>1</sub>, w<sub>2</sub> ... w<sub>m+n</sub>>

**SOLUZIONE:** True SSE W è interleaving di X e Y ossia se X e Y sono due sottosequenze disgiunte di W
*Esempio:* X = <*C, I, A, O*>	Y = <**M**, **A**, **M**, **M**, **A**>	W = <*C*, *I*, **M**, *A*, **A**, **M**, **M**, **A**, *O*>
*Devono pero essere disgiunte, una lettera non può essere parte di entrambe le sottosequenze*

Sotto problema
*istanza X<sub>i</sub> = <x<sub>1</sub> ... x<sub>i</sub>>	Y<sub>j</sub> = <y<sub>1</sub> ... y<sub>j</sub>>	W<sub>i+j</sub> = <w<sub>1</sub> ... w<sub>i+j</sub>*
*Soluzione: true sse W<sub>i+j</sub> è interleaving di X<sub>i</sub> e Y<sub>j</sub>*

#### DEFINIZIONE VARIBILI COINVOLTE

$\forall$ (i, j)	con i $\in$ {0, 1, ... m},	j $\in$ {0, 1, ... n}
C<sub>i, j</sub> è definita come la soluzione del sotto problema (i, j) true sse W<sub>i+j</sub> è interleaving di X<sub>i</sub> e Y<sub>j</sub>

#### CASO BASE

***Se i = 0 $\and$ j = 0***
C<sub>i, j</sub> = True 			*(e quindi W<sub>0 + 0</sub>)*

***Se i = 0 $\and$ j > 0***
C<sub>i, j</sub> = False			se w<sub>j</sub> $\neq$ y<sub>j</sub>  		*(ovvero se i = 0, W deve essere uguale a Y per far si che sia interleaving)*

***Se i > 0 $\and$ j = 0***
C<sub>i, j</sub> = False			se w<sub>i</sub> $\neq$ y<sub>i</sub>		  *(ovvero se j = 0, W deve essere uguale a X per far si che sia interleaving)*

***Se i > 0 $\and$ j > 0***
C<sub>i, j</sub> = False			 se w<sub>i+j</sub> $\neq$ X<sub>i</sub> $\and$ w<sub>i+j</sub> $\neq$ Y<sub>j</sub>

#### CASO RICORSIVO

***Se i = 0 $\and$ j > 0***
C<sub>i, j</sub> = C<sub>i, j-1</sub>						  se w<sub>j</sub> = y<sub>j</sub>C

***Se i > 0 $\and$ j = 0***
C<sub>i, j</sub> = C<sub>i-1, j</sub>						  se w<sub>i</sub> = y<sub>i</sub>

***Se i > 0 $\and$ j > 0***
C<sub>i, j</sub> = C<sub>i-1, j</sub>						  se w<sub>i+j</sub> = x<sub>i</sub> $\and$  w<sub>i+j</sub> $\neq$ y<sub>j</sub>
C<sub>i, j</sub> = C<sub>i, j-1</sub>						  se w<sub>i+j</sub> $\neq$ x<sub>i</sub> $\and$  w<sub>i+j</sub> = y<sub>j</sub>
C<sub>i, j</sub> = C<sub>i-1, j</sub> $\or$ C<sub>i, j-1</sub> 			se w<sub>i+j</sub> = x<sub>i</sub> $\and$  w<sub>i+j</sub> = y<sub>j</sub>

<div style="page-break-after: always; break-after: page;"></div>

## MIX TRA LCS E KNAPSACK

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... ,x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... ,y<sub>n</sub>> n
Sequenze di numeri $\mathbb{N}$

**SOLUZIONE:** Lunghezza di una LCS tra X e Y tale che la somma dei numeri è $\leq$ K *(dove K $\geq$ 0)*

#### DEFINIZIONE VARIBILI COINVOLTE

**ISTANZA:** (i, j, k)	con i $\in$ {0, 1, ... m},	j $\in$ {0, 1, ... n},	k $\in$ {0, 1, ... K}

$\forall$ (i, j, k) con i $\in$ {0, 1, ... m} , j $\in$ {0, 1, ... n}, k $\in$ {0, 1, ... K}
Introduco la variabile C<sub>i, j, k</sub> che è soluzione del sotto problema (i, j, k) ossia è la lunghezza di una LCS tra x<sub>i</sub>, y<sub>j</sub> t.c. la somma dei suoi valori sia $\leq$ k

#### CASO BASE

C<sub>i, j, k</sub> = 0			per (i = 0 $\or$ j = 0)	$\forall$ k $\in$ {0, ... K}


#### CASO RICORSIVO

C<sub>i, j, k</sub> = $\begin{cases}
C_{i-1, \space j-1, \space k} & \text{Se } x_{i} = y_{j} \space \and x_{i} > k \\
1 + C_{i-1, \space j-1, \space k - i} & \text{Se } x_{i} = y_{j} \space \and x_{i} \leq k \\
max(C_{i-1, \space j, \space k}, \space C_{i, \space j-1, \space k}) & \text{Se } x_{i} \neq y_{j} \\
\end{cases}$
*(Somiglia molto al problema dello zaino e/o alla subset sum)*

<div style="page-break-after: always; break-after: page;"></div>

## LICS

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... ,x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... ,y<sub>n</sub>> n

**SOLUZIONE:** lunghezza di LCS tra X e Y ***crescente***

#### DEFINIZIONE VARIBILI COINVOLTE

***MANCA INFORMAZIONE PER I SOTTO PROBLEMI***
Si introduce un problema ausiliario (AUX)
**ISTANZA:** x<sub>m</sub>, y<sub>n</sub>
**SOLUZIONE:** Lunghezza di LCS di X e Y crescente e che termina con x<sub>m</sub> e y<sub>n</sub> *(se questi coincidono)*

C<sub>i, j</sub> soluzione del sotto problema (i, j) del problema aux $\implies$ lunghezza di una LCS di X e Y e che termina con x<sub>i</sub> e y<sub>j</sub> se coincidono

#### CASO BASE (AUX)

C<sub>i, j</sub> = 0			se x<sub>i</sub> $\neq$ y<sub>j</sub>


#### CASO RICORSIVO (AUX)

C<sub>i, j</sub> = 1 + max{C<sub>h, k</sub>  | 1 $\leq$ h $\leq$ i,	 1 $\leq$ k $\leq$ j	|	x<sub>h</sub> < x<sub>i</sub>}	Se x<sub>i</sub> = y<sub>j</sub>		*(uguale per ordine lessicografico)*
*(per decrescente basta mettere x<sub>h</sub> > x<sub>i</sub>, uguale per ordine lessicografico decrescente)*

**SOLUZIONE PROBLEMA DI PARTENZA:** max{C<sub>i, j</sub>}	i $\in$ {0, 1, ... m}, j $\in$ {0, 1, ... n}



## VARIANTI

### CARATTERI UGUALI

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... ,x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... ,y<sub>n</sub>> n

**SOLUZIONE:** lunghezza di LCS tra X e Y senza 2 caratteri consecutivi uguali

***TUTTO UGUALE MA:***

#### CASO RICORSIVO (AUX)

C<sub>i, j</sub> = 1 + max{ C<sub>h, k</sub>  | 1 $\leq$ h $\leq$ i,	 1 $\leq$ k $\leq$ j	|	x<sub>h</sub> $\neq$ x<sub>i</sub>}	Se x<sub>i</sub> = y<sub>j</sub>



### PARI E DISPARI

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... ,x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... ,y<sub>n</sub>> n

**SOLUZIONE:** lunghezza di LCS tra X e Y che alterna pari e dispari

***TUTTO UGUALE MA:***

#### CASO RICORSIVO (AUX)

C<sub>i, j</sub> = 1 + max{ C<sub>h, k</sub>  | 1 $\leq$ h $\leq$ i,	 1 $\leq$ k $\leq$ j	|	x<sub>h</sub> % 2 $\neq$ x<sub>i</sub> % 2}	Se x<sub>i</sub> = y<sub>j</sub>



### ALTERNA BLU E ROSSO

*(Il Denny ci tiene a specificare che tifa Genova, non ci capisco un cazzo di calcio io però)*

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... ,x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... ,y<sub>n</sub>> n

**SOLUZIONE:** lunghezza di LCS tra X e Y che alterna blu e rossi

***TUTTO UGUALE MA:***

#### CASO RICORSIVO (AUX)

C<sub>i, j</sub> = 1 + max{ C<sub>h, k</sub>  | 1 $\leq$ h $\leq$ i,	 1 $\leq$ k $\leq$ j	|	Col(x<sub>h</sub>) $\neq$ Col(x<sub>i</sub>) }	Se x<sub>i</sub> = y<sub>j</sub>

*("Per me esiste solo un pesto, ovvero il genovese, oggi lo mangio che l'ho fatto io" cit. Denny)*
*(Also, il riso java lo mangia quando ha il cagotto e odia il pesto dell'esselunga)*



### A CONSECUTIVE

#### ISTANZA

X = <x<sub>1</sub>, x<sub>2</sub> ... ,x<sub>m</sub>> m
Y = <y<sub>1</sub>, y<sub>2</sub> ... ,y<sub>n</sub>> n
Caratteri

**SOLUZIONE:** lunghezza di LCS tra X e Y nella quale non vi sono mai lettere 'A' consecutive

***TUTTO UGUALE MA:***

#### CASO RICORSIVO (AUX)

C<sub>i, j</sub> = $\begin{cases}
1 + max\{C_{h, \space k} \space | 1 \leq h \leq i, \space 1 \leq k \leq j \space | \space  x_{h} \neq x_{i} \space \} & \text{Se } x_{i} = y_{j} \space \and x_{i} = \space \space  'A' \\
1 + max\{C_{h, \space k} \space | 1 \leq h \leq i, \space 1 \leq k \leq j \space \} & \text{Se } x_{i} = y_{j}  \space \and x_{i} \neq \space \space 'A' 
\end{cases}$