## BUGS

- **FAILURE** *(malfunzionamento)*: Risultato non corretto ottenuto dall'esecuzione di un programma. Dipende più dal comportamento del programma che dal codice *(ad esempio in assenza di informazioni su quale sia il codice)*.
- **FAULT** *(difetto)*: Un problema nel codice. Una condizione ***necessaria*** *(ma non sufficiente)* per generare un failure.
- **ERROR** *(errore)*: Ciò che causa un fault. Ad esempio l'errore dell'umano che ha implementato il codice o un problema con un documento legato ad esso.

**OBBIETTIVI DEI TEST**:

- Rivelare i difetti mostrando dei malfunzionamenti.
- Fornire **confidenza** della *(probabile)* correttezza del software.
  Il ***livello di confidenza*** è dato dal numero di test; più sono, più campi vengono toccati e quindi più è alto. Per fornirlo dobbiamo trovare un range di input valido che garantisca il funzionamento a livello generale.



## TERMINOLOGIA

- **TEST OBLIGATIONS *(OBJECTIVE)***: Una parziale specifica di casi di test che richiede alcune proprietà ritenute importanti per il testing; sostanzialmente è l'obiettivo che si pone un insieme di test.
- **TEST CASE**: Singola prova. Un insieme di input, condizioni di esecuzione e un criterio per definire test passati/non passati.
- **TEST SUITE**: Un insieme di test cases.
- **TEST CATALOG**: Una guida per identificare le test obligations per una classe ben identificata di elementi.
- **ADEQUACY CRITERION**: Un predicato che risulta essere *True* o *False* per una coppia *<Programma, Test Suite>*. Sostanzialmente è un metodo per stabilire, data una test suite, se essa è sufficiente per testare un programma o meno.



## DA DOVE VENGONO LE TEST OBLIGATION?

- **FUNZIONALI** *(Black box, basati sulle specifiche)*: Ottenute dalle specifiche del software (***conoscenza del dominio***) senza saperne l'implementazione effettiva.
  *Esempio*: Se una specifica richiede una qualche forma di recovery da un blackout, una test obligation dovrebbe includere la simulazione di un blackout.
- **STRUTTURALI** *(White o Glass box)*: Ottenute dal codice.
  *Esempio*: Eseguire ogni loop del programma una o più volte.

O ancora *(non trattati nel corso)*

- **MODEL-BASED**: Ottenute dal modello del sistema, ottenuti nel design o dal codice.
- **FAULT-BASED**: Ottenute da fault ipotizzati *(bug comuni come ad esempio il buffer overflow)*.

<div style="page-break-after: always;"></div>

## SELTA DEI TEST

*Perché abbiamo scelto un certo test?*
*Sono sufficienti i vari casi di test proposti? Si/No? Perché?*
*Testare con 3, 5 o 10 input differenti cambia qualcosa? Si/No? Perché?*
Chiunque proponga un test case deve saper rispondere a queste domande.



**PERCHÉ ABBIAMO SCELTO UN CERTO TEST?**
Il comportamento di un sistema viene analizzato basandosi sulle sue specifiche, che devono essere definite per ogni funzionalità del sistema da testare.
Secondo il **Boundary Testing**, in generale abbiamo tre categorie di casi di test:

- Casi scelti in quanto ***errori sicuri***.
- Casi scelti in quanto mostrano il ***normale funzionamento del sistema***.
- Casi ***bound*** *(estremi o di confine)*, ovvero i casi che sono tra l'errore e il corretto funzionamento *(es. ho un programma che accetta in input una sequenza di esattamente 6 cifre e ne inserisco 5 e/o 7)*.



**SONO SUFFICIENTI I VARI CASI DI TEST PROPOSTI?**
Per decidere se dei casi di test sono sufficientemente buoni per decidere se un programma è *(sufficientemente)* corretto, si utilizza un ***criterio di adeguatezza***, che non è altro che un set di test obligations.

Una test suite soddisfa un criterio di adeguatezza se:

- Tutti i test hanno successo *(passano)*.
- Ogni test obligation nel criterio è soddisfatto da almeno uno dei test nella test suite.

I criteri funzionali e strutturali sono ***complementari*** *(e quindi non mutualmente esclusivi)*.



**Criteri funzionali**: correttezza basata sulle specifiche del programma.
*Esempio*:

```java
int fun(int param) {
    return param / 2;
}
```

*<ins>Specifiche</ins>*: Un programma che prende in input un intero $N$ e restituisce $N/2$ se l'input è pari, altrimenti restituisce solo $N$.
È chiaro, osservando il codice, che questa implementazione funziona solo per i numeri pari; la ***failure*** quindi può rimanere nascosta se eseguiamo solo dei test strutturali, che si basano quindi solo sul codice.
Se eseguiamo dei **test funzionali** invece possiamo identificare la ***failure*** molto facilmente.
Considerando che il metodo in questione funziona bene dal punto di vista del codice, siamo di fronte ad un errore di ***missing logic***.

Generalmente si parte sempre dai test basati su criteri funzionali in quanto possono essere creati prima del termine dell'implementazione stesa del codice.



**Criteri strutturali**: correttezza basata sull'implementazione del programma.
*Esempio*:

```dart
void fun(int param) {
    if (param < 100) print(param * 2);
    else if (param < 1000) print(param);
    else print("too much!");
}
```

*<ins>Specifiche</ins>*: Un programma che prende in input un numero $N$ e stampa $2N$ se $N$ è minore 100, altrimenti stampa $N$.
Possiamo subito notare che l'implementazione non è corretta e che quindi c'è un ***fault***.
Un criterio funzionale non indica il bisogno di discriminare tra input <1000 e input $\geq$1000, ma questo è evidente nel codice, di conseguenza un **test strutturale** può identificare facilmente la ***failure***.



**Inadeguatezza**: cerchiamo di evitarla.
*Esempi*:

- *Funzionale*: se le specifiche descrivono un trattamento diverso in un set di casi, ma la test suite non controlla che quei determinati casi siano effettivamente trattati diversamente, possiamo concludere che la test suite è inadeguata ad evitare faults nella logica del programma.
- *Strutturale*: Se nessun test nella test suite esegue un particolare statement del programma, la test suite è inadeguata ad evitare faults nel codice.

