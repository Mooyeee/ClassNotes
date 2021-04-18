## CHEATSHEET ANALISI PER RICERCA OPERATIVA

### CONTINUITÀ

Sia *f* : \[a, b\] $\rightarrow$ $\mathbb{R}$ e **x<sub>0</sub>** $\in$ \[a, b\] un punto del suo dominio.

*f* è ***continua*** in **x<sub>0</sub>** se $\lim \limits_{x \rightarrow x_{0}^{+}} f(x)$ = $\lim \limits_{x \rightarrow x_{0}^{-}} f(x)$ = $\lim \limits_{x \rightarrow x_{0}} f(x)$ = $f(x_{0})$

"*Se x si avvicina a x<sub>0</sub>,* *allora* *f*(x) *si avvicina a* *f*(x<sub>0</sub>)."

**PROPRIETÀ**

- La somma algebrica di funzioni continue è una funzione continua.
- Il prodotto di funzioni continue è una funzione continua.
- La composizione di funzioni continue è una funzione continua.

**DISCONTINUITÀ**

- Prima specie - $\lim \limits_{x \rightarrow x_{0}^{-}} f(x) \neq \lim \limits_{x \rightarrow x_{0}^{+}} f(x)$ entrambi finiti.
- Seconda specie - $\lim \limits_{x \rightarrow x_{0}} f(x)$ = $\infty$ o non esiste.
- Terza specie *(eliminabile)* - $\lim \limits_{x \rightarrow x_{0}^{+}} f(x)$ = $\lim \limits_{x \rightarrow x_{0}^{-}} f(x)$ = $\lim \limits_{x \rightarrow x_{0}} f(x)$ $\neq$ $f(x_{0})$



### DERIVABILITÀ

La ***derivata*** di *f* nel punto x<sub>0</sub> è il limite del rapporto incrementale al tendere di *h* a 0, ovvero:

*f* '(x<sub>0</sub>) = $\lim\limits_{h \rightarrow 0} \frac{f(x_{0} + h) - f(x_{0})}{h}$

**PROPRIETÀ**

- $\frac{df(x_{0})}{dx} = 0$ $\implies$ x<sub>0</sub> è un punto stazionario.
- $\frac{df(x_{0})}{dx} = 0$ $\land$ **n pari**  $\land$ $\frac{d^{n}f(x_{0})}{dx^{n}} > 0$ $\implies$ x<sub>0</sub> è un punto di minimo.
- $\frac{df(x_{0})}{dx} = 0$ $\land$ **n pari**  $\land$ $\frac{d^{n}f(x_{0})}{dx^{n}} < 0$  $\implies$ x<sub>0</sub> è un punto di massimo.

Dove $\frac{d^{n}f(x_{0})}{dx^{n}}$ è la ***prima derivata non prima diversa da zero***.



### CONVESSITÀ/CONCAVITÀ	

Sia *f* : $\mathbb{R}$ $\rightarrow$ $\mathbb{R}$ una funzione due volte differenziabile in tuto $\mathbb{R}$.

- Se *f* '' (x) > 0 in un intervallo \[a, b\] $\implies$ *f* (x) è ***convessa*** in quell'intervallo.	$\cup$
- Se *f* '' (x) < 0 in un intervallo \[a, b\] $\implies$ *f* (x) è ***concava*** in quell'intervallo.	$\cap$

**PROPRIETÀ**

- Una funzione *f* (x) è convessa SSE - *f* (x) è concava e viceversa.
- La somma di funzioni concave/convesse è ancora una funzione concava/convessa.
- Se *f* (x) è convessa e x\* è un punto di minimo locale, è anche un minimo globale.
- Se *f* (x) è concava e x\* è un punto di massimo locale, è anche un massimo globale.