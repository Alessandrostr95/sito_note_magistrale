---
author: Alessandro Straziota
draft: true
---


Wake-up problem
===============

Un altro problema ben noto negli ambienti distribuiti è il cosidetto
**Wake-up problem**, o in italiano il problema della sveglia.
Informalmente in questo problema abbiamo un insieme di nodi iniziali che
si \"*svegliano*\", e che devono avvartire (tramite scambio di messaggi)
l\'intera rete in modo tale da *attivare* (o *svegliare*) tutti gli
altri nodi. Se si pensa bene, il *Wake-up problem* è una
**generalizzazione** del problema del *broadcasting*, in cui non c\'è un
solo nodo `initiator`, bensì un sottoinsieme di nodi. Più precisamente
il problema del *broadcasting* è un **caso particolare** del problema
della sveglia in cui il sottoinsieme di nodi svegli iniziali è composto
da un solo nodo.\
Una definizione formale del problema è la seguente:

-   **Configurazioni iniziali:** un sottoinsieme $W \subseteq V$ di
    `initiator` nello stato `AWAKE`, e l\'insieme *complementare*
    $\overline{W} \equiv V \setminus W$ di nodi nello stato `ASLEEP`.
-   **Configurazione finale:** il *task* risulta completato nel momento
    in cui tutti i nodi sono nello stato `AWAKE`, ovvero
    $W \equiv V \land \overline{W} \equiv \emptyset$.
-   **Restrizioni:**
    -   **Bidirectional links** se esiste l\'arco $(x,y)$ esiste anche
        l\'arco $(y,x)$, e inoltre i nodi sanno *distinguere* i vicini
        in maniera locale, ovvero $\lambda_x(x,y) = \lambda_x(y,x)$.
    -   **Connectivity** la rete è *connessa*.
    -   **Total reliability** non ci sono mai errori durante la
        trasmissione dei messaggi.

`WFLOOD` protocol
-----------------