In generale, se una test suite non soddisfa alcuni criteri, le obbligazioni non soddisfatte potrebbero fornire informazioni utili a migliorare la test suite, mentre se una test suite soddisfa tutte le obligations, ***non sappiamo definitivamente che è effettiva o meno***, ma abbiamo *qualche* prova di correttezza.



**TESTARE CON 3, 5 O 10 INPUT DIFFERENTI CAMBIA QUALCOSA?**
Iniziamo con l'introdurre due tipi di test: i test **Random** *(uniformi)* e i test **Sistematici** *(non uniformi)*;

**RANDOM**

- Testa i possibili input uniformemente.
- Evita i pregiudizi del designer. Il test designer può tuttavia fare gli stessi errori logici e cattive assunzioni del designer del programma *(specialmente se sono la stessa persona)*.
- Associa a tutti gli input lo stesso valore.

**SISTEMATICO**

- Cerca di selezionare degli input che sono più *valuable* di altri.
- Solitamente lo fa selezionando dei *rappresentati* di alcune classi di input che tendono a fallire spesso o mai. I test funzionali sono un esempio di test sistematici.



**Perché non random?**
Perché nei software le faults non sono distribuite uniformemente; le ***failing values*** sono sparse nello spazio dei possibili input, di conseguenza è molto improbabile che un test random selezioni i valori che ci interessano per effettuare dei test.

Immaginiamo l'insieme dei possibili input come una serie di quadratini come mostrato sotto.

<img src="./img/001.png" />

Assumendo che i **failure** siano densi in certi punti dello spazio di input, cerchiamo, secondo i principi del **test sistematico**, di **partizionare** tale insieme per definire delle classi che potrebbero o meno creare dei **failure**, così da avere un numero finito di elementi da testare.
Testando poi almeno un input nelle varie classi e trovando un **failure**, scopriamo il **fault**.



### FUNCTIONAL TESTING

Un metodo per eseguire il partizionamento descritto sopra è il testing funzionale, conosciuto anche come **specification-based testing** e **black box testing**.
Il testing funzionale utilizza le specifiche del programma per definire queste partizioni dello spazio degli input per poi testare ogni categoria di input e i confini tra di esse *(**boundary testing**)*.

La specifica di un programma può essere lunga e confusionaria ed è per questo che è importante suddividerla in varie parti **testabili indipendentemente** ***(atomiche)*** ragionando su come l'applicazione si comporta in base a vari input, parametri o risultati. Questa parte di scomposizione viene effettuata con due metodi principali:

- **Catalog-Based testing** che aggrega e sintetizza l'esperienza degli altri test designers in un certo dominio, così da aiutare a trovare i valori *interessanti*; radunare esperienza in una collezione sistematica può velocizzare il processo di design, facendo diventare alcune decisioni una routine, automatizzandole per permettere di usare lo sforzo umano maggiormente sull'identificazione delle caratteristiche del sistema.
  In particolare, il metodo catalog based utilizza l'esperienza degli altri designers per elencare per determinate condizioni i casi più importanti dei possibili valori *(e.g. un catalog su una variabile intera potrebbe indicare di testate l'elemento sotto al limite inferiore/superiore, il limite stesso e un elemento nell'intervallo ammissibile)*.
- **Category-Partition testing** che identifica i test objectives e caratterizza l'input space.
  Lo fa scomponendo le specifiche in parti atomiche e identificando per ognuna di esse **parametri** e **variabili d'ambiente** *(file di configurazione, contenuti di database, ecc)*.
  Successivamente si concentra su ogni input/parametro/risultato per individuare delle caratteristiche *(categorie di valori)* che permettono di discriminare il comportamento del programma e applica su ognuna di queste il boundary testing.
  Eventualmente può anche introdurre dei limiti sulle possibili combinazioni di valori.

In seguito, avremo una fase di **aggregazione**, in modo da poter identificare gli input *interessanti*, fatta attraverso il **test combinatorio** che identifica i vari attributi che possono variare *(e.g. un browser potrebbe essere IE oppure Firefox, mentre un OS potrebbe essere Vista o XP)* e genera delle combinazioni che possono essere testate *(e.g. IE con Vista, IE con XP, Firefox con Vista, Firefox con XP)*.
Tuttavia, se abbiamo un grande numero di variabili in gioco, l'approccio combinatorio puro può essere sconveniente, in quanto produrrebbe troppe combinazioni da poterle testare tutte; usando il criterio **pairwise** invece eseguiamo delle combinazioni a coppia *(se il budget lo permette si possono usare anche triple, quadruple ecc)*, riducendo drasticamente il numero di combinazioni da testare senza introdurre dei limiti *(che però potremo introdurre se volessimo)*.



**CONSTRAINTS**
Per ridurre il numero di combinazioni testabili, possiamo anche introdurre dei limiti *(ad esempio per evitare di testare combinazioni impossibili)*.
I constraints si indicano con un'etichetta.

- **[error]**: indica un valore che corrisponde ad un errore e che quindi va testato una volta, ma difficilmente potrà essere combinato con altri valori.
- **\[property\]\[if-property\]**: gestisce le combinazioni invalide di valori. In particolare **\[property\]** identifica un gruppo di valori con proprietà comuni, **\[if-property\]** limita la scelta di un valore che può essere combinato solo con un certo altro valore.
- **\[single\]**: indica un valore che l designer ***sceglie*** di testare una sola volta per ridurre il numero di test cases. Ha lo stesso effetto di *\[error\]*, ma è dettato da una scelta differente; è importante separarli per una corretta documentazione e per il regression testing.



**SOMMARIO**
Le specifiche dei requisiti generalmente sono espresse in linguaggio naturale, cosa che è un ostacolo per l'analisi automatica; il testing funzionale utilizza uno step manuale di **strutturazione** di specifiche in un set di **proprietà** e uno step automatizzabile di produzione delle **combinazioni testabili**.
Il metodo *brute force* è tedioso e porta ad errori; gli approcci combinatori decompongono il lavoro della forza bruta in diverse fasi per attaccare il problema in modo incrementale .

In particolare abbiamo ottenuto:

- Dal **Category Partition** la divisione del lavoro in uno step manuale di identificazione di categorie e proprietà e uno step automatico di generazione di combinazioni.
- Dal **Catalog Based** un miglioramento dello step manuale utilizzando dei pattern standard utili a identificare valori significanti.
- Dal **Pairwise Testing** la generazione sistematica di test suites più piccole.

<div style="page-break-after: always;"></div>

## TEST STRUTTURALE

