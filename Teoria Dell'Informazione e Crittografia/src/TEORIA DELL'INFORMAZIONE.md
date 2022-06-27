# TEORIA DELL'INFORMAZIONE

Come prima cosa vedremo quella che è chiamata *Teoria dei Codici* che si occupa

- della ***codifica di sorgente*** che trova dei modi efficienti per rappresentare le informazioni *(dove efficienti significa compatti, parliamo quindi di compressione)*
- dei ***codici a individuazione/correzione d'errore*** che invece si occupa di verificare se si verificano errori durante la trasmissione dei dati ed eventualmente di correggerli.

Possiamo inoltre vedere la **crittografia** come una codifica per cui solo chi conosce un'informazione segreta *(chiave)* può decodificare il messaggio.

La *Teoria dell'Informazione* invece si occupa di prendere dei messaggi e definire una certa quantità chiamata *informazione*. Chiaramente questa è una quantità *tecnica* e quindi non collegata al significato di un messaggio, in quanto creare una teoria su quest'ultimo è impossibile a causa delle interpretazioni diverse che può assumere.



## TEORIA DEI CODICI

Prima di parlare di codici vediamo come vengono generati i dati: abbiamo una

- **Sorgente**: un sensore, ad esempio un termometro che emette ad ogni *clock* un valore. Noi non tratteremo sorgenti continue ma solo discrete.

  La sorgente è discreta, cioè emette dei simboli appartenenti ad un **alfabeto *finito*** $S = \{s_1, \ ...,\ s_q\}$; in particolare, ad ogni colpo di *clock* la sorgente emette **un solo** simbolo, non più di uno e non zero.
  Inoltre, la sorgente è **memory-less**, ovvero il simbolo che uscirà al prossimo istante non dipende in nessun modo dai simboli emessi negli istanti precedenti.

  Infine, per essere *interessanti*, le sorgenti devono essere **casuali**, non deve esserci uno schema fisso dell'uscita dei simboli. Questo significa che ogni simbolo $s_i$ dell'alfabeto $S$ ha una certa probabilità $0 < p_i < 1$ di essere emesso; in particolare queste probabilità non possono essere $1$ in quanto in quel caso la sorgente non sarebbe più casuale *(esce sempre il simbolo con probabilità $1$)* e non possono essere $0$ in quanto un simbolo con probabilità nulla non ha motivo di essere codificato.

  Chiaramente $\sum\limits_{i=1}^{q} p_i = 1$

  Per trasmettere poi questi valori la sorgente effettuerà una operazione di ***codifica***.
  Questa codifica si compone di due parti:

  1. *Codifica di Sorgente*: il suo obiettivo è quello di rappresentare i dati nella maniera più compatta possibile *(ad esempio invece di misurare la temperatura da -100°C a +100°C è più conveniente farlo da -10°C a +40°C o ancora meglio codificare la differenza rispetto alla temperatura nell'istante prima)*
  2. *Codifica di Canale*: prepara i dati ad essere spediti all'interno del canale *(ad esempio codifica in binario)*. Essa tiene conto anche del *rumore* e quindi agisce in modo inverso rispetto alla codifica di sorgente cercando di introdurre una ridondanza che permetta al destinatario di accorgersi di eventuali errori.

- **Canale**: il mezzo trasmissivo utilizzato per inviare i dati.
  Su di esso agisce un elemento inevitabile, ovvero il **rumore** che cambia i dati all'interno del canale in modo casuale.

  Il segnale proveniente dal canale dovrà essere ***decodificato***.

- **Destinatario**: il consumatore del segnale.



### CODIFICA

Una codifica è una funzione che prende un simbolo da un alfabeto $S$ e lo trasforma in una stringa dell'alfabeto $\Gamma$, ovvero $cod\ : \ S \to \Gamma^*$. Ad esempio $\Gamma = \{0, 1\}$.

Abbiamo quindi $cod(s_i) = w_i$

L'output della codifica, la stringa dell'alfabeto $\Gamma$ viene chiamata ***codeword***.
Chiaramente, dato che è una stringa, avrà una lunghezza $l_i = |w_i|$.

Avendo ora le lunghezze di tutte le *codeword* possiamo definire $L = \sum\limits_{i=1}^{q}p_i *l_i$, ovvero una somma pesata delle lunghezze delle varie *codeword* pesate dalla probabilità che il simbolo che le genera esca dalla sorgente. Questa lunghezza media è quella che si cerca di minimizzare nella codifica di sorgente.



#### TIPOLOGIE DI CODICI

Abbiamo vari tipi di codici:

- **A BLOCCHI**: le $l_i$ sono tutte uguali *(es. ASCII)*.
  Ad ogni colpo di *clock* vengono spediti $l_i$ bit.

  Un esempio è il **codice di Van Duuren** che è un cosiddetto ***codice 3-su-7***; questo vuol dire che il codice usa delle sequenze di 7 bit in cui esattamente 3 valgono $1$. Questa regola è utile a far capire al decodificatore se il messaggio ricevuto sia o meno valido *(se i bit pari a $1$ non sono esattamente 3 chiaramente il messaggio è compromesso)*.
  Con questo codice si possono quindi codificare $(^7_3) = \frac{7!}{3!(7-3)!} = \frac{5040}{144} = 35$ *codewords*.
  *(La binomiale deriva dal fatto che si hanno 7 posizioni ma solo 3 possono valere $1$ allo stesso momento)*.

  Una variante di questi codici che permettono solo un certo numero di $1$ per volta sono i cosiddetti **CODICI PESATI** dove le posizioni di ogni cifra hanno peso diverso, un esempio è il **codice 0 1 2 4 7** che è un codice 2-su-5 che quindi può codificare 10 *codewords*. L'idea è che i pesi debbano codificare la cifra corrispondente; quindi ad esempio 1 sarà codificato come $11000 = 1*0 + 1*1 + 0*2 + 0*4 + 0*7 = 1$ e così via. La cifra $0$ è l'unica a *saltare* questa regola e utilizza l'unica codifica rimasta.

- **A LUNGHEZZA VARIABILE**: ogni *codeword* ha una lunghezza $l_i$ propria *(es. Morse)*.

  Chiaramente questi tipi di codici hanno un problema di compressione sul dove finisce una *codeword* e dove inizia la successiva non essendo appunto tutte della stessa lunghezza.

  Il vantaggio di questo tipo di codifica tuttavia è la possibilità di associare *codeword* meno lunghe a simboli più frequenti, in modo da diminuire la somma pesata $L$ complessiva.
  Ad esempio in inglese il carattere *e* è il più frequente *(compare circa il 12% delle volte nei testi)* e per questo motivo Morse assegnò a questo carattere la codifica più piccola di un unico punto.



### CODIFICA DI CANALE

Abbiamo visto che sul canale agisce anche una componente detta *rumore* che può cambiare il segnale e quindi lo scopo della codifica di canale è quello di identificare *(**Detective Code**)* ed eventualmente correggere questi errori *(**Error Correction Code**)*.



#### DETECTING CODE

Il meccanismo di base che viene utilizzato in molti codici sia per l'individuazione che per la correzione dei codici è il cosiddetto **controllo di parità** che consiste nel avere, per un pacchetto di $n-1$ bit, un bit detto *di parità* che farà si che l'intero pacchetto di $n$ bit abbia un numero pari di $1$; ovvero varrà $0$ se tra gli $n$ bit c'è già un numero pari di $1$ e $1$ viceversa.
Chiaramente il decodificatore se nota che in tutti gli $n$ bit c'è un numero dispari di $1$ saprà che c'è stato un'errore.

Se chiamiamo $x_1,\ ...,\ x_{n-1}$ i bit del messaggio e $y$ il bit di parità possiamo scrivere
$y = x_1 \oplus\ ...\ \oplus x_{n-1}$					*($\oplus$ è l'operatore logico XOR)*

È interessante notare anche che $(\land, \oplus)$ rappresenta una base completa per la logica booleana e che questi due operatori corrispondo rispettivamente al prodotto modulo 2 e alla somma modulo 2 dei bit: questo permette di semplificare le funzioni booleane complesse applicando le proprietà associativa e commutativa  delle operazioni aritmetiche.

Immaginiamo ora che il messaggio, con tanto di bit di parità, originale sia $01100$ e che il decodificatore riceva $00100$; il ricevente vede subito che c'è stato un errore e richiede la ritrasmissione.
Immaginiamo tuttavia che il decodificatore riceva invece $10100$: il numero di $1$ è pari e il ricevente non si accorge dell'errore; questo è dovuto al fatto che un singolo bit di parità riesce a rilevare solo un numero dispari di errori.

Chiaramente il rumore potrebbe colpire anche solo il bit stesso di parità ed invalidare l'intero messaggio, pur essendo questo valido. Usare un solo bit di parità ha senso però per canali con probabilità bassissime di errori.

Esistono vari modi per calcolare la parità di un messaggio: spesso le CPU, anche le meno costose a 8 bit, presentano un registro dedicato a questo. Altrimenti si può pensare ad un circuito di XOR con profondità $\log n$ dove ad ogni livello viene calcolato lo XOR tra due bit o anche ad una macchina a stati finiti che eseguirà il calcolo in $n$ passi.



##### RIDONDANZA

Definiamo la ridondanza come un numero razionale $R = \frac{\text{n tot di bit}}{\text{n bit di msg}}$

Nel caso del controllo di parità abbiamo $R = \frac{n}{n-1} = \frac{(n-1)+1}{n-1} = 1+\frac{1}{n-1}$

In generale $R = \frac{msg\ +\ check}{msg} = 1+ \frac{check}{msg}$

Chiaramente $R \geq 1$ per come è stato definito e la quantità di quanto supera $1$ viene detta **eccesso di ridondanza** e più questo è piccolo, più il codice è efficiente. Inoltre, più è piccolo, più ogni bit di controllo "*copre*" più cifre di messaggio.



##### RUMORE BIANCO E PROBABILITÀ

Per analizzare i vari casi possibili dobbiamo introdurre un modello matematico di rumore e il più semplice modello di rumore è il **rumore bianco**, la cui idea è che, considerando un pacchetto di $n$ bit, la spedizione e l'arrivo corretto o sbagliato di un bit è un *evento stocastico* e quindi la ***probabilità d'errore*** $p$ è ***fissata*** ed è ***la stessa per ogni bit***. Inoltre, nel rumore bianco, l'arrivo corretto o sbagliato di ogni bit è ***indipendente dagli altri***; si tratta quindi di **eventi indipendenti stocastici**.

Chiaramente queste assunzioni sono molto forti e spesso sono anche poco realistiche: di solito gli errori avvengono ad esempio in seguito a dei sbalzi di corrente che causano un flusso continuo di tanti errori ***(burst di errori)*** per un tot di tempo e poi tornano ad esserci pochi errori *(si pensi ad esempio ad un fulmine)*.

Ci chiediamo ora, data la spedizione di un pacchetto di bit, quale sia la probabilità di avere un errore:
Abbiamo $n$ bit, ognuno con probabilità $p$ di arrivare errato, quindi la probabilità che vi sia un solo errore è la probabilità $p$ che un bit sia sbagliato moltiplicata per la probabilità che tutti gli altri $n-1$ bit siano corretti $(1-p)^{n-1}$ *(stiamo moltiplicando perché per ipotesi gli eventi sono indipendenti)*.

Chiaramente però l'errore può occorrere in qualsiasi degli $n$ bit, mentre noi abbiamo calcolato la probabilità che l'errore occorra in una posizione specifica.

Dunque $P[1\ err] = n \sdot p\sdot (1-p)^{n-1}$, ovvero la probabilità calcolata prima moltiplicata per ogni possibile posizione.

Consideriamo ora la probabilità che occorrano due errori
$P[2\ err] = (^n_2)\sdot p^2 \sdot (1-p)^{n-2}$			*(notiamo che $(^n_1) = n$)*

Generalizzando il tutto possiamo dire che $P[k\ err] = (^n_k) \sdot p^k \sdot (1-p)^{n-k}$



##### PARITÀ ED ERRORI

Il nostro obiettivo ora è capire quando il decodificatore, nel caso del controllo di parità, sbaglia, ovvero quando abbiamo un numero di errori pari

$P[ctrl\ parità\ fail] = P[2\ err] + P[4\ err] + \ ...$

Notiamo che

- $1 = [p + (1-p)]^n = \sum\limits_{k = 0}^{n} (^n_k)\sdot p^k\sdot (1-p)^{n-k} = \sum\limits_{k=0}^{n} P[k\ err]$

- $(1 - 2p)^n = [-p + (1-p)]^n = \sum\limits_{k=0}^{n} (-1)^k \sdot (^n_k) \sdot p^k \sdot (1-p)^{n-k}$

Ora notiamo che $(-1)^k = \begin{cases} 1 &se\ k\ pari \\ -1 & se\ k\ dispari \end{cases}$

Adesso se sommiamo le due sommatorie ottenute prima, nel caso di $k$ pari otterremmo il doppio della prima mentre nel caso di $k$ dispari le due sommatorie si annullano a vicenda.

$\frac{1 + (1-2p)^n}{2} = \sum\limits_{h=0}^{n/2} (^n_{2h}) \sdot p^{2h} \sdot (1-p)^{n-2h}$

A questo punto dobbiamo togliere il valore per $h=0$, in quanto il caso in cui ci sono $0$ errori non va considerato

$P[ctrl\ parità\ fail] = \sum\limits_{h=1}^{n/2} (^n_{2h}) \sdot p^{2h} \sdot (1-p)^{n-2h} = \frac{1+(1-2p)^n}{2} - (1-p)^n$

Sapendo che $1+\alpha n$ sono i primi due termini dello sviluppo in serie di McLauren di $(1+ \alpha)^n$ possiamo dire che $P[1\ err] = n\sdot p\sdot (1-p)^{n-1} \approx n\sdot p \sdot [1-p(n-1)] = n\sdot p \sdot [1-n\sdot p +p] = n\sdot p - n^2p^2 + n\sdot p^2$

Sapendo che $0 \leq p \leq 1$ sappiamo che $p^2 \leq p$, questo ci permette di trascurare i contributi alla somma $p^2$, ottenendo così $P[1\ err] \approx np$

La stessa cosa vale nel caso generale, quindi $P[k\ err] \approx (^n_k)\sdot p^k$

Quindi se ad esempio per il nostro canale la probabilità di errore è $p = 10^{-3}$, ovvero un bit su mille arriva sbagliato, la probabilità che avvengano due errori è pari a $p = 10^{-6}$.

Come detto prima, il modello del rumore bianco non gestisce bene i burst di errori perché i vari bit risultano indipendenti fra loro in questo modello, ma si possono comunque gestire alcuni casi controllando il messaggio per colonne invece che per righe: in pratica si aggiunge un messaggio di *check* che contiene la parità degli altri messaggi in colonna *(ad esempio usando codifiche a blocchi come ASCII)*.



##### CODICI PESATI

L'idea di questi codici è di assegnare alle posizioni dei simboli che compongono il messaggio dei pesi. I simboli stessi hanno una cifra assegnata, ad esempio si possono assegnare numeri crescenti alle lettere dell'alfabeto.
Generalmente abbiamo che il messaggio $m_1\ m_2\ ...\ m_n\ c$ avrà i pesi $n+1\ n\ ...\ 2\ 1$.

In questo tipo di codici risulta utile avere un numero totali di simboli primo. Il mittente ha un messaggio da spedire e deve quindi calcolare una cifra di controllo tale per cui la somma pesata delle cifre dei messaggi per i pesi sia congrua a zero modulo $n$ dove $n$ è il numero di simboli totali *(ad esempio 37)*.

Supponiamo che il messaggio sia $C\ I\ A\ O\ chk$ che corrisponde alla sequenza di cifre $2\ 8\ 0\ 14\ chk$ con pesi $5\ 4\ 3\ 2\ 1$.
Dobbiamo trovare la cifra di controllo $chk$ tale per cui $2*5 + 8*4 + 0*3 + 14*2 + chk*1$ sia congruo a 0 modulo 37.
Facendo i conti abbiamo $70 + chk = 0\mod 37$ per cui $chk = 4$.

Il fatto che 37 sia primo permette di evitare soluzioni complicati per $chk$ poiché i numeri primi rappresentano dei campi invece che degli anelli il che permette di evitare soluzioni multiple e divisori dello zero *(elementi diversi da zero che moltiplicati per altri elementi fanno zero)*.

Il destinatario deve sapere quanto è lungo il messaggio in modo da calcolare i pesi: a questo punto può o sommare la $chk$ ricevuta e controllare che il risultato sia congruo a 0 modulo 37 o calcolarsi nuovamente la $chk$ e confrontarla con quella ricevuta. Si noti che è molto difficile che modificando il messaggio il risultato della somma pesata sia ancora congrua a 0 modulo 37.

Ipotizziamo di avere $a\ b$ con pesi $k+1\ k$ e che il mittente invece di spedire $ab$ spedisca $ba$.
Il mittente calcolerà $a(k+1) + bk$ mentre il ricevente calcolerà $b(k+1) + ak$.

Facciamo ora $a(k+1) + bk - b(k+1) - ak = ak + a + bk -bk -b -ak= a-b$

È chiaro quindi che l'unico caso in cui non si hanno problemi è quando $a = b$.

Esiste anche un algoritmo per calcolare la cifra di controllo: si hanno due variabili, somma $sum$ e somma delle somme $ssum$.
$sum = 0$
$ssum = 0$
$while\ not\ EOF:$
	$read\ sym$
	$sum = sum + sym \mod 37$
	$ssum = ssum + sum \mod 37$
$end$
$tmp = ssum + sum \mod 37$
$chk = 37 - tmp \mod 37$
$return\ chk$

Vediamo un esempio di esecuzione

| MSG  | SUM           | SSUM             |
| ---- | ------------- | ---------------- |
| w    | w             | w                |
| x    | w + x         | 2w + x           |
| y    | w + x + y     | 3w + 2x + y      |
| z    | w + x + y + z | 4w + 3x + 2y + z |

A questo punto in tmp avremo 5w + 4x + 3y + 2z. Chiaramente a tmp dobbiamo aggiungere $chk$.
Ma $tmp + chk$ deve essere congruo a 0 modulo 37, ovvero $tmp + chk = 37 \mod 37 \longrightarrow chk = 37 - tmp \mod 37$.

Un esempio reale di codice pesato è l'**ISBN**. Il codice non è ben pensato: la prima cifra indica lo stato del libro, le seguenti due cifre indicano la casa editrice, le seguenti sei indica il libro in sé e l'ultima cifra è quella di controllo. In questo codice i conti vengono fatti modulo 11 *(ci sono 10 cifre)*.
Un problema che sorge da questa scelta è che a volte la cifra di controllo può valere 10 e in quel caso si mette X.



#### CODICI RETTANGOLARI

I codici visti fino ad ora permettono solo di verificare se è accaduto un errore, ma non sanno nulla sulla loro località e quindi non possono coreggerli.
L'idea dei codici rettangolari è di disporre logicamente i bit di messaggio in una sorta di rettangolo che verrà poi *circondato* da una colonna e da una riga di cifre di controllo *(la cifra di controllo sulla diagonale è superflua)*. In particolare le cifre di controllo saranno dei bit di parità di righe e colonne.

Ovviamente nel caso di un singolo errore, incrociando numero di riga e numero di colonna dove le parità non tornano, si può risolvere l'errore.
La capacità di correzione è di un errore *(è vero che se gli errori avvengono in righe e colonne diverse si possono correggere, ma non si può controllare dove avvengono gli errori)*.

Per capire quanto è efficiente questo codice calcoliamone la ridondanza $R = \frac{m \times n}{(m-1) \times (n-1)} = 1 + \frac{1}{m-1} + \frac{1}{n-1} + \frac{1}{(m-1) \times (n-1)}$

Ma quali sono i codici rettangolari più efficienti? Chiaramente per ridurre la ridondanza a parità di cifre di messaggio dobbiamo ridurre il numero di cifre di controllo. Supponiamo di dover inviare 24 bit: possiamo fare varie scelte come inviare 4 righe e una colonna, 12 righe e 2 colonne, 6 righe e 4 colonne e così via.

Facendo i conti fuoriesce che il caso più efficiente e 6 x 4. In particolare, se trattassimo la ridondanza come una funzione di cui calcolare il minimo, scopriremo che i punti di minimo si ottengono quando $m =n$, ovvero quando il rettangolo è un quadrato.
In particolare, nel caso di un quadrato, abbiamo $R = \frac{n^2}{(n-1)^2} = 1 + \frac{2}{n-1} + \frac{1}{(n-1)^2}$



#### CODICI TRIANGOLARI

Si può migliorare ulteriormente la ridondanza con i cosiddetti codici triangolari dove le cifre di messaggio vengono disposte logicamente come un triangolo dove l'ipotenusa sarà fatta da cifre di controllo. Se il lato del triangolo è lungo $n$, abbiamo $n$ cifre di controllo e $\sum\limits_{i=1}^{n-1}i = \frac{(n-1)n}{2}$ cifre di messaggio. Le cifre di controllo calcolano la parità della riga e della colonna che intercettano.

Abbiamo quindi $\sum\limits_{i=1}^{n}i = \frac{n(n+1)}{2}$ cifre totali.

La ridondanza è quindi $R = \frac{\frac{n(n+1)}{2}}{\frac{(n-1)n}{2}} = \frac{(n+1)}{(n-1)} = \frac{n-1+2}{n-1} = 1 + \frac{2}{n-1}$

Si nota subito come questa ridondanza sia migliore di quella dei codici quadrati in quanto manca uno dei termini *($1/(n-1)^2$)*.



#### CODICI CUBICI

Si è pensato quindi alle possibilità di disposizione dei bit per migliorare ulteriormente la ridondanza *(specialmente per valori grandi di $n$)*.
Nei codici cubici disponiamo il messaggio sotto forma di cubo avendo $(n-1)^3$ cifre di messaggio. Le cifre di controllo sono disposte lungo i spigoli *(sporgono però dal cubo)* e ogni cifra controlla la parità di un unico piano. Abbiamo in totale $(3n-2)$ cifre di controllo.

Abbiamo quindi una ridondanza $R = \frac{(n-1)^3 + (3n-2)}{(n-1)^3} = 1 + \frac{3n-2}{(n-1)^3} \approx 1 + \frac{3}{n^2}$

Possiamo addirittura pensare di disporre il messaggio come un ipercubo di 4 dimensioni *(tanto la disposizione è solo logica)* avendo $(n-1)^4$ cifre di messaggio e $4(n-1) +1 = 4n-3$ cifre di controllo.
La ridondanza sarà quindi $R = \frac{(n-1)^4 + (4n-3)}{(n-1)^4} = 1 + \frac{4n-3}{(n-1)^4} \approx 1 + \frac{4}{n^3}$

In generale, aumentando le dimensioni dell'ipercubo abbiamo $R = \frac{(n-1)^k + (k(n-1) +1)}{(n-1)^k} = 1 + \frac{k(n-1) +1}{(n-1)^k} \approx 1 + \frac{k}{n^{k-1}}$

Abbiamo quindi che la ridondanza, all'aumentare delle dimensioni, tende a zero come l'inverso di un polinomio.

**Hamming** si chiese però se, in un canale con rumore bianco *(ogni posizione stessa probabilità d'errore, posizioni indipendenti)*, quale fosse il minimo numero assoluto di cifre di controllo per correggere un errore.



#### CODICI DI HAMMING

Considerando di avere $n$ bit che finiscono in un canale, di cui alcuni di messaggio e alcuni di controllo. Chiamiamo $k$ il numero di cifre di messaggio ed $m$ il numero di cifre di controllo. Queste cifre di controllo possono avere $2^m$ possibili configurazioni.
Per poter stabilire se è avvenuto un errore e dove è avvenuto è necessario che valga $2^m \geq n+1$ poiché vogliamo distinguere $n+1$ eventi, ovvero nessun errore, errore in posizione 1, errore in posizione 2, ecc.

Notiamo subito che $n = m+k$, quindi abbiamo $2^m \geq m+k+1$
Questo ci permette di capire quante cifre di controllo usare per un determinato messaggio.
Ad esempio supponiamo di voler spedire 4 cifre di messaggio, quindi $k =4$: abbiamo  $2^m \geq m + 5$
Si tratta quindi di trovare l'$m$ più piccolo che rispetti tale disuguaglianza. In questo caso abbiamo $m=3$, ovvero $2^3 \geq 8$.
Col crescere del numero di cifre di messaggio aumenta l'efficienza.

La prossima domanda è dove mettere le varie cifre di controllo.
L'ideale è avere delle equazioni di parità tali che contengano solo le cifre di messaggio e la cifra di controllo che stiamo calcolando, ovvero del tipo $c_i = x_j \oplus x_k\oplus\ ...\ \oplus\ x_l$, in modo da semplificare il tutto e poter calcolare le varie cifre di controllo in parallelo.
Inoltre, vorremmo che queste equazioni fossero **linearmente indipendenti** in modo che ogni cifra di controllo dia informazioni separate e non ridondanti.

<img src=".\img\001.png" style="zoom:80%;" align="left" />Chiaramente quando c'è un errore si vorrebbe ottenere la posizione dell'errore: l'idea è che la sequenza di $m$ bit, chiamata **sindrome**, fornisca in binario la posizione dell'errore.
Vediamo come possiamo disporre le cifre di controllo e come calcolarle con $m=3$ e $k=4$.
Notiamo che possiamo esprimere con $m=3$ bit la posizione nella quale avviene l'errore: supponendo che l'errore avvenga in posizione 5, vorremmo che la sindrome sia $101$. In generale, ci conviene fare gli xor nelle posizioni in cui la cifra $c_i$ corrisponde ad un $1$. Quindi, ad esempio, nel caso di $c_1$ abbiamo $c_1 \oplus pos_1 \oplus pos_3 \oplus pos_5 \oplus pos_7 = 0$ osservando la tabella di posizioni.
Inoltre, conviene mettere le cifre di controllo nelle uniche righe che contengono un solo 1, avendo di fatto $c_1 \oplus x_3 \oplus x_5 \oplus x_7 = 0$.
In generale, la $i$-esima cifra di controllo si trova in posizione $2^i$.

Chiaramente i codici ottimali sono quelli tali per cui $n = 2^m - 1$ in modo da non sprecare configurazioni delle cifre di controllo.
In questi casi la ridondanza e $R = n/k = \frac{k+m}{k} = 1 + \frac{m}{k} \approx 1 + \frac{\log_2 k}{k}$ perché $2^m = m+k+1 \approx k$.

Chiaramente con due cifre di controllo si ammette una sola cifra di messaggio e il risultato è una codifica per triplicazione.



##### INTERPRETAZIONE GEOMETRICA

Lavorando con pacchetti di bit, stiamo lavorando con vettori presi da $\{0, 1\}^n$.
Ad esempio possiamo avere un vettore $x = (x_1,\ ...,\ x_n)$ e un vettore $y = (y_1,\ ...,\ y_m)$ su cui possiamo definire la somma tra vettori come lo XOR bit a bit tra i vettori $x+y = (x_1 \oplus y_1,\ ...,\ x_n \oplus y_m)$.
Possiamo anche definire $c \sdot x = (c\sdot x_1,\ ...,\ c\sdot x_n)$: tutta via $c \in \{0,1\}$, quindi se $c = 0$ il risultato è il vettore nullo, se $c=1$ il risultato è $x$ stesso.

Per visualizzare il tutto pensiamo a vettori tridimensionali e ad un cubo che rappresenta i possibili vettori che possiamo creare.
Quando avviene un errore si sta di fatto facendo uno XOR con un vettore di errori che dice quali bit flippare e quali no. Inoltre, un errore corrisponde ad un percorso nel cubo dal vettore che si voleva inviare a quello ricevuto.

Definiamo anche la **distanza di Hamming** tra i due vettori come il numero minore di spigoli da attraversare per passare da un vettore ad un altro e corrisponde al numero di posizioni che cambiano. Il **peso di Hamming** di un vettore è il numero di uni che contiene.

L'interpretazione geometrica ci aiuta a capire i messaggi validi: se i messaggi validi sono a distanza uno, se c'è un errore ci ritroviamo sempre in un messaggio valido. Se sono a distanza due il ricevente che si accorge di un errore non sa correggere l'errore. Per poter anche correggere un errore dobbiamo avere una distanza maggiore di due.

Notiamo che con distanza 3 possiamo identificare un errore e correggerne uno, con distanza 4 possiamo identificarne due ma correggerne solo 1.
Possiamo pensare che le parole valide siano circondate da delle sfere: se la parola ricevuta appartiene ad una sola sfera so come correggerla, se appartiene a due sfere capisco che c'è un errore ma non so correggere.

Notiamo che $\frac{vol\ spazio}{vol\ sfera} \geq nr\ di\ sfere$. In particolare lo spazio ha dimensione $2^n$.
Una sfera per un punto $x$ è definita come $s(x, r) = \{y \in \{0,1\}^n\ |\ d_h(y,x) \leq r \}$

In particolare, una sfera di raggio 1 corrisponde al prendere il messaggio e cambiare un bit in tutti i modi possibili: una sfera di raggio 1 ha quindi dimensione $n+1$ *(tutti i modi in cui può avvenire un errore più il caso senza errori)*.

Inoltre, con $k$ bit di messaggio abbiamo $2^k$ messaggi validi: di conseguenza dobbiamo avere $2^k$ sfere.

Abbiamo quindi $\frac{2^n}{n+1} \geq 2^k$, ovvero $\frac{2^n}{2^k} \geq n+1$
Sapendo che $n = m+k$ abbiamo $2^m \geq n+1$

Notiamo che $n+1$ lo possiamo vedere come $(^n_0) + (^n_1)$, ovvero come tutti i modi in cui si hanno zero errori + tutti i modi in cui si ha un errore.
Di conseguenza, se volessimo un codice per correggere due errori, le sfere avrebbero volume $(^n_0) + (^n_1) + (^n_2) = 1 + n + \frac{n(n-1)}{2}$.



### CODIFICA DI SORGENTE

Abbiamo detto in precedenza che l'obiettivo della codifica di sorgente è quello di minimizzare $L = \sum p_il_i$, ovvero la lunghezza media delle varie codeword.
Abbiamo poi visto che ci sono codici a lunghezza fissa e in quel caso abbiamo $L = \sum p_i l = l \sum p_i = l	\sdot 1$. *($p_i$ sono probabilità, la loro somma fa 1)*.

Chiaramente sono più interessanti i codici a lunghezza variabile ma bisogna stabilire come può fare il destinatario per capire dove finisce una codeword e dove inizia un'altra. Bisogna quindi stare attenti a come si definiscono le codifiche delle varie codeword: si richiede che il codice sia **univocamente decodificabile**, cioè data una sequenza di bit ci deve essere un'unica scomposizione in codeword.

Capire, a partire dalle codifiche delle codeword, se un codice è univocamente decodificabile non è semplicissimo in quanto bisogna considerare tutte le possibili lunghezze di sequenze di simboli codificate. Tuttavia, si può usare una sottoclasse dei codici univocamente decodificabili, ovvero i **codici istantanei** per cui la verifica è semplice.
La particolarità dei codici istantanei è che è possibile scoprire subito la codeword analizzata senza osservare l'intero messaggio.
Un esempio è usare lo $0$ come terminatore *(comma code)*, codificando di fatto i simboli con gli $1$. In particolare, se nessuna codeword è prefisso di nessun'altra, so sicuramente cosa sto decodificando.

Chiaramente questo può essere rappresentato con un albero di decodifica, visto che ad ogni simbolo si scartano delle possibilità.
Ovviamente la codifica migliore dipende dalla profondità dell'albero ma anche dalle probabilità associate alle varie codeword.



#### DISUGUAGLIANZA DI KRAFT

Risulta chiaro che vorremmo lavorare sempre con codici istantanei. La domanda è, data una sorgente, quando esiste un codice istantaneo?
Per rispondere a questa domanda è stata creata la **Disuguaglianza di Kraft** *(teorema)* che dice che una condizione *necessaria* e *sufficiente* affinché ***esista*** un codice istantaneo per una sorgente di $q$ simboli con codeword di lunghezze $l_1 \leq l_2 \leq\ ...\ \leq l_q$ è che valga $K =  \sum\limits_{i=1}^{q} \frac{1}{r^{l_i}} \leq 1$ dove $r$ è il numero di simboli con cui scriviamo le codeword, ovvero $r = |\Gamma|$.

**DIMOSTRAZIONE**

1. Se esiste un codice istantaneo con lunghezze $l_1,\ ...,\ l_q$, allora $K \leq 1$
   In realtà ci viene dato un codice istantaneo con quelle lunghezze, quindi possiamo dire che per ogni c.i. con lunghezze $l_1,\ ...,\ l_q$ $K\leq 1$.

   Faremo una dimostrazione per induzione sulla profondità dell'albero.
   Ci soffermiamo sul caso $r = 2$ *(in casi maggiori cambia poco)*.

   Il **caso base** è rappresentato dagli alberi di profondità 1 che possono essere di due tipi: con una sola codeword di lunghezza 1 o con due codeword di lunghezza 1.
   Per il primo caso abbiamo una sola codeword di lunghezza 1 e quindi un solo termine per la sommatoria. In particolare $K = 1/2^1 \leq 1$.
   Per il secondo caso abbiamo due codeword di lunghezza 1 e quindi $K = 1/2^1 + 1/2^1 = 1 \leq 1$.

   **Passo di induzione**
   Supponiamo che il teorema sia vero per ogni albero di profondità minore di $n$ e dimostriamo che vale anche per alberi di profondità $n$.
   Possiamo vedere i sottoalberi come alberi di decodifica a loro volta che avranno $K_1, K_2 \leq 1$ visto che per loro vale il teorema.

   Osserviamo che mettendo insieme i due sottoalberi per creare l'albero di profondità $n$ allunghiamo di 1 la lunghezza delle varie codeword dei sottoalberi, avendo di fatto $K = \sum {1}/{r^{l_i +1}} = 1/r \sum 1/r^{l_i}$

   Abbiamo quindi $K = \frac{1}{r} K_1 + \frac{1}{r}K_2$
   Nel nostro caso $r=2$ e, per supposizione $K_1, K_2 \leq 1$. Sicuramente $\frac{1}{2} K_i \leq 1/2$ e la somma di due contributi $\leq 1/2$ è sicuramente $\leq1 $.

2. Se $K \leq 1$ allora esiste un codice istantaneo con lunghezze $l_1,\ ...,\ l_q$

   Definiamo la lunghezza massima $l = \max\{l_1,\ ...,\ l_q\}$
   Ora $\forall j \in \{1,\ ...,\ l\}$ definiamo $t_j$ come il **numero di codeword di lunghezza** $j$.

   Riscriviamo la disuguaglianza come $K = \sum\limits_{j=1}^{l} \frac{t_j}{r^j} \leq 1$
   Possiamo riscrivere $\frac{t_j}{r^j}$ come $t_j r^{-j}$ avendo $t_1 r^{-1} +\ ...\ + t_lr^{-l} \leq 1$
   Ovviamente possiamo moltiplicare entrambi i membri per $r^l$

   Ottenendo quindi $t_1r^{l-1} +\ ...\ + t_{l-1}r + t_l \leq r^l$
   Possiamo quindi ricavare, notando che $t_l$ è naturale:

   $0 \leq t_l \leq r^l - t_1r^{l-1} -\ ....\ - t_{l-1}r$
   Cioè ci sarà un numero limitato di codeword di lunghezza $l$.

   Dividendo poi per $r$ possiamo ottenere
   $0 \leq t_{l-1} \leq r^{l-1} - t_1r^{l-2} -\ ...\ - t_{l-2}r$

   Procediamo quindi dividendo per $r$ $l$ volte *(per questo abbiamo moltiplicato per $r^l$)*.
   Arriveremo a
   $0 \leq t_2 \leq r^2 - t_1r$
   $0\leq t_1 \leq r$

   A questo punto si ripercorrono le disuguaglianze dall'ultima alla prima costruendo un codice istantaneo che soddisfa le varie disuguaglianze.
   Chiaramente se le verifichiamo tutte, verifichiamo la disuguaglianza di Kraft.

   Quindi, se creiamo $t_1$ codeword di lunghezza $1$, lasciamo *da parte* $r-t_1$ simboli di $\Gamma$.
   Ora, per creare $t_2$ codeword di lunghezza $2$, nella prima cella possiamo mettere solo i $r-t_1$ lasciati da parte *(altrimenti avremmo dei prefissi)*, mentre nella seconda cella possiamo metterli tutti: in totale abbiamo $r-t_1 \sdot r = r^2 - t_1r$ possibilità.

Si può notare che per le codeword binarie si verifica $K \lt 1$ solo quando ci sono dei nodi interni di decisione che non hanno entrambi i sottorami, ovvero per codici non efficienti: se accorciamo i rami non efficienti arriviamo a $K=1$.



#### DISUGUAGLIANZA DI MCMILLAN

Abbiamo quindi visto che la disuguaglianza di Kraft è una condizione necessaria e sufficiente per stabilire che esiste un codice istantaneo con certe lunghezze. McMillan si pone domande simili a Kraft ma in relazione a tutti i codici univocamente decodificabili.

La **disuguaglianza di McMillan** ci dice che una condizione necessaria e sufficiente affinché esista un codice u.d. con codeword di lunghezze $l_1,\ ...,\ l_q$ è che valga $K = \sum\limits_{i=1}^q \frac{1}{r^{l_i}} \leq 1$

Ci dice cioè che la disuguaglianza di Kraft non vale solo per i codici istantanei, ma per tutti i codici univocamente decodificabili

**DIMOSTRAZIONE**

1. Se esiste un codice univocamente decodificabile con lunghezze $l_1,\ ...,\ l_q$, allora $K \leq 1$
   Anche in questo caso possiamo dire che per qualsiasi codice univocamente decodificabile vale $K \leq 1$.

   Sappiamo che $K = \sum\limits_{i=1}^q \frac{1}{r^{l_i}}$: prendiamo $n \gt 1$ intero e calcoliamo $K^n$: possiamo riscrivere il tutto come $K^n = \sum\limits_{t=n}^{nl} \frac{N_t}{r^t}$
   Abbiamo quindi raccolto tutti i termini con lo stesso denominatore.
   Definiamo quindi $l = \max\{l_1,\ ...,\ l_q\}$: è chiaro che $l_i$ può variare tra $1$ e quindi $t$ varierà tra $n$ e $nl$. Al numeratore avremmo un certo numero che dipende da $t$.
   Questo $N_t$ possiamo vederlo come il numero di codeword di lunghezza $t$. Se il codice è univocamente decodificabile, allora $N_t \leq r^t$ perché $r^t$ sono tutte le possibili codeword di lunghezza $t$: se $N_t$ fosse più grande dovrei far collidere due o più di queste.

   Essendo $N_t \leq r^t$, sicuramente $\frac{N_t}{r^t} \leq 1$. Ricordiamo inoltre che $n \gt 1$
   Quindi $K^n = \sum\limits_{t=n}^{nl} \frac{N_t}{r^t} \leq \sum\limits_{t=n}^{nl}1 = nl-n+1 \lt nl$
   
   Alla fine otteniamo $K^n \lt nl$: osservando graficamente le due funzioni possiamo notare che vale $K^n \lt nl$ solo per $K \leq 1$.
   
2. Se $K \leq 1$ allora esiste un codice univocamente decodificabile con lunghezze $l_1,\ ...,\ l_q$

   Supponiamo $K \leq 1$: per Kraft esiste un codice istantaneo con lunghezze $l_1,\ ...,\ l_q$.
   Ogni codice istantaneo è anche univocamente decodificabile.



#### ALGORITMO HUFFMAN

Vediamo ora un algoritmo greedy per la soluzione del problema di ottimo per trovare un codice istantaneo che minimizzi la lunghezza media.

Facciamo però delle considerazioni su come deve essere fatto un codice ottimale:

Se abbiamo le probabilità ordinate $p_1 \ge p_2 \ge\ ...\ \ge p_q$, allora le lunghezze che otterremmo saranno $l_1 \le l_2 \le\ ...\ \le l_q$. Se così non fosse il codice non avrebbe la lunghezza media più piccola possibile.

Difatti con $p_m \gt p_n$ e $l_m \gt l_n$ abbiamo
$p_ml_n + p_nl_m - p_ml_m - p_nl_n = p_m(l_n-l_m) - p_n(l_n - l_m) = (p_m - p_n)(l_n - l_m) \lt0 $

Il che significa che la differenza di contributo prima e dopo lo scambio è negativa, cioè scambiando abbiamo abbassato la lunghezza media.

Consideriamo codici binari, quindi con $r =2$.
Consideriamo una sorgente con 5 simboli che escono con probabilità $0.4,\ 0.2,\ 0.2,\ 0.1,\ 0.1$.
La scelta localmente ottima è prendere i due simboli meno probabili e costruire un nuovo simbolo come la combinazione di questi due che avrà quindi probabilità la somma delle loro probabilità.

Costruiamo quindi una nuova sorgente fittizia con i quattro simboli che avranno probabilità $0.4,\ 0.2,\ 0.2,\ 0.2$. Procediamo allo stesso modo.

Otteniamo una sorgente con tre simboli e probabilità $0.4,\ 0.4,\ 0.2$.
Riduciamo ancora e otteniamo $0.6,\ 0.4$.

Chiaramente un codice ottimali consiste nell'assegnare $0$ ad uno dei due simboli e $1$ all'altro. Tornando poi indietro andiamo ad allungare le codeword dei simboli aggregati aggiungendo uno $0$ e un $1$ **in coda** *(per non creare prefissi)*.
Cambiando l'ordine in cui si inseriscono le codeword combinate si cambia la varianza delle lunghezze delle varie codeword risultanti. Generalmente si preferisce una varianza più bassa che corrisponde al mettere il simbolo combinato il più in alto possibile. Si osserva che più si mette in alto il simbolo combinato, più si abbassa la varianza. Se si rompe l'ordine delle probabilità non otteniamo più un codice ottimale.


**DIMOSTRAZIONE**
Dimostriamo per assurdo che l'algoritmo di Huffman produce un codice ottimo.
Supponiamo che un altro algoritmo produca una lunghezza media $L' \lt L$.

Notiamo che in entrambi gli alberi ci devono essere almeno due codeword che hanno lunghezza massima, altrimenti avremmo un nodo di decisione con un solo ramo. In particolare, in entrambi i casi avremmo un contributo $l_q(p_q + p_{q-1})$. Andando a condensare i sottoalberi riduciamo il contributo e in entrambi i casi arriviamo a $(l_q -1)(p_q + p_{q-1})$.

Andando a condensare sempre di più arriviamo ad un albero con due sottoalberi con le codeword $0$ e $1$ per Huffman. Se $L' \lt L$, dovremmo avere $L' \lt 1$, che però è impossibile.