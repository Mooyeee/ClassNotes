[TOC]



<div style="page-break-after: always;"></div>

# CONCORRENZA - INTRODUZIONE

*Concorrere* singifica correre insieme; i sistemi concorrenti quindi rappresentano diversi componenti che operano in contemporanea. In particolare questi componenti possono coordinarsi per un obiettivo comune ma anche competere per le risorse disponibili.

Possiamo trovare la concorrenza ovunque, alcuni esempi di sistemi concorrenti sono:

- **Cellula vivente**: all'interno di essa avvengono in contemporanea molti processi come l'assemblaggio di proteine, la duplicazione del dna, ecc. Nella cellula non c'è un *direttore*, dunque è un sistema ***asincrono***.
- **Orchestra musicale**: in questo sistema, a differenza della cellula, c'è un direttore che da il tempo agli altri componenti; è un sistema ***sincrono***, ovvero che fa riferimento ad un *cronometro* condiviso da tutti i componenti e dove ogni componente sa che deve eseguire una certa azione in un certo momento.
- **Processore multicore**: un processore dove possono essere eseguite diverse operazioni in contemporanea.
- **Squadra antincendio**: sistema dove i componenti hanno un obiettivo comune: spegnere l'incendio e si coordinano a tal scopo.



#### COME PROGETTARE E REALIZZARE SISTEMI CONCORRENTI CORRETTI?

Per realizzare dei sistemi concorrenti corretti abbiamo bisogno di:

- **Linguaggi**: ovviamente per poter realizzare dei programmi concorrenti abbiamo bisogno di linguaggi di programmazione che permettano di stabilire quali operazioni vanno eseguite in parallelo e quali in serie.

  Un esempio pratico è Java con la classe *Thread*.
  Un esempio più astratto è la ***partitura musicale*** che rappresenta le varie parti che un'orchestra deve suonare. Essendo l'orchestra un sistema sincrono inoltre, questo si riflette anche nel linguaggio; nelle partiture il tempo è indicato allo stesso modo per tutte le parti.

  Un linguaggio astratto può essere rappresentato dai sistemi di equazioni *(ad esempio **CCS** che vedremo in seguito)*, che permettono di scrivere dei programmi estremamente astratti concentrandoci sulla comunicazione fra le parti più che al trattamento dei dati.

- **Modelli**: avremmo bisogno anche di modelli, ovvero di modi per rappresentare il sistema in una maniera semplificata rispetto al sistema reale mantenendo però le caratteristiche cruciali della concorrenza. Vogliamo analizzare questi modelli anziché i programmi concorrenti poiché sono più semplici e non presentano dettagli presenti invece nei programmi che non sono necessariamente collegati alla concorrenza, permettendoci così di concentrarci solo sugli aspetti effettivamente rilevanti.

  Un esempio di modello sono i *grafi di attività*, ovvero un grafo dove i nodi rappresentano delle attività e gli archi rappresentano la precedenza tra le varie attività; i nodi non ordinati fra loro possono essere eseguiti parallelamente.
  Altri modelli sono ad esempio le reti di Petri. Saranno studiati in seguito.

- **Logica**: per esprimere i criteri secondo i quali un certo sistema è corretto useremo la logica.

<div style="page-break-after: always;"></div>

# CORRETTEZZA DI PROGRAMMI SEQ

Consideriamo il seguente programma sequenziale

```java
	int f (int v[], int n) {
        int x = v[0]; int p = 0; int a = 1;
        while(a < n) {
            if (v[a] < x) {
                x = v[a]; p = a;
            }
            a = a + 1;
        }
        return p;
    }
```

SI tratta sostanzialmente di una *funzione* $f$ che prende in input un vettore di interi $v$ e un intero $n$ rappresentante la dimensione del vettore $v$ e calcola il valore minimo contenuto in esso. Infine, restituisce la posizione del valore minimo calcolato.

**Come possiamo dimostrare la sua correttezza?**
Prima di poter dire come si può dimostrare che il programma sia corretto è necessario definire più esattamente possibile cosa vuol dire che un programma sia *corretto*.



**DEFINIAMO IL CRITERIO DI *CORRETTEZZA***
Per definire un criterio che sia *rigoroso*, appelliamo alla matematica e al suo linguaggio *preciso*.
Per quanto riguarda il programma di esempio, vorremmo specificare la richiesta che ritorni sempre la posizione dell'elemento minimo; possiamo farlo utilizzando una formula di questo tipo:

**Alla fine dell'esecuzione $\forall i \in \{0, \ ..., \ n-1\}\ \ \  v[p] \leq v[i]$**			*(postcondizione)*
Ovvero, vogliamo che alla fine dell'esecuzione il valore indicato da $p$ sia minore o uguale di tutti gli altri valori del vettore.
In genere però non basta specificare cosa si vuole che sia vero alla fine dell'esecuzione; in molti casi un programma viene scritto per soddisfare una condizione alla fine dell'esecuzione purché valga qualcosa all'inizio dell'esecuzione. Nel nostro esempio questo è banale e può essere specificato come segue:

**Prima dell'esecuzione $\forall i \in \{0,\ ..., \ n-1\}\ \ \ v[i] \in \mathbb{Z}$**			*(precondizione)*
Ovvero, vogliamo che prima dell'esecuzione i valori del vettore $v$ siano dei numeri interi, visto che era stato dichiarato come vettore di interi.
Abbiamo specificato cosa intendiamo dire quando diciamo una funzione è corretta; diremo che la funzione $f$ è *soddisfacente* o *corretta* se ogni volta che i dati, prima dell'esecuzione, soddisfano la precondizione alla fine dell'esecuzione soddisferanno la postcondizione.



**DOMANDA**: come mai esistono programmi in grado di decidere la correttezza sintattica di un programma *(i compilatori)*, ma non esistono programmi che, dati in input precondizioni, postcondizioni e codice del programma siano in grado di deciderne la correttezza semantica?

Perché è un problema indecidibile. Basti pensare all'*halting problem*; un programma corretto è un programma che sicuramente termina, ma non possiamo decidere se termina o meno.
Tuttavia questo significa che non possiamo creare un verificatore generale di correttezza, ma non che non sia possibile decidere se un singolo programma sia o meno corretto.



## RIPASSO: LOGICA PROPOSIZIONALE

Prima di parlare di logica proposizionale è necessario inquadrare cosa si intende più in generale con logica *formale* o *matematica*; quando si parla di logica in matematica si intende un **sistema** formato da un ***linguaggio***, e quindi da una grammatica che specifica come costruire delle frasi *(formule)*.

Possiamo quindi costruire delle formule e lo facciamo con lo scopo di assegnare loro un significato; quindi, fissata una logica come linguaggio, vogliamo definire una ***semantica***, ovvero un modo per attribuire un significato agli elementi del linguaggio e in particolare alle formule.
Nel caso della logica proposizionale, dal punto di vista teorico, il significato di una formula è il suo **valore di verità**.

Una volta definite sintassi e semantica, possiamo usare una logica per costruire delle ***dimostrazioni***; aver definito una semantica non vuol dire che di fronte ad una formula si è in grado di dire subito se sia valida o meno, dunque con la logica si sviluppa un metodo di dimostrazione e si cerca, in un numero finito di passi, di dimostrare la validità di una certa formula.
Un esempio, legato alla logica dell'aritmetica, è la formula che dice che per ogni numero primo $x$, esiste sempre un numero primo $y$ più grande di esso; questa formula non è verificabile sperimentalmente essendo in numeri infiniti e va quindi dimostrata in altri modi.

Per ogni logica bisogna quindi definire un **apparato deduttivo**, cioè un insieme di ***regole di inferenza***.
Una regola di inferenza è una regola che dice *"se hai già dimostrato queste premesse, allora puoi dedurre questa formula"*.



#### LOGICA PROPOSIZIONALE - SINTASSI

Passiamo ora al caso specifico della logica proposizionale, considerata come la logica più semplice e che ci servirà per le dimostrazioni di correttezza.
Per costruire il linguaggio avremmo bisogno di

- $PA = \{p_1, \ ..., \ p_i, \ ...\}$		Proposizioni atomiche *(frasi, espressioni aritmetiche, ecc)*
- $\perp$, $\mathsf{T}$												Costanti logiche, sono formule sempre false/vere
- $\lnot$, $\lor$, $\land$, $\rightarrow$, $\leftrightarrow$								Connettivi logici, utili a creare formule complesse
- (, )													Delimitatori, sempre per le formule complesse