Il testing strutturale utlizza sia il software che la specifica e, in particolare, estende un insieme di test basandosi su entrambi; difatti le informazioni strutturali di un programma più che aiutarci a decidere come scegliere i test, risultano utili in combinazione con altri test *(funzionali)* per identificare ulteriori test atti ad identificare faults che potrebbero essere sfuggite all'analisi black-box.
Si può dire che il test strutturale è utilizzato per valutare la robustezza e la completezza delle test suite generate dai test funzionali.

Abbiamo visto che per decidere se una test suite è adeguata o meno possiamo usare i criteri di adeguatezza, ma come possiamo definire un criterio di adeguatezza per i test strutturali?
L'idea di base è di considerare ogni ***statement*** del programma come una test obligation e dire che l'adeguatezza di copertura degli statement è soddisfatta se ogni statement del programma è eseguito da almeno un test della test suite e il risultato dei test è *pass*.
Diremo che il **coverage** *(copertura)* di una test suite è dato dal #statement_eseguiti / #statement_totali.
Diremo inoltre che la test suite soddisfa il **criterio di adeguatezza** se questa ha coverage = 1 *(o a 100%)*.

In sostanza, diremo che il **criterio di copertura** è soddisfatto per un programma se testiamo tutte le sue istruzioni *(quindi ad esempio testare tutti i branch degli if)*.
Ovviamente la copertura **non** è direttamente proporzionale al numero di test della test suite, ma dipende più che altro da quante istruzioni eseguono effettivamente i test stessi. Inoltre spesso si preferisce un test rispetto ad un altro in basse alle informazioni che ci da sul tipo di fault rilevato e su dove questo si trovi. 

Osserviamo l'esempio con tre test suite, che solo insieme eseguono tutti gli statement.

<img src="./img/002.png" />

<div style="page-break-after: always;"></div>

## BASIC BLOCK TESTING

Un altro criterio di adeguatezza è il basic bloc testing, che considera dei blocchi di statement massimali che hanno un solo entry point ed un solo exit point *(ad esempio un **if** o un **while**)*. Sotto questa prospettiva, le istruzioni contigue appartengono ad uno stesso blocco finché non si incontra un *if*; gli statement condizionali terminano un blocco.
I cicli inoltre sono dei blocchi a se stanti.
L'idea alla base di questa strategia è quella di suddividere il programma in una serie di **basic blocks** da considerarsi ognuno come una ***test obligation*** e costruire un **Control Flow Graph** ***(CFG)*** che connetta i vari blocchi in base all'esito dello statement condizionale che termina il blocco *(ottenendo di fatto un DAG)*.

Il criterio di adeguatezza a questo punto è simile a quello dello **statement testing**, ovvero è soddisfatto solo quando la test suite esegue tutti i blocchi del CFG almeno una volta.
Difatti, il concetto alla base delle due strategie è lo stesso, la differenza sta nella granularità su cui si lavora: mentre nello statement testing testiamo le singole istruzioni, nel basic block testing testiamo una porzione di codice.

Tuttavia, nessuno dei due ci da alcuna garanzia sull'assenza di fault nel programma. Consideriamo il seguente programma:

```java
int abs(int x) {
    int r = 0;
    if (x < 0) {
        r = -x;
    }
    return r;
}
```

Consideriamo ora la test suite T<sub>A</sub> = { -1 };
Notiamo che la test suite soddisfa i due criteri in quanto esegue tutti gli statement/blocchi del programma, tuttavia non individua il fault causato da un valore di x > 0 in cui il  programma ritorna 0. Questo perché non stiamo considerando tutti i possibili esiti dello statement condizionale.



## BRANCH TESTING

L'idea alla base del branch testing è che un programma ha tanti **branch** *(salti)*; ogni branch allora rappresenta una test obligation e deve essere testato almeno una volta. Come prima, la copertura risulta essere 1 quando la test suite testa effettivamente tutti i branch del programma.
Stiamo quindi praticamente creando un grafo proprio come con il basic block testing, in cui gli archi sono i branch del programma.
Visitando tutti gli archi sono quindi certo di visitare anche tutti i nodi; questo implica che se una test suite soddisfa il criterio del ***branch testing***, essa soddisfa anche il criterio dello ***statement/basick block testing*** *(mentre non è necessariamente vero il contrario)*.

<div style="page-break-after: always;"></div>

## BASIC CONDITION TESTING

Abbiamo visto fino ad ora una categoria di test strutturali basata sugli statements, che espone le faults legate a come una computazione è stata suddivisa i casi.
Tuttavia, perfino il criterio del branch testing può non individuare alcune faults.
Consideriamo il seguente programma che ritorna true a = -1 o b = -1:

```java
boolean findMinusOne(int a, int b) {
    if (a == -1 || b == 1) {
        return true;
    } else {
        return false;
    }
}
```

Consideriamo ora la test suite T<sub>R</sub> = { <2, 3>, <-1, 3> };
Notiamo che seppur essa soddisfi il criterio di adeguatezza del branch testing, non individua il fault **b == 1**.

Abbiamo bisogno quindi di un'altra categoria di test strutturali per individuare questo tipo di faults, i ***condition testing***, di cui il primo esempio è il ***basic condtion testing***.
Il criterio di adeguatezza del basic condition testing stabilisce che, per essere adeguata, una test suite deve eseguire almeno una volta ogni **condizione basica**. Il rapporto sarà quindi dato dal numero di valori di verità delle condizioni che abbiamo ottenuto e dal doppio delle condizioni stesse *(poiché ogni condizione può avere due outcome; T o F)*.
Consideriamo ora il programma di prima

```java
boolean findMinusOne(int a, int b) {
    if (a == -1 || b == -1) {
        return true;
    } else {
        return false;
    }
}
```

e la seguente test suite T<sub>C</sub> = { <2, -1>; <-1, 3> };
Notiamo che essa soddisfa il criterio di adeguatezza del **basic condition testing**, ma non esegue tutto il codice, in particolare non esegue il secondo **branch** del programma.
Possiamo quindi dedurre che il soddisfacimento del criterio del **basic condition testing** ***NON*** implica il soddisfacimento del criterio del **branch testing**.

