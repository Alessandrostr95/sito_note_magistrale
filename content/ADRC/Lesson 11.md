---
author: Alessandro Straziota
draft: true
---

# Randomized Distributed Protocols

Un **algoritmo probabilistico** $\mathcal{A}$ è un algoritmo che accessi
a una fonte di *S* di random bits, e che prende decisioni in accordo ai
bit casuali dati dalla fonte *S*. Sia un qualsiasi input $x$ di
$\mathcal{A}$ di dimensione $\vert x \vert = n$, e sia $r = r(x)$ il
numero totale di random bit che $\mathcal{A}$ richiede ad *S* durante
l\'esecuzione $\mathcal{A}(x)$. Avremo quindi che la correttezza del
risultate dell\'esecuzione $\mathcal{A}(x)$ e il suo tempo di
completamento, sono *variabili aleatorie* sopra lo spazio di probabilità
$$
  \Omega = (\lbrace 0,1 \rbrace^r, \mathcal{P}(\cdot))
  $$ dove $\mathcal{P}(\cdot)$ è la *distribuzione di probabilità*
indotta da $\mathcal{A}(x)$.\
Osserviamo che se $\mathcal{A}$ sceglie gli $r$ bits **uniformemente a
caso** e in maniera **indipendente**, allora avremo che la distribuzione
$\mathcal{P}(\cdot)$ è espicitamente definita come $$
  \mathcal{P}(\mathbf{y}) = 2^{-r} \;\; \forall \mathbf{y} \in \lbrace 0,1 \rbrace^r
  $$

Infine, per semplicità assumiamo che $\mathcal{A}$ è un algoritmo per il
**problema decisionale** $\Pi$.\
Diamo ora una definizione di concetto di **probabilità d\'errore** di
$\mathcal{A}$ in termini di eventi nello spazio di probabilità $\Sigma$,
e quindi una migliore definizione di *algoritmo probabilistico*.\

> **Def.** un algoritmo $\mathcal{A}$ per un un problema $\Pi$ ha una
> **probabilità d\'errore** $\varepsilon \in \left[ 0, 1 \right]$, se
> per ogni possibile input $x \in \lbrace 0,1 \rbrace^n$ avremo che $$
> \mathcal{P}_{\Omega}(\mathcal{A}(x) = \Pi(x)) \geq 1 - \varepsilon
> $$ Diremo inoltre che $\mathcal{A}$ risolve $\Pi$ con **alta
> probabilità** (**with high probability**, in breve **w.h.p**), se per
> valori di $n$ *sufficientemente grandi*, e per ogni input
> $x \in \lbrace 0,1 \rbrace^n$ avremo che $$
> \mathcal{P}_{\Omega}(\mathcal{A}(x) = \Pi(x)) \geq 1 - n^{-\beta}
> $$ per qualche costante $\beta > 0$.

A questo punto, possiamo definire in maniera del tutto analoga il
concetto di **protocollo distribuito probabilistico**. Sia quindi un
dato protocollo $\mathcal{A}$, che viene eseguito su un grafo
$G = (V,E)$ con $\vert V \vert = n$ nodi, e che inizia con una
**configurazione iniziale** $x$. Ogni nodo $v$ ha accesso a una fonte
**provata** e **indipendente** $s(v)$ di random bits. Le decisioni di
ogni nodo $v$ in accordo in quanto descritto dal protocollo
$\mathcal{A}$, dipendono *anche* dalle sequenze di random bits generate
dalle fonti di casualità $s(v)$.\
Supponiamo che il [massimo]{.underline} numero di random bits generati
dalle fonti durante l\'esecuzione di $\mathcal{A}(G,x)$[^1] sia una
certa quantità $r > 0$. Perciò il risultato dell\'esecuzione
$\mathcal{A}(G,x)$ (o il suo tempo di completamento) è una variabile
aleatoria completamente determinata dallo spazio di probabilità $$
  \Omega = (\lbrace 0,1 \rbrace^{n \times r}, \mathcal{P}(\cdot))
  $$ dove $\mathcal{P}(\cdot)$ è la *distribuzione di probabilità*