Adesso dobbiamo definire la ***grammatica*** della logica proposizionale e lo faremo induttivamente.
Definiamo $F_p$ insieme delle **formule ben formate**:

1. $\perp$, $\mathsf{T}$ $\in$ $F_p$									Le costanti logiche sono fbf
2. $\forall p_i \in PA$, $pi \in F_p$				Le proposizioni atomiche sono fbf
3. Se $A, B \in F_p$, allora $(\lnot A)$, $(A \lor B)$, $(A \land B)$, $(A \rightarrow B$), $(A \leftrightarrow B)$ $\in F_p$

*Esempio*: $(x < 0 \land (\lnot(y \geq z)))$ *(in questo caso $x<0 \in PA$)*

<div style="page-break-after: always;"></div>

#### LOGICA PROPOSIZIONALE - SEMANTICA

Adesso vogliamo attribuire un significato alle formule definite prima.
Siamo interessati a sapere se una formula sia vera o meno. Il valore di verità di una formula tuttavia dipende dai valori di verità delle sue proposizioni atomiche.

Il punto di partenza è quindi stabilire quali proposizioni atomiche considerare vere: questo si può fare formalmente definendo una

*Funzione di valutazione* $v: PA \rightarrow \{0,\  1\}$ *(0 e 1 stanno per False e True)*

Estendiamo ora $v$ induttivamente ad una funzione definita su tutte le formule ben formate
$I_v : F_p \rightarrow \{0, 1\}$			*funzione di interpretazione*

1. $I_v(\perp) = 0$			$I_v(\mathsf{T}) = 1$					Falso e vero hanno sempre valori 0/1
2. $\forall p_i \in PA$			$I_v(p_i) = v(p_i)$			L'interpretazione riprende il valore di $v$ per le PA
3. $I_v(\lnot A) = 1 - I_v(A)$							Passo induttivo
   $I_v(A \lor B) = 1$   sse $I_v(A) = 1$ o $I_v(B) = 1)$
   . . .
   $I_v(A \rightarrow B) = 1$   sse $I_v(B) = 1$ o $I_v(A)=0$



Qualche termine

- $A$ è SODDISFATTA da $I_v$ se $I_v(A)=1$
- $A$ è SODDISFACIBILE se esiste $I_v$ tale che $I_v(A) =1$
- $A$ è una TAUTOLOGIA se $I_v(A)=1$ per ogni $I_v$
- $A$ è CONTRADDITTORIA se $I_v(A) = 0$ per ogni $I_v$

EQUIVALENZA LOGICA
$A \equiv B$ sse $I_v(A) = I_v(B)$ per ogni $I_v$



#### MODELLI

Definiamo ora i *modelli*, ovvero delle interpretazioni delle proposizioni atomiche, cioè una scelta di valori di verità per tutte le proposizioni atomiche.

Un modello è un sottoinsieme delle proposizioni atomiche $M \subseteq PA$ a cui è associata un'interpretazione $I_M : F_p \rightarrow \{0, 1\}$ definita come tale: $I_M(p_i) = 1$ sse $p_i \in M$.
In sostanza il modello *sceglie* quali proposizioni atomiche sono vere.

Possiamo definire la relazione $M \vDash A$ tra modelli e formule che possiamo leggere come
$M$ <sub>MODELLA</sub> $A$ oppure come
$A$ <sub>È SODDISFATTA IN</sub> $M$.
Quindi scriveremo $M \vDash A$ se $A$ risulta vera per la scelta particolare di $M$.

<div style="page-break-after: always;"></div>

#### LOGICA PROPOSIZIONALE - APPARATO DEDUTTIVO

Possiamo ora rappresentare l'apparato deduttivo della logica proposizionale.

Esso è composto da **regole di inferenza** scritte in questo modo: $\frac{A_1, \ ..., \ A_N}{B}$
Dove $A_i \in F_p$ sono le ***premesse***, mentre $B \in F_p$ è la ***conclusione***.

Alcune regole di inferenza sono ad esempio

- $\frac{A_1 \ \ \ \ \ \ A_2}{A_1 \land A_2}$					Se ho dimostrato vere $A_1$ e $A_2$, sarà vera anche $A_1 \land A_2$
- $\frac{A_1 \land A_2}{A_1}$					Se ho dimostrato vera $A_1 \land A_2$, $A_1$ è vera
- $\frac{A \ \ \ \ \ \ A \rightarrow B}{B}$				Modus Ponens
- $\frac{A \rightarrow B \ \ \ \ \ \ \lnot B}{\lnot A}$			Modus Tollens



Le regole di inferenza sono i mattoni con i quali si costruiscono le ***dimostrazioni***.
**DIMOSTRAZIONE**: Catena di applicazione di regole $A_1, \ ..., \ A_N \vdash B$
*($\vdash$ significa che da $A_1, \ ..., \ A_N$ si deriva $B$)*.



Abbiamo ora due nozioni distinte:

- La validità in un modello $\vDash$
- La derivabilità $\vdash$ introdotta perché in generale non siamo in grado di decidere direttamente la nozione semantica di validità e quindi cerchiamo di dimostrare una cosa.

Naturalmente ha senso fare le derivazioni se ciò che è derivabile è anche vero, per questo esistono questi due teoremi:

***Teorema***: Se $A_1, \ ..., \ A_n \vdash B$ allora  $A_1, \ ..., \ A_n \vDash B$   *(validità, correttezza)*
Cioè se riusciamo a derivare $B$ da $A_1, \ ..., \ A_n$, in ogni modello in cui sono vere $A_1, \ ..., \ A_n$, è vera anche $B$.
Questo teorema ci dice che *abbiamo scelto bene* le regole di inferenza e che queste non ci permettono di derivare cose non vere. Se questo teorema è valido, la logica è **corretta**.

***Teorema***: Se $A_1, \ ..., \ A_n \vDash B$ allora $A_1, \ ..., \ A_n \vdash B$   *(completezza)*
Cioè se in ogni modello in cui sono vere $A_1, \ ..., \ A_n$ ed è vera anche $B$, si può derivare $B$ da $A_1, \ ..., \ A_n$.
Questo teorema ci dice che possiamo dimostrare tutto ciò che è vero.
Se questo teorema è valido, la logica è **completa**.

<div style="page-break-after: always;"></div>

## LOGICA DI HOARE

Questa logica si può vedere come costruita su due livelli diversi poiché permette di stabilire e definire il criterio di correttezza per un dato programma. In particolare, definisce questa correttezza tramite le definizioni di *precondizioni* e *postcondizioni* che sono delle formule della logica proposizionale.

#### PRIMO LIVELLO

Dunque al primo livello avremmo le proposizioni atomiche definite come relazioni fra le variabili del programma *($x\leq 5$, $x > y$, $z = 2^y$, ...)*.

Per dare un significato, una semantica, alle varie proposizioni atomiche definiamo la nozione di **stato della memoria**.
Supponiamo di aver scritto un programma e chiamiamo $V$ l'insieme delle variabili del programma; uno stato della memoria è una *fotografia* della memoria del programma in un certo istante.

Possiamo definire più formalmente quest'idea di stato della memoria come una funzione $\sigma : V \rightarrow \mathbb{Z}$ che assegna un valore ad ogni variabile *(consideriamo solo variabili intere per semplicità)*.

Possiamo ora attribuire ad ogni proposizione atomica un valore di verità in un certo stato.
Ad esempio $x \leq 5$ è vera in $\sigma$ se $\sigma(x) \leq 5$.

Diremo quindi che la formula $\alpha$ è vera in $\sigma$ scrivendo $\sigma \vDash \alpha$.



#### SECONDO LIVELLO

Possiamo a questo punto definire le formule della logica di Hoare.
In particolare, le formule di questa logica sono costruite tramite **triple di Hoare** definite come segue

$\{\alpha\} \ \ \  \mathsf{C} \ \ \ \{\beta\}$			con $\alpha$ precondizione, $\beta$ postcondizione e $\mathsf{C}$ comando/programma.

Le triple di Hoare si leggono come segue:
**SE** il comando $\mathsf{C}$ viene eseguito a partire da uno stato della memoria nel quale $\alpha$ è vera, **ALLORA** l'esecuzione termina e nello stato finale $\beta$ è vera.

Quello che faremo sarà cercare di dimostrare varie triple di Hoare sviluppando poco per volta un apparato deduttivo per questa logica.



#### PSEUDO LINGUAGGIO USATO