**BRANCH AND CONDITION**
Una soluzione a questo problema è unire i due criteri.
Consideriamo le seguenti test suite T<sub>A</sub> = { <-1, -1, -1>; <6, 7, 8> } e T<sub>C</sub> = { <-1, 2, 3>; <2, -1, 2>; <6, 7, -1>; <6, 7, 8> }.
Notiamo che entrambe soddisfano i criteri di branch e condition, tuttavia T<sub>C</sub> ci da più informazioni su dove si trovi un fault nel caso lo individuasse.

<div style="page-break-after: always;"></div>

## COMPOUND TESTING

Abbiamo detto che i criteri dei branch e delle basic conditions non sono comparabili in quanto nessuno dei due implica l'altro, dunque abbiamo bisogno di un test che controlli tutte le **combinazioni** di tutte le **condizioni** e percorra tutti i **blocchi di codice**; il ***compound testing*** effettua proprio questo lavoro.
Il criterio di adeguatezza del compound testing risulta soddisfatto solo se la test suite copre tutti i **branch** del programma E se valuta tutti i valori di tutte le **basic conditions**.

**ATTENZIONE**: poiché molti linguaggi di programmazione utilizzano ciò che è chiamata left-to-right/short-circuit evaluation, la quale implica che se abbiamo una catena di OR, non appena uno di essi risulta essere true, non andiamo a testare gli altri.
Discorso analogo vale per gli AND, ma con i false.
Questo ci permette di evitare di considerare tutte le possibili combinazioni in fase di testing.

Per esempio, per la condizione if( a == -1 || b == -1 || c == -1) basta la test suite
T = { <-1, *, *>; <2, -1, *>; <2, 3, -1>; <2, 3, 4>}.

Tuttavia, il compound testing ha un problema: il costo computazionale.
Difatti ha una **crescita esponenziale** dei casi di test da effettuare.
A volte la short-circuit evaluation riduce questo numero in modo che sia gestibile, ma non sempre.



## MODIFIED CONDITION/DECISION

L'idea alla base del MC/DC è di effettuare i test solo per le combinazioni **importanti** di condizioni, in modo da evitare la crescite esponenziale.
Le combinazioni **importanti** sono quelle condizioni che, se cambiate **singolarmente**, portano ad un cambiamento dell'output.



## WHY STRUCTURAL TESTING

Una possibile risposta è perché il test strutturale ci rivela cosa **manca** nella nostra test suite: se una parte di un programma non viene mai eseguita, le fault al suo interno non possono essere scoperte.

Il testing strutturale complementa quello funzionale; esso rappresenta un altro modo per individuare input trattati in modo diverso che però si affida al codice per farlo. Inoltre, il testing strutturale includere casi non scoperti nel testing funzionale, così come il testing funzionale può includere casi non scoperti nel test strutturale.

Tuttavia, eseguire tutti gli elementi del control flow **NON GARANTISCE** l'assenza di fault nel codice, il testing strutturale rimuove le inadeguatezze ovvie, ma non da alcuna garanzia di trovare tutti i fault.

A volte, nessuna test suite può soddisfare un criterio per un programma dato; in questo caso si possono escludere gli statement non raggiungibili *(tuttavia non possiamo sempre sapere per certo quali statement non verranno mai eseguiti)* oppure misurare quanto copre del programma la test suite.



## SECURITY IS NOT SAFETY

Definiamo:

- **SAFETY ENGINEERING**: prevenzione di danni *'catastrofici'* causati dal cattivo funzionamento di un sistema informatico.
- **SECURITY ENGINEERING**: prevenzione, difesa e recupero danni contro la possibilità di attacchi che possono mettere a repentaglio i valori rappresentati da un sistema informatico.

Tutte le misure di **safety** sono anche misure di **security**, mentre non è vero il viceversa.

Un problema di sicurezza, o *vulnerabilità*, può essere viso come una ***via alternativa***, un modo di usare il sistema non pensato dal progettista. Ovviamente più è complesso il sistema, più possono essere le vie alternative.
*Esempio*: un progettista di software di un auto potrebbe pensare che sia utile che le portiere si sblocchino quando la macchina si capovolge. Tuttavia, una via alternativa potrebbe essere far pressione sul tetto per aprire la macchina quando dovrebbe rimanere chiusa.



**CIA TRIAD**

- **Confidenzialità**: fa riferimento al fatto che una risorsa *(o un servizio)* deve essere accessibile solo a chi è autorizzato ad usufruirne.

  Può essere descritto anche come capacità di controllare/assicurare la **privatezza**/**riservatezza** delle informazioni rispetto a soggetti non autorizzati.
  Questo concetto non si applica solo, per esempio, al contenuto di un file, ma anche alla conoscenza dell'esistenza di tale file o meno o anche all'utilizzo di un certo mezzo *(ad esempio la rete)*.

- **Integrità**: fa riferimento al concetto di assicurazione che un'informazione non sia stata modificata e che quindi la risorsa sia fruibile nel modo esatto nel quale è stata resa disponibile.

  Può essere descritto anche come capacità di controllare/assicurare la **non modificabilità**/**attendibilità** da parte di soggetti non autorizzati delle informazioni.
  Il concetto di modificabilità si estende anche alla creazione/rimozione di oggetti *(anche se nel caso di rimozione si parla anche di interruzione di disponibilità)*, oltre che alla manomissione di informazioni.

  Due particolari sotto-casi del principio di integrità sono:

  1. **Autenticazione di mittente**: definisce la capacità di garantire l'identità del mittente.
  2. **Non-ripudio**: definisce la capacità di assicurare che il mittente non possa negare la paternità delle informazioni trasmesse.

- **Disponibilità** *(Availability)*: fa riferimento al fatto che se una risorsa deve essere accessibile a degli utenti, un attacco non deve inibire questo accesso *(esempio un attacco DDoS)*.

  Può essere descritto anche come capacità di controllare/assicurare la **possibilità di usare** risorse e servizi da parte degli utenti autorizzati.
  Alcuni esempi di attacchi alla disponibilità sono il crash di un sistema, la manomissione di algoritmi di scheduling per non far mai eseguire un certo processo o anche la distruzione fisica di un computer.

Gli attacchi alla triade CIA sono solitamente identificati come *DAD*:

- **Disclosure** > Confidentiality
- **Alteration** > Integrity
- **Destruction** > Availability

**USABILITY VS SECURITY**
L'obiettivo di fondo dell'informatica è favorire l'accesso alle risorse, mentre la sicurezza obbliga a limitarne l'accesso. Dunque spesso l'obiettivo della sicurezza è trovare il giusto compromesso tra usabilità e sicurezza del sistema.

