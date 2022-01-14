### CHEATSHEET

1. $D_j^0[i] = 1 \implies D_j^1[i] = 1$
   Se il prefisso $P[1,i]$ del pattern è suffisso di $T[1,j]$ con $0$ errori, a maggior ragione lo sarà con $1$ errore.
   
   Ancora più in generale $D_j^h[i] = 1\implies D^{h'}_j[i] = 1$         per $h' > h$
   
2. $D_j^0[i] = 1 \implies D_j^1[i+1] = 1$
   Se il prefisso $P[1,i]$ del pattern è suffisso di $T[1,j]$ con $0$ errori, sicuramente $P[1, i+1]$ sarà suffisso di $T[1,j]$ con un errore *(ho aggiunto un carattere)*

   In generale $D^h_j[i] = 1 \implies D^{h+1}_j[i+1] = 1$
   Lo stesso discorso vale anche per il bit precedente, ovvero $D^h_j[i] = 1 \implies D^{h-1}_j[i+1] = 1$

3. $|B(P)| = i \iff D_j[m] = 1$ e $i$ è la più grande posizione $<m$ tale che $D_j[i] = 1$
   Se $D_j[m] = 1$ il pattern ha un'occorrenza nel testo, quindi $D_j[i] = 1$ implica che $P[1,i] = suff(P)$

4. Data $C(\sigma)$ il numero di suffissi che iniziano con un simbolo $\sigma'$ è dato da
   $C(\sigma' +1) - C(\sigma')$

   Il loro intervallo va da $C[\sigma'] +1$ a $C[\sigma+1]$
   Perché $C[\sigma]$ fornisce anche la posizione massima dell'ultimo suffisso che inizia col simbolo immediatamente inferiore.

5. Sommando i valori dell'ultima riga di $Occ$ otteniamo $C$

6. Per trovare $BWT$ da $SA$ basta osservare che $B[i] = T[S[i] - 1]$
   Poiché per definizione di $BWT$ $B[i]$ è il simbolo che precede il suffisso $T[S[i], |T|]$

7. Per vedere in che posizione lessicografica è un suffisso che inizia con un simbolo $\sigma$ basta guardare il valore di $F[\sigma]$ che contiene il primo simbolo dei suffissi in ordine lessicografico.