Il linguaggio che useremo con le triple di Hoare ha le seguenti istruzioni:
x **:=** 5	*assegnamento*
**if** x < 0 **then**x := -x **else** x := 2*x **endif**	*scelta*
**while** x < n **do** x := x + 1 **endwhile**	*iterazione*
**skip**	*istruzione NOP*

<div style="page-break-after: always;"></div>

### LOGICA DI HOARE - APPARATO DEDUTTIVO

1. Istruzione nulla					$\frac{}{\{p\}\ \ \  skip\ \ \  \{p\}}$

   Iniziamo dal caso più banale, ovvero quello dell'istruzione *skip*.
   Per qualunque precondizione $p$, se eseguiamo uno skip questa non cambia il valore di verità della formula $p$ poiché non cambia lo stato della memoria. Notiamo che questa regola non ha bisogno di premesse.

   

2. Sequenza, composizione					$\frac{\{p\}\ \ \ C\ \ \ \{r\}\ \ \ \ \ \ \ \ \ \ \{r\} \ \ \ D\ \ \ \{q\}}{\{p\} \ \ \ C; \ D \ \ \ \{q\}}$

   Se troviamo

   - Una formula $r$ che, sotto l'ipotesi di eseguire $C$ a partire da uno stato in cui è valida $p$, risulti valida al termine dell'esecuzione
   - E che, eseguendo $D$ da uno stato in cui è valida $r$ alla fine risulti valida $q$

   Possiamo derivare la tripla che dice che se eseguiamo $C$ e $D$ in sequenza da uno stato in cui è valida $p$, alla fine varrà $q$.

   

   

3. Scelta					$\frac{\{p\  \land\  B\} \ \ \ C \ \ \ \{q\} \ \ \ \ \ \ \ \ \ \ \{p\  \land\  \lnot B\}\ \ \  D\ \ \  \{q\}}{\{p\}\ \ \  if\ B\ then\ C\ else\ D\ endif\ \ \ \{q\}}$

   Se dimostriamo che

   - Eseguendo $C$ da uno stato dove vale la precondizione $p$ e vale anche la condizione $B$ al termine dell'esecuzione vale $q$
   - Eseguendo $D$ da uno stato dove vale $p$ ma non vale $B$ arriviamo comunque ad uno stato dove vale $q$

   Possiamo derivare la regola della scelta dove prima dell'esecuzione dell'$\mathtt{if}$ vale $p$ mentre dopo vale $q$.

   

4. Implicazione, conseguenza					$\frac{\{p\  \rightarrow\  p'\} \ \ \ \ \ \ \ \ \ \ \{p'\}\ \ \  C\ \ \  \{q\}}{\{p\}\ \ \ C\ \ \ \{q\}}$         $\frac{\{p\}\ \ \ C\ \ \ \{q\}\ \ \ \ \ \ \ \ \ \ q\  \rightarrow\ q'}{\{p\}\ \ \ C\ \ \ \{q'\}}$

   Osserviamo un esempio:
   Supponiamo di aver scritto un programma e di voler dimostrare $\{x>0\}\ \ \ C\ \ \ \{x=2^y\} $, cioè vogliamo dimostrare che, eseguendo il programma $C$ a partire da uno stato della memoria dove vale $x>0$, si arriverà in uno stato dove $x$ sarà pari a $2^y$.

   Supponiamo di aver derivato una tripla che ci dice che eseguendo $C$ da uno stato dove vale $x \geq 0$, arriveremo in uno stato in cui $x = 2^y$, supponiamo cioè $\vdash \{x \ge 0\}\ \ \  C\ \ \  \{x=2^y\}$.

   Osserviamo a questo punto che $x>0 \rightarrow x \geq 0$; siccome quando $x$ è maggiore di 0 è sicuramente anche maggiore o uguale a 0 è ragionevole  concludere che eseguendo $C$ da uno stato dove $x>0$ arriverò in uno stato dove $x=2^y$, ovvero $\vDash \{x>0\}\ \ \ C\ \ \ \{x=2^y\}$.

   Notare l'utilizzo del $\vDash$; il ragionamento fatto si riferisce alla semantica della formula.

   

   Generalizzando possiamo dire che se abbiamo dimostrato una tripla e osserviamo una condizione che implica la precondizione della tripla, possiamo derivare la tripla con la condizione osservata al posto della precondizione.
   Discorso analogo è valido anche per la postcondizione.

   

5. Assegnamento					$\frac{}{\{q[E/x]\} \ \ \ x:=E\ \ \ \{q\}}$

   Dove con $[E/x]$ intendiamo dire *'sostituisci ogni occorenza di $x$ in $q$ con $E$'*.

   Osserviamo anche qui un esempio: prendiamo un'istruzione di assegnamento e supponiamo che il nostro obiettivo sia raggiungere uno stato dove vale $x\geq0$, ovvero $x := y+2\ \ \  \{x\geq0\}$.

   Quale precondizione garantisce la postcondizione richiesta?
   Notiamo che per far sì che $x \geq 0$, $y$ deve essere maggiore o uguale a $-2$.

   Possiamo generalizzare questo procedimento impostando come precondizione di un assegnamento la postcondizione desiderata sostituendo alla variabile assegnata il suo valore, cioè $\vDash \{y +2 \geq 0\} \ \ \ x:=y+2\ \ \ \{x \geq 0\}$, dove in questo caso $E = y+2$.

   

   **ISTANTI DI TEMPO DIVERSI**
   Nulla vieta che $x$ appaia di nuovo nella precondizione, ad esempio:
   Da $x:=x+2\ \ \ \{x<0\}$ possiamo derivare $\vdash \{x+2<0\} \ \ \ x:=x+2\ \ \ \{x<0\}$.
   In questo caso la $x$ della precondizione e la $x$ della postcondizione si riferiscono alla variabile $x$ in due instanti di tempo diversi.
   

   **CONTRADDIZIONI**
   Supponiamo di avere $x:=-2\ \ \ \{x>0\}$; notiamo subito che se assegnamo $-2$ ad $x$, non potrà mai essere maggiore di $0$. Applichiamo meccanicamente la regola: $\{-2>0\}\ \ \ x:= -2 \ \ \ \{x>0\}$ e notiamo subito che la proposizione atomica $-2 > 0$ equivale a $False$.
   Questa tripla è legittima e valida, ma non esiste alcun stato in cui valga la formula $False$.

   

   Anche questa regola non ha bisogno di alcuna premessa.

   

6. Iterazione *(correttezza parziale)*					$\frac{\{inv\  \land\  B\} \ \ \ C\ \ \ \{inv\}}{\{inv\} \ \ \  while\ B\ do\ C\ endwhile\ \ \ \{inv\  \land \  \lnot B\}}$

   L'idea per le istruzioni iterative è di trovare una formula che sia ***invariante*** rispetto al blocco dell'istruzione iterativa. Se troviamo un'invariante possiamo dire che se questa formula è vera all'inizio dell'istruzione iterativa, lo sarà anche alla fine e in più possiamo dire che al termine del blocco iterativo vale anche la negazione della condizione di ciclo.
   In generale, avendo un'istruzione $W = \mathtt{while\ B\ do\ C\ end-while}$, se deriviamo $\vdash \{p\land B\}\ C\ \{p\}$, allora possiamo dire che $p$ è invariante rispetto a $W$.

   **ALCUNE NOTE**

   - Data un'istruzione iterativa, non c'è un solo invariante. Ad esempio, $True$ è invariante per ogni istruzione iterativa. Generalmente ci interessano gli invarianti utili alla dimostrazione in corso.

   - Nella pratica, le istruzioni iterative sono inserite in programmi, di conseguenza la scelta dell'invariante dipende dal contesto e si abbina alla regola dell'implicazione.

     Vediamo un esempio:
     Supponiamo di avere la seguente formula $\{p\}\ C \ \{r\}\ W\ \{z\}\ D\ \{q\}$ e di volerci ricondurre a $\{inv\} \ W\ \{inv \land \lnot B\}$; se riusciamo a dimostrare che $r \rightarrow inv$ e che $inv \lnot B \rightarrow z$ avremo ricostruito una dimostrazione completa.
     
   - Avvolte l'invariante non è assoluto, ma lo diventa aggiungendoci la condizione di ciclo.

   - L'invariante in genere è violata guardando lo stato della memoria durante l'esecuzione del blocco, ma viene ripristinata al termine del blocco.

   