Inoltre, spesso la sicurezza viene presa in considerazione solo dopo i primi incidenti; in realtà il compromesso citato sopra andrebbe preso in considerazione fin da subito, trovando un compromesso anche tra benefici e costi, che quindi richiede un'*analisi dei rischi*.



**ANALISI DEI RISCHI**
Rischio: **possibilità **di **perdita** o danno.
Nell'ambito della sicurezza il rischio è calcolabile come **Rischio = Vulnerabilità x Minacce x Valori** dove:

- **VALORI**: solo le parti del sistema informativo importanti, sensibili, critiche, da proteggere.
- **VULNERABILITÀ**: debolezze del sistema, attacchi che possono essere sfruttati per comprometterlo.
- **MINACCE**: circostanze o agenti che possono/vogliono arrecare danni.

Notiamo il fattore moltiplicativo; se le vulnerabilità sono poche, il rischio è basso, viceversa se le minacce sono basse si corrono pochi rischi. Se i valori non sono importanti, anche se ci fosse un attacco la perdita è minima.



![003](./img/003.png)

<div style="page-break-after: always;"></div>

## CRITTOGRAFIA

La crittografia *(dal greco **kryptos**, nascosto)* è una disciplina che studia tecniche per *codificare*/*decodificare* messaggi in modo da garantire la privacy della comunicazione fra due soggetti.
Essa **NON** è la soluzione per tutti i problemi di sicurezza, ma può essere uno strumento importante se implementata in modo corretto.

Generalmente un sistema crittografico fa uso di un algoritmo di crittografia governato da una **chiave** che permetta di garantire la confidenzialità ed integrità del messaggio e che permetta al destinatario di poterlo leggere.

Un sistema crittografico è formato da 5 parti

![004](./img/004.png)

Un primo esempio di sistema crittografico è il **cifrario di Cesare** che consiste nello spostamento delle lettere utilizzando l'aritmetica in modulo n. Avremmo quindi:

- **P**: lettere alfabeto
- **C**: lettere alfabeto
- **K**: numeri da 1 a 25
- **encrypt(k, p)**: carattere a distanza k nell'alfabeto
- **decrypt(k, c)**: carattere a distanza -k nell'alfabeto

Il principale problema di questo sistema è il basso numero di chiavi disponibili, che lo rende molto vulnerabile ad un attacco di forza bruta.



**CRITTOGRAFIA E CRITTOANALISI**

- **Crittografia**: schemi/metodi/algoritmi per la codifica sicura dei messaggi.
- **Crittoanalisi**: rappresenta il tentativo di *violare* la crittografia attraverso lo studio di insiemi di messaggi crittati. È utile per scoprire le debolezze di un algoritmo crittografico o del suo ambiente di funzionamento.
  Si pone come obiettivo non solo quello di scoprire un messaggio, ma anche di scoprire le chiavi usate per la cifratura o addirittura dedurre significati segreti senza necessariamente decifrare i messaggi *(ad esempio guardando la frequenza di invio dei messaggi o la loro lunghezza)*.

Un algoritmo crittografico è violabile se può essere violato da un crittoanalista **a patto di disporre di risorse e tempo sufficienti** *(se non consideriamo il tempo e le risorse, teoricamente, ogni algoritmo è violabile con forza bruta)*.

Gli algoritmi ***degni di fiducia*** solitamente sono basati sulla matematica, analizzati da esperti e in uso da diverso tempo senza essere violati.
Ma devono anche essere **usabili**, ovvero il tempo necessario per crittare/decrittare i messaggi deve essere commisurato alla sicurezza necessaria, le chiavi non devono essere soggetti a requisiti complessi e il processo crittografico non deve essere troppo complesso per l'utente.

Ovviamente la prima cosa che pensiamo alla crittografia è la **confidenzialità** dei messaggi, ma gli algoritmi crittografici vengono usati anche per garantire **integrità** e **non ripudio**, similmente a quanto accade con i documenti fisici.

Gli algoritmi crittografici si dividono in 3 classi:

- **Algoritmi simmetrici a chiave segreta**: viene usata un'unica chiave sia per crittare che decrittare.
- **Algoritmi asimmetrici a chiave pubblica**: vengono usate chiavi diverse per crittare e decrittare.
- **Algoritmi di hashing**: il messaggio stesso è la chiave e servono a garantirne l'integrità.



## SISTEMI SIMMETRICI

Abbiamo detto che gli algoritmi simmetrici usano una stessa chiave per decrittare e crittare, dunque decrypt(k, encrypt(k, p)) = p.
Dunque, per istanziare una comunicazione confidenziale, due soggetti devono conoscere una chiave *k* non nota a nessun altro.

![005](./img/005.png)



Assumendo che mittente e ricevente siano gli unici a conoscenza della chiave:

| REQUISITO                    | SI/NO | MOTIVO                                                       |
| ---------------------------- | :---: | ------------------------------------------------------------ |
| Confidenzialità              |  Si   | Solo chi conosce la chiave segreta può decodificare il messaggio. |
| Integrità                    |  Si   | Una volta crittato, il messaggio non è modificabile prima della decrittazione. Se venisse modificato il messaggio crittato, questo verrebbe corrotto. |
| Autenticazione e non ripudio |  Si   | Solo chi conosce la chiave segreta può crittare e quindi essere mittente del messaggio *(assumendo che il sistema funzioni e che quindi la chiave non venga condivisa)*. |

In questo caso **non è possibile** garantire uno solo di questi requisiti indipendentemente dagli altri.<img src="./img/006.png" alt="006" style="zoom:80%;" align="right" />

Un primo aspetto *controverso* di questi algoritmi riguarda lo scambio della chiave che deve avvenire ovviamente in modo sicuro, ma ne parleremo successivamente.
Un altro aspetto di questo tipo di algoritmi è il numero di chiavi necessarie; per $N$ individui che comunicano in maniera sicura a coppie abbiamo bisogno di $n(n-1)/2$ chiavi *(una per coppia, un numero quadratico)*!



**DES**
Un tipico algoritmo simmetrico è **DES: Data Encryption Standard** che codifica i messaggi in blocchi da 64 bit e che consiste nell'applicare iterativamente per 16 volte una funzione combinatoria ad ogni blocco usando una chiave di 56 bit come uno dei parametri della funzione stessa.

<img src="./img/007.png" alt="007" style="zoom:50%;" align="left" />Ovviamente, essendo un algoritmo simmetrico, lo stesso algoritmo che viene usato per crittare i messaggi viene usato anche per decrittarli. Inoltre, la particolarità di DES è che opera sui **bit**: combina i bit, permuta i bit, somma i bit. Questo rende l'algoritmo particolarmente adatto all'**implementazione su hardware** e quindi è **molto veloce** come algoritmo.

Alla sua uscita, nel 77, DES risultava non violabile con forza bruta *(ci sarebbero voluti 700 anni con la potenza computazionale di allora)*, tuttavia nel 97 venne risolto in circa 4 mesi usando la computazione parallela. Con la potenza computazionale di oggi probabilmente ci vuole poco a violare tale algoritmo.

Come risposta a ciò è stato creato triple DES che cifra 3 volte con 3 chiavi diverse, aumentando notevolmente il tempo stimabile per un attacco di forza bruta.

Un'evoluzione di DES è **AES (Advanced Encryption Standard)** che si basa sulle stesse operazioni di DES ma è estendibile, ovvero permette di usare chiavi di lunghezza variabile *(solitamente vengono usate chiavi da 128, 192 o 256 bit ma si può andare oltre)* e permette di eseguire più cicli di codifica, rendendolo di fatto molto più sicuro.

<div style="page-break-after: always;"></div>

## SITEMI ASIMMETRICI

I sistemi asimmetrici funzionano utilizzando due chiavi diverse per la decrittazione e per la crittazione; in particolare, ogni soggetto è in possesso di:

- **k_pub *(chiave pubblica)***: chiave conosciuta da tutti *(ad esempio pubblicata sul sito web del soggetto)*.
- **k_priv *(chiave privata)***: chiave a conoscenza solo del soggetto.

Nei sistemi asimmetrici si ha che un messaggio codificato con una chiave può essere decodificato solo dall'altra, dunque **decrypt(k_priv, encrypt(k_pub, p)) = p** & **decrypt(k_pub, encrypt(k_priv, p)) = p**.

Chiave pubblica e privata sono ugualmente strutturate, viene scelto *a caso* quale sia pubblica e quale privata.

![008](./img/008.png)



<img src="./img/009.png" alt="009" style="zoom:50%;" align="left" />
Un primo aspetto positivo di questo tipo di sistemi è che le chiavi necessarie per far comunicare N soggetti sono N coppie, si passa quindi da un valore n<sup>2</sup> ad uno 2n.







| REQUISITO                    | SI/NO | MOTIVO                                                       |
| ---------------------------- | :---: | ------------------------------------------------------------ |
| Confidenzialità              |  Si   | Solo chi conosce la chiave segreta può decodificare il messaggio crittato con la corrispondente chiave pubblica. |
| Integrità                    |  Si   | Una volta crittato, il messaggio non è modificabile prima della decrittazione. |
| Autenticazione e non ripudio |  Si   | Solo chi conosce la chiave segreta può essere mittente del messaggio crittato con chiave privata. |

Un altro aspetto interessante è che, in questo caso, posso gestire confidenzialità ed autenticazione in modo separato; se critto con la chiave pubblica, ho la certezza che il messaggio sia **confidenziale** e solo chi possiede la chiave privata possa leggerlo, se critto con la chiave privata , nessun atro può modificare il messaggio e crittarlo nuovamente con la mia chiave privata, quindi ho **integrità** e **autenticazione**.
Se critto il messaggio con la mia chiave privata e successivamente con la chiave pubblica del destinatario, ottengo sia **confidenzialità** *(il destinatario deve usare la sua chiave privata per decrittare il primo livello)* sia **integrità/autenticazione** *(userà la mia chiave pubblica per il secondo livello)*.

**RSA**
Un algoritmo a chiave pubblica è **RSA (Rivest, Shamir, Adleman)** che calcola chiave pubblica e privata come funzioni di due numeri primi molto grandi.
I numeri primi sono scelti casualmente e vengono scartati dopo il calcolo delle chiavi e la sicurezza dell’algoritmo si basa sulla difficoltà di risolvere il problema di fattorizzare numeri molto grandi.

Si basa sul teorema di Eulero che dice che, dati due numeri primi **p** e **q**, con **x** che non ha divisori comuni con *p* e *q*, vale **x<sup>(p-1)(q-1)</sup> = 1 (mod p\*q)**.

<img src="./img/010.png" alt="010" style="zoom:40%;" align="right" />RSA

- sceglie **p** e **q** molto grandi
- calcola **n = p \* q** ed **f = (p - 1)\*(q - 1)**
- sceglie **e**, **d** tali che **e \* d = 1 (mod f)**
- scarta **p**, **q** ed **f**
- usa **k_pub = (e, n)** e **k_priv = (d, n)**





Questo algoritmo è molto difficile da violare con la forza bruta, tuttavia può essere violato in altri modi, ad esempio se il sistema di generazioni usa dei numeri pseudo-casuali facili da determinare.
Un'altra **importante differenza** tra questo tipo di algoritmi e quelli simmetrici e che quelli asimmetrici sono molto più difficili da implementare e richiedono quindi più tempo per messaggi molto grandi.



## MESSAGE DIGEST

Un **message digest** è un '*riassunto*' di un messaggio, un checksum crittografico che caratterizza il messaggio. Idealmente, ogni digest corrisponde ad un solo messaggio, nella pratica non è possibile.
Gli algoritmi di message digest inoltre sono **non invertibili**, ovvero non è possibile risalire al messaggio originale partendo dal digest o comunque deve essere molto difficile trovare un altro messaggio che abbia lo stesso digest.

Il digest è utile per garantire l'integrità di un messaggio; supponiamo di avere un digest **MD** di un messaggio **MSG**, per calcolare l'integrità di **MSG** mi basta calcolare il suo digest e assicurarmi che corrisponda ad **MD**.

Un tipico algoritmo di questo tipo è **MD5** che applica una trasformazione combinatoria a blocchi di 512 bit del messaggio e genera un output a 128 bit.

| REQUISITO                    | SI/NO | MOTIVO                                                       |
| ---------------------------- | :---: | ------------------------------------------------------------ |
| Confidenzialità              |  No   | Il digest non implica alcun tipo di confidenzialità del messaggio, anche perché non è possibile risalire ad esso dal digest stesso. |
| Integrità                    |  Si   | La coppia <MSG, MD> permette di stabilire se il messaggio è integro o meno. |
| Autenticazione e non ripudio |  No   | Chiunque può generare il digest di un messaggio.             |

## PRESTAZIONI ALGORITMI

Abbiamo detto che RSA, rispetto a DES e MD5 è di parecchi ordini più lento, dunque solitamente RSA viene usato per crittare piccole quantità di dati.



**CHIAVI DI SESSIONE**
Solitamente, RSA e DES vengono usati insieme, in particolare

- Si cifra con RSA una chiave segreta casuale *(chiave di sessione)*
- Si usa la chiave di sessione come chiave segreta DES

In questo modo si sfrutta l'efficienza di DES e la maggiore flessibilità di RSA per la gestone delle chiavi.



**FIRME DIGITALI**
Abbiamo detto che RSA permette di implementare il non ripudio, cifrando con la chiave privata, tuttavia cifrare grossi messaggi può richiedere molto tempo. Dunque, spesso viene *firmato* solo il **digest** del messaggio, che è molto più piccolo.
In questo modo il destinatario riceve il messaggio in chiaro e il digest crittato con la chiave privata del mittente, in modo da poterlo verificare decrittando il digest con la sua chiave pubblica e confrontandolo con un digest generato da esso, in modo da assicurarsi che il messaggio sia integro.



## DISTRIBUZIONE CHIAVI

In un sistema crittografico a chiave pubblica, si usa la chiave pubblica di un utente per mandare messaggi confidenziali ad esso e per autenticare provenienza e integrità dei messaggi inviati dall'utente, ma bisogna essere certi che la chiave pubblica corrisponda effettivamente all'utente.

Un modo per garantire questo tipo di informazioni è usare un **certificato**, che generalmente è emesso da da un'**autorità di certificazione** *fidata e autorizzata*. Questi certificati sono rilasciati in formato **X.509** e contengono il nome dell'entità, la sua chiave pubblica, il nome dell'autorità di certificazione e la firma digitale dell'autorità, in modo che con la chiave pubblica dell'autorità si possa garantire l'integrità del certificato.

Un'autorità può inoltre certificare delle altre autorità creando quella che si chiama **catena di certificati**, in modo che poi gli enti certificati da quest'ultima possano essere verificati anche dall'autorità top level.
<img src="./img/011.png" alt="011" style="zoom:55%;" align="right" />Questo ci permette poi di creare un'infrastruttura di cui tutti si fidano che si occupa di certificare altre autorità di certificazione, in modo da poter rilasciare certificati più facilmente agli utenti.





## PGP

Un tool che utilizza le tecniche appena descritte è **Pretty Good Privacy** che utilizza RSA ed implementa crittografia e firme digitali.



**FIRME DIGITALI**

<img src="./img/012.png" alt="008" style="zoom:40%;" />



**CIFRATURA**

<img src="./img/013.png" alt="013" style="zoom:40%;" />



**RIASSUMENDO**

<img src="./img/014.png" alt="014" style="zoom:50%;" />



## SICUREZZA NEI SISTEMI OPERATIVI

Un **sistema operativo** è un *layer* intermedio tra le risorse fisiche di una macchina e le sue applicazioni, e che quindi virtualizza tali risorse per renderle disponibili alle applicazioni che verranno poi utilizzate dagli utenti.
Dunque, il sistema operativo si deve occupare di mantenere lo stato della memoria della macchina valido utilizzando tecniche di segmentazione e paginazione, di assicurarsi che tutte le applicazioni abbiano accesso alla CPU, di virtualizzare le connessioni I/O, di organizzare le informazioni sulla memoria di massa, di interpretare i comandi dell'utente.



**AREE DI INTERESSE PER LA SICUREZZA NEI SO**
Quello che può suscitare interesse nei sistemi operativi per quanto concerne la sicurezza è l'**autenticazione degli utenti**, la **protezione della memoria**, il **controllo degli accessi** da parte degli utenti ai file piuttosto che alle periferiche I/O e la relativa condivisione di risorse fra utenti.



## AUTENTICAZIONE

L'obiettivo dell'autenticazione è quello di evitare che soggetti non autorizzati riescano ad accedere alle risorse del sistema. Esistono diversi metodi per implementare l'autenticazione, alcuni sono:

- **DIMOSTRAZIONE DI CONOSCENZA**: richiede all'utente di fornire un PIN, una password o una **challenge/response** che consiste nel chiedere all'utente un problema che può essere risolto solo da chi è autorizzato ad accedere al sistema, quindi la risposta vera è propria può cambiare ogni volta, al contrario delle password.
  In generale, il sistema si assicura che l'utente sia autenticato se possiede la conoscenza richiesta.
- **DIMOSTRAZIONE DI POSSESSO**: richiede all'utente di dimostrare il possesso di chiavi fisiche *(ad esempio su chiavetta usb o anche una OTK inviata al mio smartphone)* come potrebbero essere delle *smartcard*.
- **CARATTERISTICHE PERSONALI (BIOMETRIA)**: si basa sulla biometria dell'utente e quindi usa dati come le impronte digitali, il riconoscimento del viso, della voce.



**USO DI PASSWORD**
L'autenticazione tramite password richiede all'utente di introdurre una password che, in teoria, solo lui conosce e se questa corrisponde a quella che il sistema ha memorizzato, l'utente viene autenticato.
È buona norma che il sistema operativo memorizzi le password in formato *hash* *(e quindi un digest della password, ricordiamo che i digest sono non-invertibili)*. Questo evita che, nel caso qualcuno abbia accesso al file delle password del sistema operativo, non siano rivelate le password in chiaro. Inoltre, questo permette al SO di autenticare semplicemente calcolando il digest della password in input e confrontarlo con quello salvato.



**SICUREZZA PASSWORD**
Non sono nuovi gli attacchi di tipo brute force ai sistemi protetti da password, quindi è buona norma implementare alcune norme che ne aumentino la sicurezza e l'improbabilità di essere indovinata.



In ordine di complessità avremmo

- Password assenti
- Password uguali/derivate da user ID *(è importante tener conto che nel password file del sistema operativo ci sono i nomi utente)*
- Nomi comuni
- Vocabolari con sostituzioni banali *(a = @, a = 4, i = 1)*
- Dizionari completi per una o più lingue
- Password con numeri e lettere minuscole e maiuscole
- Password con set completo di caratteri.

È anche possibile sfruttare informazioni su sistemi di generazione di password per sferrare questo tipo di attacchi, anche se le password generate sono complesse dal punto di vista dei caratteri usati.

È buona norma quindi evitare di usare password troppo semplici che sarebbero facilmente identificabili con dei dizionari di password *(che contengono i più comuni nomi di persone, animali domestici, date e parole di uso frequente)* e di usare un set di caratteri ampio. Inoltre, scrivere o ripetere la password ne mette a repentaglio la sicurezza.



**PASSWORD FILE CRIPTATI**
Abbiamo detto che le password non sono salvate in chiaro, ma viene salvato il loro hash.
Ma se due utenti scelgono la stessa password?
In questi casi viene utilizzato un *salt*, ovvero un numero di 12 bit random che viene aggiunto alla password prima di eseguirne l'hash e viene poi salvato nel file delle password, in modo da concatenarlo alla password in chiaro e poter risalire all'esatto hash in fase di autenticazione.

**Multiple Point of Failure**: il sistema, oltre a mantenere le password crittate, protegge anche il file delle password.



**ONE TIME PASSWORD (CHALLENGE/RESPONSE)**
In questi casi, la password è in realtà una funzione matematica $F(x)$ che l'utente sa calcolare; dunque il sistema propone all'utente un valore a caso $x$ e l'utente calcola $F(x)$.
Un esempio di questo tipo di password potrebbe essere una funzione segreta di numeri o stringhe, oppure un generatore di numeri disponibile all'utente e al sistema *(le OTK delle banche)* o anche una funzione crittografica.



**PROCESSO DI AUTENTICAZIONE**
Un sistema di autenticazione ben progettato

- Non fornisce dettagli su nomi utente e sistema prima che l'autenticazione avvenga *(non dice ad esempio se il nome utente è giusto ma la password no, in caso di password errata rifiuta l'autenticazione e basta, senza dare informazioni sull'user ID)*.
- Può essere *lento* intenzionalmente per scoraggiare attacchi di forza bruta.
- Può disabilitare gli utenti dopo un numero di tentativi errati.
- Può utilizzare un'autenticazione a più fattori.
- È resistente ad attacchi basati *sul tempo di risposta*; ovvero la risposta non deve dipendere da quanto della password è stato indovinato o meno *(immaginano un algoritmo che controlla la password carattere per carattere)*.



## ACCESSI DALLA RETE

<img src="./img/015.png" alt="015" style="zoom:40%;" align="right"/>Attraverso la rete abbiano un problema in più da considerare; le informazioni trasmesse dall'utente e dal sistema possono essere intercettate *(man in the middle attack)* e quindi, inviando per esempio le password in chiaro, rende il nostro sistema vulnerabile.



Un protocollo di autenticazione su rete ben progettato, solitamente fa uso di **crittografia in abbinamento a meccanismi di challenge/response**, in cui il sistema richiede all'utente di crittare un messaggio con la sua chiave segreta *(dunque la funzione challenge in questo caso è una funzione crittografica)*.
Ma questo non basta, poiché il messaggio crittato può essere intercettato, quindi la challenge deve cambiare ogni volta per evitare che un messaggio crittato possa essere riutilizzato *(questo tipo di challenge si chiamano **nonce**)*.

<img src="./img/016.png" alt="016" style="zoom:60%;" />



Spesso l'autenticazione è bidirezionale, ovvero non solo il server richiede all'utente di crittare un nonce, ma anche l'utente richiede al server di codificare un suo nonce.

<img src="./img/017.png" alt="017" style="zoom:60%;" />



<img src="./img/018.png" alt="018" style="zoom:50%;" align="right"/>Questo sistema però crea una falla che permette di sferrare i cosiddetti **reflection attacks**.

Supponiamo che l'attaccante, che sa che l'autenticazione è bidirezionale, abbia intercettato una sessione di comunicazione tra l'utente e il server, in particolare ha intercettato la parte di autenticazione del server.
Supponiamo che l'attaccante blocchi il nonce dell'utente che serve per autenticare il server ed inizi subito una nuova richiesta di autenticazione; a questo punto il server si aspetta sia il nonce dell'utente iniziale, sia il resolve della nuova fase di comunicazione.
A questo punto l'attaccante può usare la sessione dell'utente iniziale per inviare al server il nonce che ha ricevuto nella sua sessione, e farlo quindi crittare al server stesso per poi utilizzare il messaggio crittato per autenticarsi sulla propria sessione.

In rosso è rappresentata la sessione dell'utente, mentre in nero quella dell'attaccante.

<img src="./img/019.png" alt="019" style="zoom:60%;" />



Un altro requisito per un buon algoritmo di autenticazione in rete è dunque che i **messaggi debbano riportare degli indentificativi degli utenti nei messaggi crittati**, proprio per evitare questo tipo di attacchi.
In questo modo il messaggio che l'attaccante poteva riflettere pima non ha più potere poiché l'attaccante si identificherebbe al server come il server, e quindi non avrebbe senso.

Ricapitolando, i tre requisiti per un sistema di autenticazione in rete sono

1. Crittografia in abbinamento a challenge/response
2. Cambiare il challenge ogni volta *(nonce)*
3. Inserire identificativi nei messaggi crittati.



**AUTENTICAZIONE BIDIREZIONALE ASIMMETRICA**
Un protocollo simile è che soddisfa i tre requisiti descritti sopra ma che usa le chiavi pubbliche è il protocollo Needham-Shcroeder.
Nel primo passaggio l'utente critta la propria identità e un nonce per il server con la sua **chiave pubblica**, in modo che solo il server possa leggerlo. Il server risponde con la conferma e il nonce per l'utente, crittandolo con la chiave pubblica dell'utente. Infine l'utente manda la response al nonce del server.

<img src="./img/020.png" alt="020" style="zoom:60%;" />

Tuttavia, questo protocollo è vulnerabile, poiché il secondo e il terzo messaggio non includono identificativi del mittente.
In particolare, questo protocollo è vulnerabile al reflection attack; un attaccante può far credere all'utente di essere il server e ottenere le utente dell'utente per poi identificarsi al server vero.

<img src="./img/021.png" alt="021" style="zoom:60%;" />

Introducendo degli identificativi anche nel secondo e nel terzo messaggio questo bug può essere risolto.

<img src="./img/022.png" alt="022" style="zoom:60%;" />

<img src="./img/023.png" alt="023" style="zoom:60%;" />