Come detto in precedenza, il problema della sveglia è una
*generalizzazione* del problema del *broadcast*, perciò applicando le
dovute modifiche al protocollo [flood](./03.html#org9159fdc) possiamo
definire il protocollo `WFLOOD` per il problema

``` {.python}
if self.state == "AWAKE":
if self.is_initiator:
    send("wake-up!") to self.neighbors()
else:
    recieving(msg):
    None

if self.state == "ASLEEP":
recieving(msg):
    send("wake-up!") to self.neighbors() - msg.sender
    self.state = "AWAKE"
```

```{=html}
<p align="center"><span class="figure-number">Code 1:</span> Pseudocodice python-like del protocollo WFlood</p>
```
Come di consueto, analizziamo la *correttezza* e la *complessità* del
protocollo.

### Correttezza

Si vuole dimostrare che esiste un tempo $t \in \mathcal{R}^+$ tale che
$state_t(x) = \texttt{AWAKE}$ per ogni $x \in V$, ovvero che al tempo
$t$ avremo che $W \equiv V$. Consideriamo solo il caso in $W$ è un
sottoinsieme *proprio*[^1] di $V$, perché se tutti i nodi sono
`initiator` allora il tempo $t$ esiste ed è pari a 0.\
Quindi sia $W \subset V$, definiamo la distanza di un nodo dall\'insieme
$W$ come $$
    d(x,W) = \min{\lbrace d(x,y) : y \in W \rbrace}
    $$ Osserviamo che se $x \in W$ allora $d(x,W) = d(x,x) = 0$. Per
definizione del protocollo sappiamo che a un certo punto tutti i nodi in
$W$ che sono nello stato `AWAKE` avranno inviato il messaggio a tutti i
loro vicini, e data l\'assuzione di *total reliability* sappiamo che
esiste un tempo $t_1$ entro il quale tutti i nodi a distanza 1 da $W$
saranno informati (e di conseguenza *svegliati*). Per comodità definiamo
l\'insieme $$
    L_1 = \lbrace x \in V | d(x,W) = 1 \rbrace
    $$ Per *ipotesi induttiva* supponiamo che per ogni $d>1$ sia vero
che esiste un tempo $t_d$ tale che tutti i nodi in $L_d$ siano nello
stato `AWAKE`.\
Si vuole dimostrare che questo è vero anche per il tempo $d+1$.
Consideriamo un nodo generico $x \in L_{d+1}$: certamente deve esistere
almeno un arco $(y,x)$ tale che $y \in L_d$. Se così non fosse, allora
ogni cammino minimo da $W$ arriverebbe in $x$ tramite un arco
\$(z,x)\$[^2] tale che $z \in L_k$ con $k \neq d$, e ciò implica che $x$
è a distanza $k+1 \neq d+1$, ovvero $x \not\in L_{d+1}$.\
Dato che $y$ è stato informato entro il tempo $t_d$, e data la
definizione del processo, $y$ invierà il messaggio di `"wake-up"` ad
$x$. Infine, grazie alla *total reliability*, siamo certi che tale
messaggio arriverà a destinazione, svegliano $x$. Quindi esiste un tempo
$t_{d+1}$ tale che ogni $x \in L_{d+1}$ verrà svegliato entro il tempo
$t_{d+1}$ $\square$.\

### Message Complexity

Ovviamente il caso peggiore è quando $W \equiv V$. Dato che ogni nodo
**non sa** lo stato degli altri, avremo che tutti i nodi manderanno il
messagio di `"wake-up"` a **tutti** i vicini, ovvero a tutti i propri
archi incidenti, perciò la message complexity in questo caso sarà
$2|E| = 2m$. Viceversa, il caso mgliore è quando $|W| = 1$, dove il
protocollo si riduce all\'esecuzione del protocollo di flooding a
singola sorgente, con message complexity di $2m - n + 1$ $$
    MSG(\texttt{FLOOD}) = 2m -n +1 \leq MSG(\texttt{WFLOOD}) \leq 2m 
    $$

Andando a fare un\'analisi più fine, consideriamo il caso genereico in
cui $|W| = k$. Sicuramente $k$ nodi invieranno i messaggi a **tutti** i
loro vicini, mentre $n-k$ invieranno i messaggi a tutti i vicini eccetto
al nodo dal quale sono stati svegliati.

```{=latex}
\begin{align*}
      MSG(\texttt{WFLOOD})
      &= \sum_{w \in W} \delta(w) + \sum_{v \in V \setminus W} (\delta(v) - 1)\\
      &= \sum_{w \in W} \delta(w) + \sum_{v \in V \setminus W} \delta(v) - \sum_{v \in V \setminus W} 1\\
      &= \sum_{v \in V} \delta(v) - \sum_{v \in V \setminus W} 1\\
      &= 2m - (n-k) = 2m - n + k
\end{align*}
```
dove $\delta(v)$ è il grado (uscente) del nodo $v$.

### Esercizio - WFLOOD on trees

Dimostrare che la message complexity del protocollo `WFLOOD` per il
problema della sveglia su un *albero* è pari a $n + k - 2$, dove
$k = |W|$.\
Rifacendoci alla formula calcolata nella precedente sezione, basta porre
$m = n-1$, ottenendo $$
      MSG(\texttt{WFLOOD}/\texttt{Tree}) = 2m - n + k = 2(n-1) - n + k = n + k - 2
    $$

Un Lower-bound al *wake-up problem*
-----------------------------------

**THM** se stiamo sotto le ulteriori assunzioni che $G$ sia una
**clique** $K_n$, e che ogni nodo è **univocamente identificabile**
dagli altri nodi da un identificatore globale che chiunque conosce,
allora una delimitazione inferiore alla message complexity al problema
della sveglia è $\Omega(n\log{n})$.

------------------------------------------------------------------------

[^1]: $W \subset V$

[^2]: per *ipotesi di connettività* $x$ è sempre raggiungibile dai nodi
    di $W$