7. Iterazione *(correttezza totale)*					$\frac{inv \rightarrow E \geq 0\ \ \ \ \ \ \ \ \ \ \{inv\  \land\  B\  \land\  E = k\}\ \ \ C\ \ \ \{inv\ \land \ E < k\}}{\{inv\} \ \ \  while\ B\ do\ C\ endwhile\ \ \ \{inv\  \land \  \lnot B\}}$

   Quando trattiamo istruzioni iterative esistono due modi diversi per cui l'istruzione può fallire rispetto alla specifica data da una tripla:
   
   1. L'istruzione iterativa, eseguita a partire da uno stato che soddisfa la precondizione, termina in uno stato che *NON* soddisfa la postcondizione *(gestito nella correttezza parziale)*
   2. L'istruzione iterativa *NON* termina
   
   Chiaramente per dimostrare la correttezza di un programma è necessario tener traccia di entrambi questi casi. Parleremo quindi di *correttezza totale* quando considereremo anche la terminazione e di *correttezza parziale* quando ci concentreremo solo su pre e postcondizioni dando per scontato che l'istruzione termini.
   
   
   Consideriamo $W = \mathtt{while\ } B\ \mathtt{do}\ C\ \mathtt{endwhile}$
   
   **TECNICA DI DIMOSTRAZIONE**
   Supponiamo che $E$ sia un'espressione aritmetica nella quale compaiono variabili del programma, costanti numeriche e operazioni aritmetiche e che $inv$ sia un invariante di ciclo per $W$ scelta in modo che:
   
   1. $inv \rightarrow E \geq 0$
   2. $\vdash^{^{TOT}}_{_{ITER}} \{inv \land B \and E = k >0\}\ \ \ C\ \ \ \{inv \land E < k\}$
   
   ALLORA:
   $\vdash^{^{TOT}}_{_{ITER}} \{inv\} \ \ \ W\ \ \ \{inv \land \lnot B\}$
   
   
   
   Notiamo il $^{^{TOT}}$; questa  una definizione ricorsiva perché $C$ potrebbe contenere al suo interno altre operazioni ricorsive.
   
   Cerchiamo cioè una quantità *variante* che sarà maggiore o uguale a 0 quando è valido l'invariante e che permetterà di derivare la tripla che dice che il variante $E$ avrà un valore costante $k>0$ prima dell'esecuzione e sarà $E<k$ dopo l'esecuzione. Questo significa che se eseguiamo il corpo dell'iterazione $C$ partendo da uno stato dove valgono $inv$, $B$ e $E$ ha un certo valore, al termine di una iterazione il valore di $E$ diminuisce strettamente. 
   
   Notiamo che l'invariante implica che $E \geq 0$; avendo che ad ogni iterazione il valore di $E$ diminuisce strettamente e che l'invariante è sempre valido prima e dopo l'iterazione, siamo sicuri che l'istruzione prima o poi terminerà.
   
   **OSSERVAZIONI**
   
   - $E$ ***non*** è una formula logica, $E \geq 0$ lo è
   - Lo $0$ in $E \geq 0$ può essere sostituito da qualsiasi numero

<div style="page-break-after: always;"></div>

#### ESEMPI DI DERIVAZIONE

##### ASSEGNAMENTO
Consideriamo l'istruzione $x:= -x$, fissando come postcondizione $\{x<0\}$.
Ricaviamo una precondizione corretta per questa combinazione di comando e postcondizione.

Applichiamo la regola dell'assegnamento: $\vdash_{_{ASS}} \{-x<0\}\ \ \ x:=-x\ \ \ \{x<0\}$
Possiamo riscrivere la tripla in maniera più leggibile come $\{x>0\}\ \ \ \ x:=-x\ \ \ \{x<0\}$.



##### ASSEGNAMENTO

Consideriamo $x:=2*y \ \ \ \{x>y\}$
$\vdash_{_{ASS}} \{2*y>y\}\ \ \ x:=2*y\ \ \ \{x>y\}$      $\equiv$      $\{y>0\}\ \ \ x:=2*y\ \ \ \{x>y\}$



##### CONTROESEMPI

Supponiamo ora di dover dimostrare questa tripla $?\ \ \ \{True\}\ \ \ x=2*y\ \ \ \{x>y\}$
Proviamo ad applicare la regola dell'assegnamento $\vdash_{_{ASS}} \{y>0\}\ \ \ x=2*y\ \ \ \{x>y\}$: sappiamo ora che questa tripla è valida poiché l'abbiamo ottenuta tramite la derivazione, ma il nostro obiettivo è la prima tripla. L'unica regola che possiamo pensare di applicare è quella dell'implicazione, ma osserviamo che non è vero che $True \rightarrow y>0$, dunque non possiamo dimostrare la validità della prima tripla.
Questo è un bene in realtà, poiché non è certamente vero che eseguendo quell'assegnamento da uno stato in cui vale $True$ arriverò *sicuramente* in uno stato in cui vale $x>y$.

Tuttavia, il fatto che non siamo riusciti a dimostrare che sia vera non vuol dire che sia falsa: dobbiamo dimostrare che è falsa. Il modo più semplice è trovare un controesempio.
Un controesempio in questo caso è uno stato in cui $y=-1$: applicando l'assegnamento avremo $x=-2$ e $x \ngtr y$.



##### SEQUENZA

Supponiamo di voler dimostrare $?\ \ \ \{x+y=K\}\ \ \ x:=x-1;\ y:=y+1\ \ \ \{x+y=K\}$
Notiamo subito che qui il programma è composto da una sequenza di due assegnamenti, quindi dobbiamo trovare una formula intermedia che faccia da postcondizione per il primo assegnamento e da precondizione per il secondo. Siccome si tratta di assegnamenti, possiamo pensare di usare la regola dell'assegnamento, ottenendo:

- Da $y:=y+1\ \ \ \{x+y=K\}$          $\vdash_{_{ASS}}\{x+y+1=K\}\ \ \ y:=y+1\ \ \ \{x+y=K\}$
- E a questo punto da $x:=x-1 \ \ \ \{x+y+1=K\}$        $\vdash_{_{ASS}} \{x-1+y+1 = K\}\ \ \ x:=x-1\ \ \ \{x+y+1 = K\}$

Notiamo che $x-1+y+1 = K \equiv x+y= K$. 

Notiamo anche che la precondizione dell'assegnamento $y:=y+1$ è uguale alla postcondizione dell'assegnamento $x:=x-1$, dunque possiamo applicare la regola della sequenza $\vdash_{_{SEQ}} \{x+y=K\}\ \ \ x:=x-1;\ y:=y+1\ \ \ \{x+y = K\}$

Abbiamo così dimostrato la validità della tripla iniziale.

<div style="page-break-after: always;"></div>

##### SCELTA

Consideriamo il programma $P$

```pascal
y := k;
if x > 0 then
	y := y * x
else
	y := -2 * x
end-if
```

Vogliamo stabilire se vale $?\ \ \ \{k>0\} \ \ \ P\ \ \ \{y \geq 0\}$.

Osserviamo la postcondizione e in particolare che questa vale subito dopo un'istruzione di scelta: per poter derivare una tripla come istruzione di scelta dobbiamo *"spezzare"* i rami della scelta e stabilire se tutti e due i casi portino alla postcondizione richiesta.
I rami in questo caso sono

- $y:=y*x\ \ \ \{y \geq 0\}$
  Possiamo applicare questo ramo la regola dell'assegnamento $\vdash_{_{ASS}} \{y*x\geq 0\}\ \ \ y:= y*x \ \ \ \{y \geq 0\}$
- $y := -2 * x\ \ \ \{y \geq 0\}$
  Applichiamo ancora la regola dell'assegnamento
  $\vdash_{_{ASS}} \{-2*x \geq 0\}\ \ \ y:= -2 * x \ \ \ \{y \geq 0\}$



Ricordiamo che le precondizioni delle premesse della regola della scelta sono
$\{p \land B\}$          e          $\{p \land \lnot B\}$.

Notiamo che la precondizione della tripla da dimostrare dice che $k>0$ e l'istruzione prima dell'$\mathtt{if}$ assegna $k$ a $y$, di conseguenza avremo che sicuramente $y > 0$ dopo il primo assegnamento.

