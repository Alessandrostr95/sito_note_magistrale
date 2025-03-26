Pollution game
==============

Traccia
-------

There are $n$ countries. Each country faces the choice of either passing
legislation to control pollution or not. Assume that pollution control
has a cost of 3 for the country, but each country that pollutes adds 1
to all countries (in term of added health costs). The cost of
controlling pollution is 3.\

-   Can we bound the PoA?
-   And the PoS?

Soluzione
---------

Supponiamo di acere $n_1$ nazioni irresponsabili ed egoiste (che non
approvano la legge sul controllo dell\'inquinamento) ed $n_2$
responsabili (che l\'approvano). Certamente $n_1 + n_2 = n$. Certamente
le nazioni irresponsabili porteranno un costo complessivo pari ad $n_1$
per ogni nazione, per un totale globale di $n \cdot n_1$. Invece le
nazioni responsabili porteranno un costo totale di $3n_2$. Perciò il
costo sociale globale in questa situazione sarà pari a
$n \cdot n_1 + 3n_2$.\
Sicuramente ad una nazione irresponsabile **non conviene** cambiare
strategia, in quanto approvare la legge comporterebbe un aumento di
costo pari a 2 (ovvero passa dal pagare 1 al pagare 3). Tale scelta però
converrebbe in maniera *globale*, in quanto abbasserebbe di 1 il costo a
tutti i restanti $n-1$ player. Perciò in seguito all\'approvazione della
legge, il costo sociale diventa pari a $n(n_1 - 1) + 3(n_2 + 1)$.
Vediamo ora per quali valori di $n$ conviene socialmente

```{=latex}
\begin{align*}
     n(n_1 - 1) + 3(n_2 + 1) &< n \cdot n_1 + 3n_2\\
     n \cdot n_1 - n + 3n_2 + 3 &< n \cdot n_1 + 3n_2\\
     -n + 3 &< 0\\
     n &> 3
\end{align*}
```
Perciò, supponendo che ci sono **più** di 3 città in gioco,
[socialemnte]{.underline} conviene che un player sia responsabile.\
Viceversa, a un player responsabile conviene sempre adottare la politica
dell\'inquinamento, rimuovendo la legge. In tal caso avrebbe un
risparmio pari a 2, inffatti passerebbe dal pagare 3 al pagare solamente
1. Purtroppo però questo comporta un aumento dal punto di vista sociale,
in quanto farebbe pagare 1 in più a tutti i restanti $n-1$ player.
Perciò il costo sociale diventerebbe pari a $n(n_1 + 1) + 3(n_2 - 1)$.
Anche in questo caso ciò comporta un aumento del costo sociale se
$n > 3$. Infatti

```{=latex}
\begin{align*}
     n(n_1 + 1) + 3(n_2 - 1) &> n \cdot n_1 + 3n_2\\
     n \cdot n_1 + n + 3n_2 - 3 &> n \cdot n_1 + 3n_2\\
     n - 3 &> 0\\
     n &> 3
\end{align*}
```
\
Perciò, sempre assumendo che ci sono **più** di 3 città, possiamo dire
che la configurazione [socialmente]{.underline} migliore è quando tutti
sono responsabili, con un costo sociale pari a $3n$, mentre
l\'[unica]{.underline} configurazione d\'equilibrio è quando ognuno è
irresponsabile, con un costo totale pari a $n^2$.\
In conclusione, per $n > 3$, il prezzo dell\'anarchia `PoA` e quello
della stabilità `PoS` saranno pari a $n/3$.

------------------------------------------------------------------------

Tragedy of commons
==================

Traccia
-------

There are $n$ players. Each player wants to send information along a
shared channel of known maximum capacity 1. Player $i$'s strategy is to
send $x_i$ units of flow along the channel, for some
$x_i \in \left[ 0,1 \right]$. Each player would like to have a large
fraction of the bandwidth but the quality of the channel deteriorates as
the total assigned bandwidth increases. More precisely, the value of a
player $i$ is $x_i \cdot (1 - \sum_j x_j)$.\

-   Can we bound the PoA?
-   And the PoS?

Soluzione
---------

Iniziamo col cercare di individuare una soluzione
[socialmente]{.underline} ottima. Data una configurazione di strategie
$S = (x_1, x_2, ..., x_n)$, il suo costo sarà della forma

```{=latex}
\begin{align*}
     COST(S)
     &= \sum_i \left( x_i \cdot (1 - \sum_j x_j) \right)\\
     &= \left( \sum_i x_i \right) \left( 1 - \sum_i x_i \right) = s_n \cdot (1 - s_n)
\end{align*}
```
dove $s_n$ è la somma di tutti gli $x_i$.\
Osserviamo che tale funzioni ha due punti di minimo, quando $s_n = 0$ e
quando $s_n = 1$.

![Funzione di costo sociale al crescere di
$s_n$.](../images/adrc-ex-img1.png){style="max-width:400px; width:100%"}

Ovviamente non ha senso $s_n = 0$, in quanto significherebbe che nessun
player usufruisse del canale condiviso, e quindi avrebbero un guadagno
personale nullo. Perciò l\'unica situazione [sensata]{.underline}
socialmente migliore e quando $s_n = \sum_i x_i = 1$, per esempio quando
tutti utilizzano un $n$-esimo di banda ($x_i = 1/n$).\
Cerchiamo di individuare una situazione di equilibrio, per esempio
vedendo se esiste una strategia dominante per un generico player $i$.
Fissiamo un generico $S_{-i} = (x_1, ..., x_{i-1}, x_{i+1}, ..., x_n)$ e
vediamo se esiste una strategia $x_i$ che **indipendentemente** da
$S_{-i}$ è ottima per il palyer $i$. Indichiamo con $X_{-i}$ la somma di
tutte le strategie eccetto quella di $i$, ovvero $$
   s_n = \sum_{x_j \in S} x_j = ( \sum_{x_j \in S_{-i}} x_j ) + x_i = X_{-i} + x_i
   $$ Perciò supponendo che la strategia di $i$ sia $x_i$, il suo
guadagno sarà $$
   COST_i(S) = x_i \cdot (1 - \sum_{x_j \in S} x_j) = x_i \cdot (1 - X_{-i} - x_i)
   $$ è questo valore è ottimale per $x_i = 1 - X_{-i}$.\
Perciò la strategia ottima di un player [dipende]{.underline} dalle
strategie degli altri, quindi non possiamo definire una strategia
domiante. Possiamo però definrie una strategia better response per un
player $i$. Ovvero dato un $S_{-i}$, la strategie migliore per il player
$i$ sarà $$
   x_i = 1 - \sum_{x_j \in S_{-i}}x_j
   $$

Una configurazione $S$ sarà quindi di equilibrio quando per ogni $i$ è
vero che $x_i = 1 - \sum_{i \neq j} x_j$.

```{=latex}
\begin{cases}
     x_1 &= 1 - (x_2 + x_3 + ... x_n)\\
     x_2 &= 1 - (x_1 + x_3 + ... x_n)\\
     &\vdots\\
     x_i &= 1 - (x_1 + x_2 + ... + x_{i-1} + x_{i+1} + ... x_n)\\
     &\vdots\\
     x_n &= 1 - (x_1 + x_2 + ... x_{n-1})\\
\end{cases}
```
$$
     \implies 1 = \sum_i x_i
   $$

**\[DA FINRIE...\]**

------------------------------------------------------------------------

Max-Cut Game
============

Gli ingredienti di questo gioco sono:

-   un grafo $G = (V,E)$ [non diretto]{.underline}.
-   ad ogni nodo è associato un player egoistico.
-   la strategia di un generico player $u$ è
    $S_u \in \lbrace \texttt{red}, \texttt{green} \rbrace$.
-   la ricompensa di un player $u$ rispetto alla configurazione di
    strategie $S$ giocata da tutti i player è $$
     p_u(S) = \vert \lbrace (u,v) \in E : S_u \neq S_v \rbrace \vert
     $$ ovvero il numero di suoi vicini che hanno scelto un colore
    diverso da quello scelto da $u$.
-   l\'obiettivo di ogni player è quello di **massimizzare** la propria
    ricompens.
-   la funzione di costo sociale è pari a $$
     COST(S) = \sum_u p_u(S)
     $$ ovvero [due volte]{.underline} la dimensione del taglio tra i
    nodi colorati di verde e quelli colorati di rosso.

Traccia
-------

Mostrare che:

1.  Max-cut game è un **gioco potenziale**.
2.  $\texttt{PoS} = 1$.
3.  $\texttt{PoA} \geq 1/2$.
4.  there is an instance of the game having a NE with social welfare of
    1/2 the social optimum.

Soluzione
---------

Quesito 1

:   Una funzione potenziale per questo gioco è la seguente $$
     \Phi(S) = \frac{ \vert (V_R, V_G) \vert }{2} = \frac{ COST(S) }{2}
     $$ dove con $(V_R, V_G)$ indichiamo il taglio tra i nodi colorati
    di *rosso* (ovvero quelli che hanno scelto come strategie
    $\texttt{red}$) e i nodi colorati di *verde* (ovvero quelli che
    hanno scelto come strategie $\texttt{green}$).\
    Dimostriamo ora perché questa funzione è [potenziale]{.underline}.
    Sia $S$ una configurazione di strategie nella quale il player $u$ ha
    scelto di colorarsi di *verde*, ovvero che si trova nel versanete
    $V_G$ del taglio. Consideriamo la configurazione
    $S' = (S_{-u}, \texttt{red})$ nella quale $u$ cambia versante e si
    colora di *rosso*, e indichiamo con $(V'_R, V'_G)$ il nuovo taglio.
    Calcoliamo la differenza della funzione potenziale $\Phi$ applicata
    ad $S$ e ad $S'$.

    ```{=latex}
    \begin{align*}
       \Phi(S) - \Phi(S')
       &= \frac{1}{2} (\vert (V_R, V_G) \vert - \vert (V'_R, V'_G) \vert)\\
       &= \frac{1}{2} (COST(S) - COST(S'))\\
       &= \vert \lbrace (u,v) \in E : v \in V_R \rbrace \vert - \vert \lbrace (u,v) \in E : v \in V'_G \rbrace \vert\\
       &= COST_u(S) - COST_u(S')
    \end{align*}
    ```
    perciò $\Phi$ è potenziale.

Quesito 2

:   Sia $M \subseteq E$ un [taglio massimo]{.underline} dell\'istanza
    $G$. Si può dimostrare che ad ogni max-cut corrisponde un equilibrio
    di nash per il Max-cut game. Poniamo in $M=(V_G, V_R)$, ovvero tutti
    i nodi nel versante $V_G$ adottano la strategia $\texttt{green}$,
    mentre tutti quille nel versante $V_R$ adottano la strategia
    $\texttt{red}$. Per dimostrare che $M$ è anche un `NE` basta
    dimostrare che a un player in $V_G$ non conviene cambiare versante
    (simmetrico il viceversa).\
    Indichiamo quindi un qualsiasi $u \in V_G$. Certamente, dato che $M$
    è un taglio massimo, avremo che i vicini di $u$ in $V_G$ sono di
    meno che quelli nel versante opposto $V_R$, altrimenti non sarebbe
    massimo. Ovvero $$
     \lbrace (u,v) \in E : v \in V_R \rbrace \vert \geq \lbrace (u,v) \in E : v \in V_G \rbrace \vert
     $$ Indichiamo con $M'$ il nuovo taglio in cui $u$ cambia versante e
    va in $V_R$ (cambiando colore). Il nuovo guadagno di $u$ diventa $$
     COST_u(M') = \lbrace (u,v) \in E : v \in V_G \rbrace \vert \leq \lbrace (u,v) \in E : v \in V_R \rbrace \vert = COST_u(M)
     $$ perciò $M$ è un equilibrio di Nash.

Quesito 3

:   
