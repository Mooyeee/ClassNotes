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



#### DETECTIVE CODE

Il meccanismo di base che viene utilizzato in molti codici sia per l'individuazione che per la correzione dei codici è il cosiddetto **controllo di parità** che consiste nel avere, per un pacchetto di $n-1$ bit, un bit detto *di parità* che farà si che l'intero pacchetto di $n$ bit abbia un numero pari di $1$; ovvero varrà $0$ se tra gli $n$ bit c'è già un numero pari di $1$ e $1$ viceversa.
Chiaramente il decodificatore se nota che in tutti gli $n$ bit c'è un numero dispari di $1$ saprà che c'è stato un'errore.

Se chiamiamo $x_1,\ ...,\ x_{n-1}$ i bit del messaggio e $y$ il bit di parità possiamo scrivere
$y = x_1 \oplus\ ...\ \oplus x_{n-1}$					*($\oplus$ è l'operatore logico XOR)*

È interessante notare anche che $(\land, \oplus)$ rappresenta una base completa per la logica booleana e che questi due operatori corrispondo rispettivamente al prodotto modulo 2 e alla somma modulo 2 dei bit.

Immaginiamo ora che il messaggio, con tanto di bit di parità, originale sia $01100$ e che il decodificatore riceva $00100$; il ricevente vede subito che c'è stato un errore e richiede la ritrasmissione.
Immaginiamo tuttavia che il decodificatore riceva invece $10100$: il numero di $1$ è pari e il ricevente non si accorge dell'errore; questo è dovuto al fatto che un singolo bit di parità riesce a rilevare solo un numero dispari di errori.

Chiaramente il rumore potrebbe colpire anche solo il bit stesso di parità ed invalidare l'intero messaggio, pur essendo questo valido.



##### RIDONDANZA

Definiamo la ridondanza come un numero razionale $R = \frac{\text{n tot di bit}}{\text{n bit di msg}}$

Nel caso del controllo di parità abbiamo $R = \frac{n}{n-1} = \frac{(n-1)+1}{n-1} = 1+\frac{1}{n-1}$

Chiaramente $R \geq 1$ per come è stato definito e la quantità di quanto supera $1$ viene detta **eccesso di ridondanza** e più questo è piccolo, più il codice è efficiente. Inoltre, più è piccolo, più ogni bit di controllo "*copre*" più cifre di messaggio.



##### RUMORE BIANCO E PROBABILITÀ

Per analizzare i vari casi possibili dobbiamo introdurre un modello matematico di rumore e il più semplice modello di rumore è il rumore bianco, la cui idea è che, considerando un pacchetto di $n$ bit, la spedizione e l'arrivo corretto o sbagliato di un bit è un evento stocastico e quindi la probabilità d'errore $p$ è fissata ed è la stessa per ogni bit. Inoltre, nel rumore bianco, l'arrivo corretto o sbagliato di ogni bit è indipendente dagli altri; si tratta quindi di eventi indipendenti stocastici.

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

- $[-p + (1-p)]^n = \sum\limits_{k=0}^{n} (-1)^k \sdot (^n_k) \sdot p^k \sdot (1-p)^{n-k}$

Ora notiamo che $(-1)^k = \begin{cases} 1 &se\ k\ pari \\ -1 & se\ k\ dispari \end{cases}$

Adesso se sommiamo le due sommatorie ottenute prima, nel caso di $k$ pari otterremmo il doppio della prima mentre nel caso di $k$ dispari le due sommatorie si annullano a vicenda.

$\frac{1 + (1-2p)^n}{2} = \sum\limits_{h=0}^{n/2} (^n_{2h}) \sdot p^{2h} \sdot (1-p)^{n-2h}$

A questo punto dobbiamo togliere il valore per $h=0$, in quanto il caso in cui ci sono $0$ errori non va considerato

$P[ctrl\ parità\ fail] = \sum\limits_{h=1}^{n/2} (^n_{2h}) \sdot p^{2h} \sdot (1-p)^{n-2h} = \frac{1+(1-2p)^n}{2} - (1-p)^n$

Sapendo che $1+\alpha n$ sono i primi due termini dello sviluppo in serie di McLauren di $(1+ \alpha)^n$ possiamo dire che $P[1\ err] = n\sdot p\sdot (1-p)^{n+1} \approx n\sdot p \sdot [1-p(n-1)] = n\sdot p \sdot [1-n\sdot p +p] = n\sdot p - n^2p^2 + n\sdot p^2$

Sapendo che $0 \leq p \leq 1$ sappiamo che $p^2 \leq p$, questo ci permette di trascurare i contributi alla somma $p^2$, ottenendo così $P[1\ err] \approx np$

La stessa cosa vale nel caso generale, quindi $P[k\ err] \approx (^n_k)\sdot p^k$

Quindi se ad esempio per il nostro canale la probabilità di errore è $p = 10^{-3}$, ovvero un bit su mille arriva sbagliato, la probabilità che avvengano due errori è pari a $p = 10^{-6}$.