Scriviamo ora, sostituendo a $p= y>0$ e a $B = x>0$ *(la condizione dell'$\mathtt{if}$)* le premesse della regola della scelta *(dobbiamo dimostrare queste triple per poterla applicare)*

- $?\ \ \ \{y>0 \land x>0\}\ \ \ y:=y*x\ \ \ \{y \geq 0\}$                                  *($\{p \land B\}\ C\ \{q\}$)*
- $?\ \ \ \{y>0 \land x \leq 0\}\ \ \ y:=-2*x\ \ \ \{y\geq 0\}$                              *($\{p \land \lnot B\}\ D\ \{q\}$)*

Notiamo che la nostra precondizione nel primo ramo ($y*x \geq 0$) non coincide esattamente con la precondizione ricavata ora, ma notiamo anche che  $y>0 \land x>0 \rightarrow xy>0$, quindi $\vdash_{_{IMPL}}\{y>0 \land x>0\}\ \ \ y:=y*x\ \ \ \{y\geq 0\}$

Osserviamo ora il secondo ramo; dobbiamo confrontare $y>0 \land x \leq 0$ con $-2x\geq 0$.
Notiamo che possiamo riscrivere $-2x \geq 0$ come $x \leq 0$.
Senz'altro $y>0 \land x \leq 0 \rightarrow x \leq 0$, quindi applichiamo nuovamente la regola dell'implicazione $\vdash_{_{IMPL}} \{y>0 \land x \leq 0\}\ \ \ y:= -2x\ \ \ \{y \geq 0\}$

Notiamo ora che le ultime due triple derivate hanno esattamente la forma delle premesse della regola della scelta, quindi $\vdash_{_{IF}} \{y>0\}\ if\ x>0\ then\ y:= y*x\ else\ y:=-2*x\ endif\ \{y \geq 0\}$

Ora manca solo da dimostrare che anche l'assegnamento iniziale è valido: per fare ciò usiamo la regola dell'assegnamento usando come postcondizione la precondizione dell'$\mathtt{if}$, quindi $\vdash_{_{ASS}} \{k > 0\}\ \ \ y:= k\ \ \ \{y>0\}$

Infine, osservando che la postcondizione dell'assegnamento è la precondizione dell'$\mathtt{if}$ possiamo dire $\vdash_{_{SEQ}} \{k>0\} \ \ \ P\ \ \ \{y \geq 0\}$.

##### ITERAZIONE - ESPONENZIALE

Consideriamo il seguente programma $P$ che calcola l'esponenziale

```pascal
i := 0; s := 1; //A	
while i < N do  	//W
	i := i + 1; //C //W
	s := s * x; //C //W
end-while			//W
```

Vogliamo dimostrare la tripla $?\ \ \  \{N \geq 0\}\ \ \ P\ \ \ \{s = x^N\}$

Notiamo che il programma contiene due assegnamenti e un ciclo, dobbiamo quindi cercare un'invariante; per farlo proviamo a simulare i primi passi di un'esecuzione:
$i\ \ \ 0\ \ \ 1\ \ \ \ 2\ \ \ \ \ 3\ \ \ ...$
$s\ \ \ 1\ \ \ x\ \ \ x^2\ \ \ x^3\ \ \ ...$

Notiamo che $s = x^i$ per ogni iterazione; ogni volta $i$ viene incrementato, ma anche $s$ viene aggiornato. Chiaramente, se dovessimo osservare la memoria dopo il primo assegnamento ma prima dell'aggiornamento di $s$ l'invariante non sarebbe più valido, ma a noi interessano i momenti iniziale e finale del blocco iterativo.



**1. DIMOSTRAZIONE INVARIANTE**
Ipotizziamo quindi che $s = x^i$ sia un'invariante: dobbiamo dimostrarlo e per farlo possiamo usare la tripla che fa da premessa alla regola dell'iterazione *(che è sostanzialmente la definizione di invariante)*, ovvero $?\ \ \ \{s=x^i \land i <N\}\ \ \ C\ \ \ \{s=x^i\}$

Notiamo che la $C$ è composto da due assegnamenti, quindi possiamo derivare la tripla premessa della regola dell'iterazione usando la regola dell'assegnamento tramite $C\ \ \ \{s=x^i\}$:
$\vdash_{_{ASS}}\{sx=x^i\}\ \ \ s:= s*x\ \ \ \{s=x^i\}$

Usiamo ora la precondizione ottenuta come postcondizione per l'altro assegnamento:
$\vdash_{_{ASS}} \{sx=x^{i+1}\}\ \ \ i:=i+1\ \ \  \{sx=x^i\}$

A questo punto, osservando che la precondizione della prima tripla derivata è la postcondizione della seconda, possiamo usare la regola della sequenza per derivare $\vdash_{_{SEQ}} \{sx = x^{i+1}\} \ \ \ C\ \ \ \{sx = x^i\}$.

Notiamo però che i due assegnamenti sono indipendenti, ovvero operano su variabili distinte; questo significa che avremmo anche potuto applicare la regola dell'assegnamento contemporaneamente ad entrambi gli assegnamenti ottenendo lo stesso risultato.

A questo punto abbiamo una tripla con la postcondizione che ci serviva ma con la precondizione che non coincide esattamente con quella della tripla da dimostrare, quindi dobbiamo manipolarla per ricondurla a quella che ci serve.
Notiamo che $sx = x^{i+1}\  \implies\ sx = x^ix\ \implies\ s = x^i$
Otteniamo quindi $\vdash\{s=x^i\} \ \ \ C\ \ \ \{s=x^i\}$.

A noi però serve derivare la precondizione $s=x^i \land i<N$, ma osserviamo che $s=x^i \land i \rightarrow s=x^i$, quindi possiamo derivare, tramite la regola dell'implicazione
$\vdash_{_{IMPL}} \{s=x^i \land i < N\} \ \ \ C\ \ \ \{s=x^i\}$

Abbiamo quindi dimostrato l'invariante e derivato la premessa per la regola dell'iterazione.
Possiamo ora applicare tale regola $\vdash_{_{ITER}} \{s=x^i\}\ \ \ W\ \ \ \{s=x^i \land i \geq N\}$.



**2. RAFFORZAMENTO INVARIANTE**
Notiamo però che quello che vogliamo derivare noi ha una postcondizione $s = x^N$, mentre quello che abbiamo ricavato con la regola dell'iterazione è più debole: abbiamo ricavato che $s=x^i$ e che $i 	\geq N$.
Per avere la postcondizione voluta dobbiamo dimostrare che al termine dell'esecuzione $i = N$, ma l'invariante trovato non è sufficiente a questo scopo e quindi dobbiamo *rafforzarlo*.

Per farlo, osserviamo che anche $i \leq N$ è un invariante, quindi ipotizziamolo:
$?\ \ \ \{i\leq N \land i < N\}\ \ \ C\ \ \ \{i \leq N\}$          Notiamo che $i\leq N \land i < N \equiv i<N$

Usiamo sempre la regola dell'assegnamento:
$\vdash_{_{ASS}}\{i+1 \leq N\}\ \ \ C\ \ \ \{i \leq N\}$

Notiamo che $i < N \rightarrow i+1 \leq N$:
$\vdash_{_{IMPL}}\{i < N\}\ \ \ C\ \ \ \{i \leq N\}$

Possiamo ora derivare, tramite la regola dell'iterazione:
$\vdash_{_{ITER}}\{i\leq N\}\ \ \ W\ \ \ \{i \leq N \land i\geq N\}$          Notiamo che $i\leq N \land i \geq N \equiv i = N$

Ora combiniamo i due invarianti:
$\vdash_{_{ITER}}\{s=x^i \land i \leq N\}\ \ \ W\ \ \ \{s=x^i \land i\leq N \land i \geq N\}$, ovvero
$\vdash\{s=x^i \land i \leq N\}\ \ \ W\ \ \ \{s=x^i \land i=N\}$, ovvero
$\vdash\{s=x^i \land i \leq N\}\ \ \ W\ \ \ \{s=x^N\}$



**3. ASSEGNAMENTI INIZIALI**
A questo punto ci rimane solo da dimostrare che dopo gli assegnamenti iniziali valgono gli invarianti
$?\ \ \ \{N \geq 0\}\ \ \ A\ \ \ \{s=x^i \land i \leq N\}$

Usiamo la regola dell'assegnamento:
$\vdash_{_{ASS}}\{1= x^i \land i \leq N\}\ \ \ s:=1\ \ \ \{s=x^i\land i \leq N\}$

$\vdash_{_{ASS}}\{1 = x^0 \land 0 \leq N\}\ \ \ i:=0\ \ \ \{1 = x^i \land i \leq N\}$, ovvero
$\vdash\{N\geq0\}\ \ \ i:=0\ \ \ \{1 = x^i \land i \leq N\}$

A questo punto usiamo la regola della sequenza:
$\vdash_{_{SEQ}}\{N\geq 0\}\ \ \ A\ \ \ \{s=x^i \land i \leq N\}$



**4. DIMOSTRAZIONE FINALE**
A questo punto possiamo combinare, con la regola della sequenza, le triple $\{N\geq 0\}\ \ \ A\ \ \ \{s=x^i \land i \leq N\}$ e $\{s=x^i \land i \leq N\}\ \ \ W\ \ \ \{s=x^N\}$, ottenendo
$\vdash_{_{SEQ}}\{N \geq 0\}\ \ \ P\ \ \ \{s = x^N\}$, dimostrando così la tripla iniziale.

<div style="page-break-after: always;"></div>

##### ITERAZIONE - DIVISIONE

Consideriamo il programma $P$ che calcola quoziente e resto di $x/y$

```pascal
quo := 0; rem := x;		//A
while rem >= y do		//W
	rem := rem - y;	//C	//W
	quo := quo + 1;	//C	//W
end-while				//W
```

Vogliamo dimostrare $?\ \ \ \{x \geq 0 \land y \geq 0\}\ \ \ P\ \ \ \{x = quo*y +rem \land rem \geq 0 \land rem < y \}$

Osservazione: $rem < y \equiv \lnot B$

Notiamo che siamo in presenza di un ciclo, cerchiamo dunque un'invariante:
Simuliamo qualche passo con $x=26$ e $y=5$
$quo\ \ \ \ \ 0\ \ \ 1\ \ \ \ \ 2\ \ \ \ 3\ \ \ \ 4\ \ 5$
$rem\ \ \ 26\ \ 21\ \ 16\ \ 11\ \ \ 6\ \ 1$

Osserviamo sia la tabella che la postcondizione di interesse.



**1. DIMOSTRAZIONE INVARIANTE**
Ipotesi: $x = quo*y + rem \land rem \geq 0$ è invariante.
$?\ \ \ \{x=quo*y + rem \land rem \geq 0 \land rem \geq y\}\ \ \ C\ \ \ \{inv\}$ 

Usiamo la regola dell'assegnamento:
$\vdash_{_{ASS}}\{x=(quo+1)*y+rem-y \land rem - y \geq 0\}\ \ \ C\ \ \ \{inv\}$, ovvero
$\vdash\{x=quo*y+rem \land rem \geq y\}\ \ \ C\ \ \ \{inv\}$



Sappiamo però che $y \geq 0$, quindi
   $\vdash \{x=quo*y +rem \land rem \geq 0\}\ \ \ C\ \ \ \{inv\}$

A questo punto possiamo usare la regola dell'iterazione:
   $\vdash_{_{ITER}} \{x=quo*y +rem\land rem \geq 0\}\ \ \ W\ \ \ \{x = quo *y +rem \land rem \geq 0 \land rem < y\}$

In questo caso abbiamo già ottenuto la postcondizione che ci interessa.



**2. ASSEGNAMENTI INIZIALI**
Per concludere la dimostrazione dobbiamo ora dimostrare che l'invariante vale dopo gli assegnamenti iniziali:
$\vdash_{_{ASS}} \{x = x \land x \geq 0 \}\ \ \ A\ \ \ \{x=quo*y + rem \land rem \geq 0\}$, ovvero
$\vdash \{x \geq 0 \}\ \ \ A\ \ \ \{x=quo*y + rem \land rem \geq 0\}$


Notiamo che $x \geq 0 \land y \geq 0 \rightarrow x\geq 0$, quindi
$\vdash_{_{IMPL}} \{x \geq 0 \land y\geq 0\} \ \ \ A\ \ \ \{x=quo*y +rem \land rem \geq 0\}$


Dunque, l'invariante è valido dopo gli assegnamenti iniziali.



**3. DIMOSTRAZIONE FINALE**
   Ora possiamo comporre il tutto con la regola della sequenza e terminare la dimostrazione
   $\vdash_{_{SEQ}}\{x\geq 0 \land y \geq 0\}\ \ \ P\ \ \ \{x=quo*y+rem \land rem \geq 0 \land rem < y\}$

##### CORRETTEZZA TOTALE - ESEMPIO

Consideriamo il programma $P$

```pascal
while x > 5 do
	x := x - 1;
end-while
```

Supponiamo di voler dimostrare $?\ \ \ \{x > 5\}\ \ \ P\ \ \ \{x=5\}$

Dobbiamo cercare un variante $E$ il cui valore decresce ad ogni esecuzione dell'iterazione e un invariante $inv$ che garantisca che $E$ abbia sempre valore maggiore o uguale a $0$.

In questo caso possiamo usare $E = x - 5$ e $inv = x \geq 5$. Osserviamo che $inv \rightarrow E \geq 0$.

Dimostriamo l'invariante:
$?\ \ \ \{x\geq5 \land x >5\}\ \ \ C\ \ \ \{x \geq 5\}$

Usiamo la regola dell'assegnamento:
$\vdash_{_{ASS}} \{x \geq 6\}\ \ \ x:=x-1\ \ \ \{x \geq 5\}$

Osserviamo che $x \geq 5 \land x >5  \rightarrow x \geq 6$:
$\vdash_{_{IMPL}} \{x \geq 5 \land x > 5\} \ \ \ C\ \ \ \{x \geq 5\}$ dunque l'invariante è valido.



Ora dobbiamo dimostrare $?\ \ \ \{inv \land B \land x-5 = k\}\ \ \ x:=x-1\ \ \ \{inv \land x-5 < k\}$

Applichiamo la regola dell'assegnamento:
$\vdash_{_{ASS}} \{x-1\geq 5 \land x-1-5 < k\}\ \ \ x:=x-1\ \ \ \{x \geq 5 \land x-5 < k\}$ ovvero
$\vdash \{x\geq 6 \land x-6 < k\}\ \ \ x:=x-1\ \ \ \{x \geq 5 \land x-5 < k\}$

Notiamo che $x \geq 5 \land x > 5 \land x-5 = k \rightarrow x \geq 6 \land x-6 <k$, poiché se $x >5$, allora sicuramente $x >= 6$ visto che lavoriamo con interi e se $x-5 = k$ sicuramente $x-6 <k$.
$\vdash_{_{IMPL}} \{x\geq 5 \land x> 5 \land x-5 <k\}\ \ \ x:=x-1\ \ \ \{x\geq5 \land x-5 < k\}$


A questo punto possiamo usare la regola dell'iterazione totale
$\vdash_{_{ITER}}^{^{TOT}} \{x \geq 5\}\ \ \ P\ \ \ \{x\geq5 \land x \leq 5\}$



**E se $B$ fosse $x \neq 5$?**
Proviamo a vedere se l'invariante di prima è ancora valido
Dovremmo quindi dimostrare $?\ \ \ \{x \geq 5 \land x \neq 5\}\ \ \ C\ \ \ \ \{x \geq 5\}$

Usiamo sempre la regola dell'assegnamento
$\vdash_{_{ASS}}\{x\geq6\}\ \ \ x:=x-1\ \ \ \{x \geq 5\}$

Osserviamo che anche in questo caso $x \geq 5 \land x \neq 5 \rightarrow x \geq 6$, quindi
$\vdash_{_{IMPL}} \{x\geq 5 \land x \neq5\}\ \ \ C\ \ \ \{x\geq 5\}$, quindi l'invariante è ancora valido.

Dimostriamo ora $?\ \ \ \{x\geq 5 \land x\neq 5 \land x-5 = k\}\ \ \ C\ \ \ \{inv \land x-5 < k\}$
Applichiamo la regola dell'assegnamento:
$\vdash_{_{ASS}}\{x \geq 6 \land x-6 < k\}\ \ \ x:=x-1\ \ \ \{x\geq 5 \land x-5 < k\}$

Notiamo che $x\geq 5 \land x \neq 5 \land x-5 = k \rightarrow x \geq 6 \land x-6 < k$, quindi
$\vdash_{_{IMPL}} \{x\geq 5 \land x \neq 5\land x-5 = k\}\ \ \ C\ \ \ \{x \geq 5 \land x-5 < k\}$

A questo punto possiamo usare la regola dell'iterazione totale
$\vdash_{_{ITER}}^{^{TOT}} \{x \geq 5\}\ \ \ P\ \ \ \{x\geq5 \land x \leq 5\}$

##### CORRETTEZZA TOTALE - VARIABILE NON DECREMENTA

Consideriamo il programma $P$

```pascal
while x < 5 do
	x := x + 1
end-while
```

Supponiamo di voler dimostrare $?\ \ \ \{x < 5\}\ \ \ P\ \ \ \{x=5\}$

Proviamo ad usare $inv = x \leq 5$ ed $E = 5-x$, notando che $inv \rightarrow E \geq 0$.

Dobbiamo dimostrare l'invariante, quindi
$?\ \ \ \{x \leq 5 \land x <5\}\ \ \ x:=x+1\ \ \ \{x\leq 5\}$

Usiamo la regola dell'assegnamento:
$\vdash_{_{ASS}} \{x \leq 4\}\ \ \ x:=x+1\ \ \ \{x\leq 5\}$

Notiamo che $x\leq 5 \land x <5 \rightarrow x \leq 4$, quindi
$\vdash_{_{IMPL}} \{x\leq 5 \land x < 5\}\ \ \ C\ \ \ \{x\leq 5\}$, quindi l'invariante è valido.




   Ora dobbiamo dimostrare la seconda premessa della regola dell'iterazione totale, ovvero $?\ \ \ \{x \leq 5 \land x < 5 \land 5-x = k\}\ \ \ x:=x+1\ \ \ \{x \leq 5 \land 5-x < k\}$

   Usiamo la regola dell'assegnamento:
   $\vdash_{_{ASS}} \{x\leq 4\land 4-x < k\}\ \ \ x:=x+1\ \ \ \{x\leq 5 \land 5-x < k\}$

   Notiamo che $x\leq5 \land x <5 \land 5-x = k \rightarrow x\leq 4 \land 4-x< k$, poiché se $x <5$, allora sicuramente $x \leq 4$ perché lavoriamo con gli interi e se $5-x =k$ allora sicuramente $4-x < k$:
   $\vdash_{_{IMPL}} \{x \leq 5 \land x < 5 \land 5-x=k\}\ \ \ C\ \ \ \{x \leq5 \land 5 -x <k\}$

   

   Possiamo ora usare la regola dell'iterazione totale:
   $\vdash_{_{ITER}}^{^{TOT}} \{x \leq 5\}\ \ \ P\ \ \ \{x\leq5 \land x \geq 5\}$

   Notiamo che $x\leq 5 \land x \geq 5 \rightarrow x=5$, quindi
   $\vdash_{_{IMPL}} \{x \leq 5\}\ \ \ P\ \ \ \{x =5\}$

  <div style="page-break-after: always;"></div>

##### CORRETTEZZA TOTALE - ESPONENZIALE

Consideriamo il programma $P$:

   ```pascal
   p := 1; y := b;		//A
   while y > 0 do		//W
   	p := p * x;	//C	//W
   	y := y - 1;	//C	//W
   end-while			//W
   ```

Vogliamo dimostrare $?\ \ \ \{b \geq 0\}\ \ \ P\ \ \ \{p = x^b\}$

Notiamo subito che si tratta di un assegnamento e di un ciclo, cerchiamo quindi un'invariante simulando qualche iterazione:
   $p\ \ \ 1\ \ \ \ \ \ x\ \ \ \ \  \ \ \ \ x^2\ \ \ \ \ \ \ x^3\ \ \ \ \ \ \ x^4$
   $y\ \ \ b\ \ \ b-1\ \ \ b-2\ \ \ b-3\ \ \ b-4$

Notiamo che ad ogni passaggio $p = x^{b-y}$.

Abbiamo visto nel primo esempio di iterazione come possa capitare che un invariante semplice possa non bastare a dimostrare la validità di un ciclo, rafforziamolo da subito quindi con $y \geq 0$, che è un invariante *"garantito"* dalla condizione del ciclo.

   

**1. DIMOSTRAZIONE INVARIANTE**
  Dobbiamo ora dimostrate l'invariante trovato, dobbiamo cioè dimostrare la tripla
  $?\ \ \ \{p=x^{b-y} \land y \geq 0 \land y > 0 \}\ \ \ C\ \ \ \{p=x^{b-y} \land y \geq 0\}$

Notiamo subito che $C$ consiste di due assegnamenti indipendenti, possiamo quindi usare direttamente la regola dell'assegnamento su entrambi contemporaneamente:
   $\vdash_{_{ASS}}\{px=x^{b-(y-1)} \land y -1 \geq 0\}\ \ \ C \ \ \ \{p=x^{b-y} \land y \geq 0\}$
   $\vdash\{p = x^{b-y} \land y \geq 1\}\ \ \ C\ \ \ \{p=x^{b-y}\land y \geq 0\}$

Notiamo che $p=x^{b-y} \land y \geq 0 \land y > 0 \rightarrow p=x^{b-y} \land y \geq 1$, poiché, lavorando con interi, se $y>0$ *(che è più forte rispetto a $y\geq 0$)* allora sicuramente $y \geq 1 $:
   $\vdash_{_{IMPL}} \{p=x^{b-y} \land y\geq 0 \land y > 0\}\ \ \ C\ \ \ \{p=^{b-y} \land y \geq 0\}$, dunque l'invariante è valido.

Possiamo ora applicare la regola dell'iterazione parziale:
   $\vdash_{_{ITER}}^{^{PART}} \{p=x^{b-y} \land y \geq 0\}\ \ \ W\ \ \ \{p=x^{b-y}\land y\geq 0 \land y \leq 0\}$

Notiamo che $p=x^{b-y} \land y \geq 0 \land y \leq 0 \rightarrow p=x^{b-y} \land y = 0 \rightarrow p= x^b$, quindi possiamo derivare
   $\vdash_{_{IMPL}} \{p=x^{b-y} \land y \geq 0\}\ \ \ W\ \ \ \{p=x^b\}$

   

**2. ASSEGNAMENTI INIZIALI**
Adesso dobbiamo dimostrare che la premessa della tripla appena derivata sia valida dopo gli assegnamenti iniziali usando gli assegnamenti e la loro postcondizione $p=x^{b-y} \land y \geq 0$:
   $\vdash_{_{ASS}} \{1 = x^{b-b} \land b \geq 0\}\ \ \ A\ \ \ \{p=x^{b-y} \land y \geq 0\}$
   $\vdash \{b\geq 0\}\ \ \ A\ \ \ \{p=x^{b-y} \land y \geq 0\}$

   

**3. DIMOSTRAZIONE FINALE**
Possiamo infine dimostrare tutto il programma utilizzando la regola della sequenza per unire le due regole ottenute, osservando che la postcondizione di una è esattamente la precondizione dell'altra:
   $\vdash_{_{SEQ}} \{b \geq 0\}\ \ \ P\ \ \ \{p=x^b\}$

   

**4. DIMOSTRAZIONE TOTALE**
Cerchiamo ora di dimostrare anche la correttezza totale del ciclo; per farlo cerchiamo una variante che diminuisca ad ogni iterazione e il cui valore $\geq 0$ sia implicato dall'invariante.

Proviamo a impostare $E = y$, notando che $p=x^{b-y} \land y \geq 0 \rightarrow E \geq 0$:
Per dimostrare che sia valido come variante dobbiamo dimostrare la tripla
   $?\ \ \ \{p=x^{b-y} \land y \geq 0 \land y>0\land y=k\}\ \ \ C\ \ \ \{p=x^{b-y} \land y \geq 0 \land y < k\}$

Usiamo la regola dell'assegnamento:
   $\vdash_{_{ASS}} \{px = x^{b-(y-1)} \land y-1 \geq 0 \land y-1 < k\}\ \ \ C\ \ \ \{p=x^{b-y} \land y\geq 0 \land y < k\}$
   $\vdash \{p = x^{b-y} \land y \geq 1 \land y-1 < k\}\ \ \ C\ \ \ \{p=x^{b-y} \land y \geq 0 \land y < k\}$

   

Notiamo che $p=x^{b-y} \land y \geq 0 \land y > 0 \land y = k \rightarrow p=x^{b-y} \land y \geq 1 \land y-1 < k$, poiché, lavorando con gli interi, se $y>0$, allora sicuramente $y \geq 1$ e se $y=k$ allora sicuramente $y-1 < k$:
   $\vdash_{_{IMPL}} \{p = x^{b-y} \land y\geq0 \land y >0 \land y = k\}\ \ \ C\ \ \ \{p=x^{b-y}\land y \geq 0 \land y <k\}$
Dunque il nostro variante è valido.

   

A questo punto possiamo applicare la regola dell'iterazione totale ottenendo:
   $\vdash_{_{ITER}}^{^{TOT}} \{p=x^{b-y} \land y \geq 0\}\ \ \ W\ \ \ \{p=x^{b-y} \land y \geq 0\land y \leq 0\}$

   

A questo punto dovremmo dimostrare che la precondizione di questa tripla vale dopo gli assegnamenti iniziali e ricondurci alla tripla iniziale, che è esattamente quello che abbiamo fatto prima facendo la dimostrazione parziale.

  <div style="page-break-after: always;"></div>

##### CORRETTEZZA TOTALE - FATTORIALE

Consideriamo il programma $P$ e la tripla $?\ \ \ \{x \geq 0\} \ \ \ P\ \ \ \{?\}$ *(bisogna stabilire cosa calcola il programma)*

```pascal
z := 1;			//A
while x > 0 do		//W
	z := z * x;	//C	//W
	x := x - 1;	//C	//W
end-while			//W
```

Notiamo che il programma calcola di fatto il fattoriale di $x$, ma non possiamo scrivere $?\ \ \ \{x\geq 0\} \ \ \ P\ \ \ \{z=x!\}$, poiché alla fine dell'esecuzione $x$ varrà 0. Introduciamo quindi una variabile ausiliaria $x_0$ che *"blocchi"* il valore iniziale di $x$: $?\ \ \ \{x=x_0 \land x \geq 0\}\ \ \ P\ \ \ \{z = x_0!\}$

Anche in questo caso si tratta di un ciclo e un assegnamento, quindi dobbiamo cercare un invariante. Simuliamo qualche iterazione:
$z\ \ \ 1\ \ \ \ \ \ \ \ \ \ \ x_0\ \ \ \ \ \ \ \ \ x_0(x_0-1)\ \ \ \ \ \ x_0(x_0-1)(x_0 -2)$
$x\ \ \ x_0\ \ \ \ \ x_0 -1 \ \ \ \ \ \ \ \ x_0-2\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ x_0-3$

Prendiamo l'ultimo passaggio e osserviamo che $z = \frac{x_0(x_0-1)(x_0-2)(x_0-3)!}{(x_0-3)!}$
Osserviamo anche che quello che c'è sopra alla frazione non è altro che $x_0!$, mentre quello che c'è sotto è sostanzialmente $x$.

Possiamo dire allora che, in generale, $z = \frac{x_0!}{x!}$. Aggiungiamo all'invariante anche $x \geq 0$ per rafforzarlo *(visto che anche $x \geq 0$ è invariante)*.



**1. DIMOSTRAZIONE INVARIANTE**
Dobbiamo ora dimostrare la validità dell'invariante proposto, ovvero dimostrare la tripla
$?\ \ \ \{z = \frac{x_0!}{x!} \land x \geq 0 \land x > 0\}\ \ \ C\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0\}$