indotta da $\mathcal{A}(G,x)$.\
Analogamente possiamo estendere i concetti di **probabilità di errore**
e **w.h.p** anche per i protocolli distribuiti probabilistici.\

Leader Election in a Unlabeled Ring
===================================

Nella lezione della [leader election](./08.html) abbiamo visto che non è
possibile risolvere *deterministicamente* tale problema senza
l\'assunzione che tutti i nodi abbiano un **Unique ID**.\
Assumiamo ora un contensto in cui abbiamo una rete $G=(V,E)$ di $n$ nodi
**anonimi** (senza un ID unico) in cui però tutti i nodi conoscono $n$ e
sanno di essere in un [anello]{.underline}, e supponiamo inoltre che
ogni nodo $v$ abba accesso ad una fonte [privata]{.underline} e
[indipendente]{.underline} di casualità $s(v)$. È possibile definire un
protocollo probabilistico che risolve questo problema sotto le
precedenti assunzioni (e senza l\'assunzione di `Unique ID`).\
L\'idea è abbastanza semlice: ogni nodo sceglie uniformemente a caso e
in maniera indipendente un numero nell\'insieme $\left[ m \right]$, il
quale diventerà l\'ID del nodo. A questo punto basta eseguire uno dei
protocolli deterministici conosciuti per la leader election su anello
con l\'assunzione di `Unique ID`.\
Certamente potrebbe capitare che gli identificatori dei nodi non siano
univoci, però il valore $m$ è scelto in modo tale che *w.h.p.* (*con
alta probabilità*) non ci saranno due nodi $x,y$ con stesso id,
$id(x) \neq id(y)$. Rifacendoci alla definizione di protocollo
probabilistico, in questo caso il risultato dell\'esecuzione del
protocollo è una *variabile aleatoria*. Infatti noi vogliamo che lo
stato finale sia uno stato in cui [un solo nodo]{.underline} sia nello
stato di `leader`, e tutti gli altri nello stato `follower`. Con questo
protocollo probabilistico non abbiamo tale certezza di arrivare ad una
configurazione finale accettabile, però sappiamo che arriveremo a tale
configurazione desiderata con alta probabilità.\

``` {.python}
self.id = random_choice(range(m))

self.run("stage_tecnique")
```

Analisi
-------

Riferiamoci al protocollo appena descritto con $\texttt{RL}$ (*Random
Leader*) e alla sua esecuzione $\texttt{RL}(G, n, m)$, dove $n$ è il
numero di nodi dell\'anello $G$ ed $m$ è il parametro di scelta delle
etichette che vedremo alla fine di questa analisi come scegliere in modo
tale che con alta probabilità il protocollo porti ad una configurazione
accettabile.\
Osserviamo che un **evento sufficiente** (ma **non necessario**)
affinché il protocollo termini [correttamente]{.underline} è il seguente
$$
   \mathcal{B} = \mbox{"non esistono due nodi differenti } v,w \in V \mbox{, con } v \neq w \mbox{ tali che } id(v) = id(w) \mbox{"}
   $$ L\'evento $\mathcal{B}$ non è una condizione necessaria in quanto
potrebbero esserci due nodi $v,w$ con stessa etichetta, ma se esiste un
altro nodo $u$ con etichetta [maggiore]{.underline}
$id(u) > id(v) = id(w)$, allora tutti i protocolli deterministici per la
leader election già visti in questi appunti porteranno ad una
configurazione finale accettabile. Viceversa, se occorre l\'evento
$\mathcal{B}$, ovvero se tutti i nodi hanno etichette differenti, allora
certamente il protocollo porterà ad una configurazione finale
accettabile.\
Osserviamo ora che l\'azione di scegliere un\'etichetta uniformemente a
caso in $\left[ m \right]$, equivale all\'azione di pescare una pallina
in un\'urna di $m$ palline enumerate da 1 ad $m$. Per calcolare la
probabilità dell\'evento desiderato $\mathcal{B}$ conviene calcolare la
probabilità dell\'evento *complementare* $$
   \overline{\mathcal{B}} = \mbox{"esistono almeno due nodi differenti } v,w \in V \mbox{, con } v \neq w \mbox{ tali che } id(v) = id(w) \mbox{"}
   $$

Possiamo calcolare la probabilità di questo evento come l\'
[unione]{.underline} degli eventi **disgiunti** $$
   \overline{\mathcal{B}}_i = \mbox{"esistono almeno due nodi differenti con etichetta} i \mbox{"}
   $$

Tale evento a sua volta può essere modellato con l\'
[unione]{.underline} di v.a. **binomiali**

```{=latex}
\begin{align*}
     \mathcal{P}(\overline{\mathcal{B}}_i)
     &= \mathcal{P}(\bigcup_{k = 2}^{n} \lbrace \mbox{esistono esattamente } k \mbox{ nodi con etichetta } i  \rbrace)\\
     \textbf{(1)}  &= \sum_{k = 2}^{n} \mathcal{P}( \lbrace \mbox{esistono esattamente } k \mbox{ nodi con etichetta } i \rbrace)\\
     &= \sum_{k = 2}^{n} \binom{n}{k}\left( \frac{1}{m} \right)^k \left( 1 - \frac{1}{m} \right)^{m-k}\\
     &\leq \sum_{k = 2}^{n} \binom{n}{k}\left( \frac{1}{m} \right)^k\\
     \textbf{(2)} &\leq \sum_{k = 2}^{n} \left( \frac{en}{k} \cdot \frac{1}{m} \right)^k\\
     &= \frac{e^2n^2}{4m^2} + \sum_{k = 3}^{n} \left( \frac{en}{k} \cdot \frac{1}{m} \right)^k\\
     &\leq O\left(\frac{n^2}{m^2}\right) + \sum_{k = 3}^{n} \left( \frac{en}{k} \cdot \frac{1}{m} \right)^k\\
     &= O\left(\frac{n^2}{m^2}\right) + \sum_{k = 3}^{n} O\left( \frac{n^k}{m^k} \right)\\
     (\mbox{perché } m \geq n) &\leq O\left(\frac{n^2}{m^2}\right) + O\left( \frac{n^3}{m^3} \right)\\
     &\leq O\left(\frac{n^2}{m^2} +  \frac{n^4}{m^3} \right)
\end{align*}
```
dove **(1)** perché gli eventi dell\'unione sono tutti **disgiunti**, e
**(2)** per l\'[approssimazione di
Stirling](https://it.wikipedia.org/wiki/Approssimazione_di_Stirling).\
Dopodiché, possiamo calcolare la probabilità di $\overline{\mathcal{B}}$
come l\'unione di tutte le probabilità $\overline{\mathcal{B}}_i$ per
ogni $i \in \left[ m \right]$.

```{=latex}
\begin{align*}
     \mathcal{P}(\overline{\mathcal{B}})
     &= \mathcal{P}(\bigcup_{i = 1}^{m} \overline{\mathcal{B}}_i)\\
     &\leq \sum_{i = 1}^{m} \mathcal{P}(\overline{\mathcal{B}}_i)\\
     &\leq m \cdot O\left(\frac{n^2}{m^2} + \frac{n^4}{m^3} \right) = O\left(\frac{n^2}{m} + \frac{n^4}{m^2} \right)
\end{align*}
```
Ponendo ora $m = n^3$ avremo che tale probabilità sarà al più $$
   \mathcal{P}(\overline{\mathcal{B}}) \leq O\left(\frac{1}{n} + \frac{1}{n^2} \right) \in O\left( \frac{1}{n} \right)
   $$

Possiamo quindi concludere che per $m \geq n^3$ l\'esecuzione del
protocollo $\texttt{RL}(G, n, m)$ porterà la rete in una configurazione
finale accettabile **con alta probabilità** $$
   \mathcal{P}(\mathcal{B}) \geq 1 - O\left( \frac{1}{n} \right)
   $$

------------------------------------------------------------------------

[^1]: ovvero l\'esecuzione del protocollo $\mathcal{A}$ sul grafo $G$
    partendo dalla configurazione $x$.
