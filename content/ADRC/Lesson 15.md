---
author: Alessandro Straziota
---

# Majority Consensus

Sia un grafo $G = (\left[ n \right], E)$ ed una sua **colorazione iniziale**
$$
\mathbb{x} : \left[ n \right] \rightarrow C \equiv \lbrace 0, 1 \rbrace
$$
Si vuole progettare un protocollo efficiente per il problema del `Majority Consensus`.
Partendo dall'assunzione che la configurazione iniziale abbia un certo sbilanciamento $s$ (*bias*), l'obiettivo è quello di convergere ad una configurazione monocromatica dove ogni nodo è colorato col colore più popolare inizialmente.

Più precisamente indichiamo con
- $x_v^t$ il colore del nodo $v$ al tempo $t$.
- $x^t(0) = \vert \lbrace v \in V | x_v^t = 0 \rbrace \vert = a_t$, ovvero il numero di nodi che al tempo $t$ sono nel colore $0$.
- $x^t(1) = \vert \lbrace v \in V | x_v^t = 1 \rbrace \vert = b_t$, ovvero il numero di nodi che al tempo $t$ sono nel colore $1$.
- $C_t \in \lbrace 0,1 \rbrace^n$ la colorazione di $G$ al tempo $t$.
- assumendo che all'inizio la maggioranza ha colore $1$ ($x^0(1) > x^0(0)$) definiamo il *bias* (*sbilanciamento*) iniziale la quantità
	$$
	 x^0(1) = \frac{n}{2} + s_0 \implies s_0 = x^0(1) - \frac{n}{2}
	 $$

D'ora in poi faremo riferimento semplicemente ad $s$ come al *bias* che dipende dal corrente contesto $C_t$.

Definiamo ora formalmente il problema del *$\ell$-Majority-Consenus Problem*

> [!tldr] Def. (*$\ell$-Majority-Consenus Problem*)
> Dato un sistema distribuito $G = (\left[ n \right], E)$, l'*$\ell$-Majority-Consenus Problem* è definito tramite le seguenti proprietà.
> Iniziando da una colorazione iniziale $C_0$ con *bias* $s > \ell$, il sistema deve convergere ad uno stato stabile in cui tutti i nodi hanno adottato il colore di maggioranza iniziale, ovvero arrivare a un tempo $\tau$ tale che
> $$
> C_\tau = \lbrace \max{(a_0, b_0)} \rbrace^n
> $$

^a22dd3

## Simple Sampling
Consideriamo il problema del $\ell$-[[#^a22dd3|Majority-Consenus]] su un **grafo completo** $G = K_n$, e senza perdita di generalità assumiamo che il colore di maggioranza iniziale sia 1, perciò avremo che $b_0 > a_0$.

Un protocollo abbastanza semplice è il **Simple Sampling**, nel quale ogni nodo campiona *u.a.r.* $R > 1$ vicini (compreso se stesso) e decide che colore assumere in base alla maggioranza dei nodi che ha campionato.

Per prima cosa consideriamo un caso in cui il *bias* sia **molto grande**, del tipo $s \geq \frac{n}{4}$.
In questo caso avremo che $b_0 \geq \frac{3}{4}n$.

Indichiamo con $R$ il numero di sampling che fa un nodo $v$, e con $R_a,R_b$ il numero di volte che campiona un nodo colorato di $0$ e $1$ rispettivamente.

Diremo che l'evento *"$v$ ha successo"* occorre se alla fine degli $R$ campionamenti $v$ deciderà di passare al colore di maggioranza.
Osservare che ciò accade se almeno la metà delle volte $v$ campiona il colore di maggioranza $1$, ovvero se $R_b \geq R/2$.

Sia la v.a. binaria $Y_i^v$ che vale 1 se $v$ campiona un nodo con colore $1$ al *campionamento* $i$-esimo, e $0$ se ne campiona uno col colore di minoranza $0$.
Tale v.a. avrà probabilità
$$
\mathcal{P}(Y_i^v = 1) = \frac{b_0}{n}
$$
e media
$$
\mathbb{E}\left[ Y_i^v \right] = \frac{b_0}{n}
$$
per ogni $i = 1, 2, ..., R$.

Per linerità, la media del numero di volte che $v$ campiona un nodo con colore $1$ (ovvero la media di $R_b$) sarà
$$
\mathbb{E}\left[ Y^v \right] = \mathbb{E}\left[ R_b \right] = \sum_{i=1}^{R}\mathbb{E}\left[ Y_i^v \right] = R\frac{b_0}{n} \geq \frac{3}{4}R
$$

Si vuole calcolare in funzione di $R$ con quale probabilità $v$ ha successo, ovvero qual è la probabilità che $\mathcal{P}(R_b \geq \frac{R}{2}) = \mathcal{P}(R_a < \frac{R}{2})$.

Ovviamente, dato che vogliamo che il sampling dia un risultato correttamente con **alta probabilità**, si vuole trovare un valore di $R$ per il quale
$$
\mathcal{P}\left( R_b \geq \frac{R}{2} \right) \geq 1 - \frac{1}{n^{\Omega(1)}}
$$
Per fare ciò, iniziamo col calcolare la probabilità dell'evento inverso.
$$
\begin{align*}
     \mathcal{P}\left( R_b < \frac{R}{2} \right)
     &= \mathcal{P}\left(R_b < \left(\frac{3}{4} - \frac{1}{4}\right) R \right)\\
     &= \mathcal{P}\left(R_b < \left(1 - \frac{1}{3}\right) \frac{3}{4}R \right)\\
     &= \mathcal{P}\left(R_b < \left(1 - \delta \right) \mathbb{E}\left[ R_b \right] \right)\\
     (\text{by Chernoff bound})&\leq e^{-\beta R}
\end{align*}
$$

Ponendo $R = c\log{n}$, per un certo $c > 0$ tale che $c \beta > 1$, avremo ottenuto l'alta probabilità
$$
\mathcal{P}\left( R_b \geq \frac{c \log{n}}{2} \right) \geq 1 - \frac{1}{n}
$$

Infine per ottenere che questo risultato rimanga con alta probabilità per **tutti** gli $n$ nodi, è necessario scegliere un $c$ tale che $c\beta > 2$ e applicare lo **Union Bound**, infatti
$$
\begin{align*}
     \mathcal{P}(\text{``insuccesso"}) &= \mathcal{P}(\exists v \text{ che ha insuccesso})\\
     &= \mathcal{P}\left( \bigcup_{v \in V} Y^v < \frac{c\log{n}}{2} \right)\\
     &\leq \sum_{v \in V} \mathcal{P}\left( Y^v < \frac{c\log{n}}{2} \right)\\
     &= n \cdot \mathcal{P}\left( Y^v < \frac{c\log{n}}{2} \right)\\
     &\leq \frac{1}{n}
\end{align*}
$$

Perciò, se il *bias* è molto grande $s \in \Theta(n)$, con un numero **logaritmico** di campionamenti è possibile fare una buona stima con alta probabilità.

Purtroppo però non è vero il contrario.

Se per esempio consideriamo un *bias* decisamente più piccolo, per esempio $s \in \Theta(\sqrt{n\log{n}}) \in o(n)$, non basta un numero logaritmico di *samples*[^1] per avere l'alta probabilità.
In questo caso è possibile dimostrare che si necessitano $\Theta(\sqrt{n\log{n}})$ *sample*, il quale numero è **esponenzialmente** più grande di $\Theta(\log{n})$.

[^1]: campionamenti.