Usiamo la regola dell'assegnamento, ma stiamo attenti: ora gli assegnamenti non sono più indipendenti, quindi dobbiamo gestirli separatamente:

1. $\vdash_{_{ASS}}\{z=\frac{x_0!}{(x-1)!} \land x-1 \geq 0\} \ \ \ x:= x-1\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0\}$
2. $\vdash_{_{ASS}} \{zx=\frac{x_0!}{(x-1)!} \land x-1 \geq 0\}\ \ \ z := z * x \ \ \ \{z=\frac{x_0!}{(x-1)!} \land x-1 \geq 0\}$
   $\vdash \{z=\frac{x_0!}{x!} \land x -1 \geq 0\}\ \ \ z:=z*x\ \ \ \{z=\frac{x_0!}{(x-1)!} \land x-1 \geq 0\}$

A questo punto usiamo la regola della sequenza per unire le due triple, notando che la precondizione della prima è la postcondizione della seconda:
$\vdash_{_{SEQ}} \{z=\frac{x_0!}{x!} \land x-1 \geq 0\} \ \ \ C\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0\}$

Notiamo che $z=\frac{x_0!}{x!} \land x \geq 0 \land x > 0 \rightarrow z=\frac{x_0!}{x!} \land x-1 \geq 0$, quindi:
$\vdash_{_{IMPL}} \{z=\frac{x_0!}{x!} \land x\geq 0 \land x >0\}\ \ \ C\ \ \ \{z=\frac{x_0!}{x!} \land x\geq 0\}$
E quindi l'invariante proposto è valido.



Possiamo ora usare la regola dell'iterazione parziale:
$\vdash_{_{ITER}}^{^{PART}} \{z=\frac{x_0!}{x!} \land x \geq 0\}\ \ \ W\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0 \land x \leq 0\}$

Notiamo adesso che $z=\frac{x_0!}{x!} \land x\geq 0 \land x \leq 0 \rightarrow z=\frac{x_0!}{x!} \land x = 0 \rightarrow z = x0!$:
$\vdash_{_{IMPL}} \{z=\frac{x_0!}{x!} \land x \geq 0\}\ \ \ W\ \ \ \{z = x_0!\}$



**2. ASSEGNAMENTO INIZIALE**
Ora dobbiamo dimostrare che la precondizione della tripla ottenuta sia valida dopo l'assegnamento iniziale:
$\vdash_{_{ASS}} \{1 = \frac{x_0!}{x!} \land x \geq 0\}\ \ \ A\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0\}$
$\vdash \{x = x_0 \land x\geq 0\}\ \ \ A\ \ \ \{z=\frac{x_0!}{x!} \land x\geq 0\}$



**3. DIMOSTRAZIONE FINALE**
A questo punto possiamo unire le triple derivate con la regola della sequenza e concludere la dimostrazione:
$\vdash_{_{SEQ}} \{x=x_0\land x \geq 0\}\ \ \ P\ \ \ \{z = x_0!\}$



**4. DIMOSTRAZIONE TOTALE**
Cerchiamo ora di dimostrare anche la correttezza totale del ciclo; per farlo cerchiamo una variante che diminuisca ad ogni iterazione e il cui valore $\geq 0$ sia implicato dall'invariante.

Proviamo ad impostare $E = x$, osservando che $z=\frac{x_0!}{x!} \land x \geq 0 \rightarrow E=x \geq 0$.
Per dimostrare la validità del variante dobbiamo dimostrare la tripla $?\ \ \ \{z=\frac{x_0!}{x!} \land x\geq 0 \land x > 0 \land x = k\}\ \ \ C\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0 \land x < k\}$

Usiamo quindi la regola dell'assegnamento:

1. $\vdash_{_{ASS}} \{z=\frac{x_0!}{(x-1)!} \land x-1 \geq 0 \land x-1 < k\}\ \ \ x:=x-1\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0 \land x <k\}$
2. $\vdash_{_{ASS}} \{zx = \frac{x_0!}{(x-1)!} \land x -1 \geq 0 \land x-1 < k\}\ \ \ z := z*x\ \ \ \{z=\frac{x_0!}{(x-1)!} \land x-1 \geq 0 \land x-1 < k\}$
   $\vdash \{z=\frac{x_0!}{x!} \land x -1 \geq 0 \land x-1 < k\}\ \ \ z:=z*x\ \ \ \{z=\frac{x_0!}{(x-1)!} \land x-1 \geq 0 \land x-1 <k\}$

Usiamo ora la regola della sequenza:
$\vdash_{_{SEQ}} \{z=\frac{x_0!}{x!} \land x \geq 1\land x-1 < k\}\ \ \ C\ \ \ \{z=\frac{x_0!}{x!} \land x\geq 0 \land x < k\}$



Notiamo ora che $z=\frac{x_0!}{x!} \land x\geq 0\land x >0 \land x =k \rightarrow z=\frac{x_0!}{x!} \land x \geq 1 \land x-1 <k$, poiché se $x>0$, allora sicuramente $x \geq 1$ e se $x =k$ sicuramente $x-1 < k$:
$\vdash_{_{IMPL}} \{z=\frac{x_0!}{x!} \land x\geq 0\land x > 0 \land x = k\}\ \ \ C\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0 \land x < k\}$



Da qui possiamo applicare la regola dell'iterazione totale:
$\vdash_{_{ITER}}^{^{TOT}} \{z=\frac{x_0!}{x!} \land x \geq 0\}\ \ \ W\ \ \ \{z=\frac{x_0!}{x!} \land x \geq 0 \land x \leq 0\}$



Infine per terminare la dimostrazione dovremmo mostrare che la precondizione della tripla ottenuta è valida anche dopo il primo assegnamento e concatenare il tutto come fatto prima.

