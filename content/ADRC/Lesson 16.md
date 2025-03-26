---
author: Alessandro Straziota
draft: true
---

# Swarm Intelligence - k-Majority Protocol

Nella [lezione precedente](./15.html) abbiamo visto che fare un semplice
*campionamento uniforme* non è un metodo molto efficiente per risolvere
il problema del *2-majority consensus*, in quanto per ottenere l\'alta
probabilità è necessario fare un numero di campionamenti inversamente
proporzionale alla grandezza del *bias*.\
Ricordiamo che il *bias* di una 2-colorazione di un grafo $G$ è pari
alla quantità $$
  s = b - \frac{n}{2}
  $$ dove $b$ è il numero di nodi colorati col colore `b` (o 1),
assumendo che esso sia il colore di maggioranza iniziale.\
Più precisamente abbiamo visto che nel caso in cui il *bias* sia molto
grande, $s \in \Theta(n)$, allora un numero piccolo ($O(\log{n})$) di
campionamenti è sufficiente per l\'alta probabilità. Invece nel caso in
cui lo sbilanciamento sia più piccolo, per esempio
$s \in \Theta(\sqrt{n\log{n}})$, in meno di $\Theta(\sqrt{n\log{n}})$
campionamenti non è possibile avere l\'alta probabiltà. Se per esempio
stiamo facendo un campionamento su tutti i 60 milioni di italiani, non è
ragionevole fare
$c\sqrt{60.000.000\log{(60.000.000)}} \approx c \times 33.000$ chiamate
per ottenere una statistica attendibile, soprattutto se $c$ non è molto
piccolo.

Un modo più efficiente per fare una statistica precisa della maggioranza
è quello di sfruttare la *conoscenza* degli altri nodi (**intelligenza
di massa**, o **swarm intelligence**). L\'idea è quella di campionare un
certo numero di nodi *u.a.r.*, e invece di cambiare colore in accordo
alla loro colorazione iniziale, bisogna cambiare colore in base a ciò
che la maggior parte dei nodi campionati [ritengono]{.underline} il
colore di maggioranza.\
In poche parole, ad ogni tempo $t \geq 0$, ogni nodo campiona $k$ nodi
*u.a.r.*, ed aggiorna il suo colore in base al colore di maggioranza che
ha campionato.\
La differenza col `Simple Sampling` sta nel fatto che mentre prima ogni
nodo vaceva $R$ campionamenti sulla colorazione iniziale e poi prendeva
una decisione finale, adesso invece ogni nodo [cambia]{.underline} stato
ad ogni tempo $t$ e quindi il campionamento è fatto in base alla
colorazione del grafo al tempo $t - 1$.\
Campionare i colori al tempo precedente sostanzialmente consiste nello
sfruttare le conoscenze pregresse dei campionamenti fatti dagli altri
nodi. Tale protocollo è noto come (*$k$-Majority Protocol*)

> **Def.** (*$k$-Majority Protocol*) Nel *$k$-Majority Protocol*, in
> breve $k$-MAJ, ad ogni *time slot* ogni nodo [campiona]{.underline}
> *u.a.r.* $k$ tra i suoi vicini (compreso se stesso) e si colora del
> colore di maggioranza che osserva. Il protocollo termina quando tutti
> i nodi sono dello stesso colore.

1-MAJ
-----

È faciel dimostrare che la versione del $k$-MAJ con $k = 1$ è molto poco
efficiente, ovvero convergerà molto lentamente nello stato finale
desiderato. Per dimostrarlo, basta dimostrare che mediamente ad ogni
time slot il *bias* s rimane uguale al *bias* al tempo precedente.\
Sia $s_t = b_t - \frac{n}{2}$ il valore del *bias* al tempo $t$, e sia
la v.a. $s_{t+1}$ che indica il valore del *bias* a tempo $t + 1$. Siano
inoltre le v.a. binarie $X_1, ..., X_n$ tali che $X_i = 1$ se il nodo
$v_i$ campiona un nodo colorato di 1 al tempo $t$ e quindi al tempo
$t + 1$ si colore di 1, oppure vale 0 altrimenti, per ogni $v_i \in V$.
[Sapendo]{.underline} che $b_t = n/2 + s_t$, tali variabili avranno
probabilità $$
   \mathcal{P}(X_i = 1 | s_t) = \frac{b_t}{n} = \frac{\frac{n}{2} + s_t}{n} = \frac{1}{2} + \frac{s_t}{n}
   $$ e media $$
   \mathbb{E}\left[ X_i | s_t \right] = \frac{b_t}{n} = \frac{1}{2} + \frac{s_t}{n}
   $$

Sia quindi $b_{t+1} = X_1 + ... + X_n$ il numero di nodi che hanno
colore 1 al tempo $t + 1$, con media $$
   \mathbb{E}\left[ b_{t+1} | s_t \right] = \sum_{i = 1}^{n} \mathbb{E}\left[ X_i | s_t \right] = \frac{n}{2} + s_t
   $$ Ciò implica che $s_{t+1}$ in madia varrà $$
   \mathbb{E}\left[ s_{t+1} | s_t \right] = \mathbb{E}\left[ b_{t+1} | s_t \right] - \frac{n}{2} = s_t
   $$

3-MAJ
-----

Purtroppo è possibile dimostrare che anche con la varsione 2-MAJ del
protocollo è possibile ottenere alta probabilità in tempi brevi. Pericò
è necessario considerare una versione con un valore di $k$ di avlore
[almeno]{.underline} 3. In questa sezione analizzeremo la varianete
3-MAJ del protocollo.\

> **THM** Se $G \equiv K_n$ è una *clique* di $n$ nodi, con una
> 2-colorazione iniziale sbilanciata di *bias*
> $s \in \Omega(\sqrt{n\log{n}})$, allora il protocollo 3-MAJ convergerà
> al colore di maggioranza dopo $O(\log{n}$ passi, *w.h.p.*

> **Proof** Assumiamo senza perdita di generalità che il colore di
> maggioranza iniziale sia il colore 1, ovvero che $b_0 > a_0$. Inoltre,
> fissato un generico tempo $t \geq 1$, ci riferiremo semplicemente ad
> $a = a_t$ come il numero di nodi colorati di 0, e a $b = b_t$ come il
> numero di nodi colorati di 1 (la maggioranza).\
> Sia la v.a. $X_t$ che indica il numero di nodi colorati di 0 (la
> minoranza) al tempo $t$. Per ogni nodo $v \in V$, indichiamo con
> $Y^i_v$ la v.a. binaria che vale 1 se al tempo $i + 1$ il nodo $v$ si
> colora di 0, oppure vale 0 se si colora di 1.\
> Perciò avremo che la probabilità che la tempo $t + 1$ un nodo $v$ si
> colori del colore di minoranza, sapendo che precedentemente ci sono
> $X_t = a$ nodi colorati di 0, sarà $$
> \mathcal{P}(Y^{t+1}_v = 1 \vert X_t = a) = \left( \frac{a}{n} \right)^3 + \binom{3}{2}\frac{a^2(n-a)}{n} = \frac{a^2}{n^3}(3n - 2a)
> $$ Perciò mediamente il numero di nodi colorati col numero di
> [minoranza]{.underline} 0 al tempo $t+1$ sarà $$
> \mathbb{E}\left[ X_{t+1} \; \vert \; X_t = a \right] = \sum_{v \in V} \mathbb{E}\left[ Y^{t+1}_v = 1 \vert X_t = a \right] = \left(\frac{a}{n}\right)^2(3n - 2a)
> $$
>
> Fatta questa breve prefazione, la dimostrazione si suddividerà in 3
> fasi:
>
> -   **Fase 1** dimostrare che in tempo logaritmico il *bias* $s$
>     cresce da $c \sqrt{n\log{n}}$ fino ad $n/4$, ovvero che il numero
>     di nodi colorati col colore di minoranza descrese da
>     $n/2 - c \sqrt{n\log{n}}$ fino a meno $n/4$ molto velocemente.
> -   **Fase 2** dimostrare che in tempo logaritmico il numero di nodi
>     colorati col colore di minoranza descrese ancora da $n/4$ fino a
>     meno $O(\log{n})$.
> -   **Fase 3** dimostrare che in un solo passo tutti i restanti
>     $\log{n}$ nodi assumeranno il colore di maggioranza 1.
>
> Verrà dimostrato che le tre fasi occorrono [con alta
> probabilità]{.underline}.\
>
> **FASE 1** $n/4 \leq  a_t \leq n/2 - c \sqrt{n\log{n}}$\
> Per definizione sappiamo che $X_t = a$ per un certo valore fissato di
> $a_t = n/2 - s_t$. Sappiamo che per ipotesi del teorema
> $s_t \geq c \sqrt{n\log{n}}$ per qualche costante $c > 0$. Poniamo
> inoltre in un caso non ottimale, assumendo che lo sbilanciamento $s_t$
> non sia troppo grande, ovvero supponiamo che $s_t \leq n/4$.\
> Osserviamo che la funzione $$
> f(a) = a^2(3n - 2a) = n^2 \cdot \mathbb{E}\left[ X_{t+1} \; \vert \; X_t = a \right]
> $$ è sempre crescente per $1 < a < n$.\
> Dato che $a \leq n/2 - s$
>
> ```{=latex}
> \begin{align*}
>   \mathbb{E}\left[ X_{t+1} \; \vert \; X_t = a \right] &= \frac{f(a)}{n^2} = \frac{1}{n^2}f\left(\frac{n}{2} - s\right)\\
>   &= \frac{1}{n^2}\left(\frac{n}{2} - s\right)^2 \left(3n - 2\left(\frac{n}{2} - s\right)\right)\\
>   &= \frac{n}{2} - \frac{3}{2}s + 2\frac{s^3}{n^2} \leq \frac{n}{2} - \frac{5}{4}s \;\;\; (in \; realtà \; \frac{11}{8}s \; ???)
> \end{align*}
> ```
> dove l\'ultima disuguaglianza è data dal fatto che $s \leq n/4$.\
> Osserviamo che $X_{t+1}$ è la somma di $Y^{t+1}_v$ v.a.
> **indipendenti**, perciò possiamo applicare il *Chernoff Bound*. In
> questo caso useremo la **forma additiva** del bound con parametri $$
> \mu = \frac{n}{2} - \frac{5}{4}s, \;\;\; \lambda = \frac{1}{8}s
> $$
>
> otterremo per ogni $a \leq s \leq n/4$ che $$
> \mathcal{P}\left( X_{t+1} \geq \left( \frac{n}{2} - \frac{5}{4}s \right) + \frac{1}{8}s  \right) =\\
> \mathcal{P}\left( X_{t+1} \geq \frac{n}{2} - \frac{9}{8}s \; \vert \; X_t = a \right) \leq e^{-\frac{s^2}{64n}}
> $$ dato che $s \geq c \sqrt{n\log{n}}$, per un $c$ sufficentemente
> grande otteremo che $$
> e^{-\frac{s^2}{64n}} \leq \frac{1}{n^{\Theta(1)}}
> $$ ovvero $X_{t+1} \leq \frac{n}{2} - \frac{9}{8}s_t$ accade **con
> alta probabilità**. Osservare che per definizione
> $X_{t + 1} = \frac{n}{2} - s_{t+1}$, perciò avermo che
> $s_{t+1} \geq \frac{9}{8}s_t$ **con alta probabilità**, ovvero lo
> sbilanciamento cresce in maniera esponenziale da $c\sqrt{n\log{n}}$ a
> $n/4$.\
> Sia ora l\'evento $\mathcal{E}_t$ definito come $$
> \mathcal{E}_t = \; " X_t \leq \max{ \Big\lbrace \frac{n}{4}, \frac{n}{2} - \left(\frac{9}{8}\right)^t \Big\rbrace} " 
> $$ Dai calcoli fatti in precedenza, possiamo affermare che l\'evento
> condizionato $\mathcal{E}_{t+1} | \bigcap_{i=1}^{t} \mathcal{E}_i$
> accade anch\'esso con alta probabilità. Infatti da primasappiamo che
> $$
> \mathcal{P}\left( X_{t+1} \geq \frac{n}{2} - \frac{9}{8} \; \vert \; X_t = a \right) \leq \mathcal{P}\left( X_{t+1} \geq \frac{n}{2} - \frac{9}{8}s \; \vert \; X_t = a \right) \leq \frac{1}{n^\alpha}
> $$ perciò $$
> \mathcal{P}\left(\mathcal{E}_{t+1} \Big\vert \bigcap_{i=1}^{t} \mathcal{E}_i\right) \geq 1 - \frac{1}{n^\alpha}
> $$
>
> Ponendo ora $T = \log_{9/8}{(\frac{n}{4})} \in O(\log{n})$, la
> probabilità che i nodi colorati di 0 vadano sotto $n/4$ entro $T$
> passi sarà
>
> ```{=latex}
> \begin{align*}
>   \mathcal{P}\left(\exists t \in \left[0,T\right] : X_t \leq \frac{n}{4} \right) &\geq \mathcal{P}( \bigcap_{t=1}^{T} \mathcal{E}_t ) \geq \prod_{t=1}^{T} \mathcal{P}( \mathcal{E}_t )\\
>   &= \prod_{t=1}^{T} \mathcal{P}( \mathcal{E}_t | \bigcap_{i=1}^{t-1} \mathcal{E}_i ) \cdot \mathcal{P}(\bigcap_{i=1}^{t-1} \mathcal{E}_i)\\
>   &= \mathcal{P}( \mathcal{E}_T | \bigcap_{i=1}^{T-1} \mathcal{E}_i ) \prod_{t=1}^{T-1} \mathcal{P}( \mathcal{E}_t | \bigcap_{i=1}^{t-1} \mathcal{E}_i ) \cdot \mathcal{P}(\bigcap_{i=1}^{t-1} \mathcal{E}_i)\\
>   &\geq (1 - 1/n^\alpha) \prod_{t=1}^{T-1} \mathcal{P}( \mathcal{E}_t | \bigcap_{i=1}^{t-1} \mathcal{E}_i ) \cdot \mathcal{P}(\bigcap_{i=1}^{t-1} \mathcal{E}_i)\\
>   &\geq (1 - 1/n^\alpha)^T \geq 1 - \frac{2T}{n^\alpha} \geq 1 - 1/n^{\frac{\alpha}{2}}
> \end{align*}
> ```
> **FASE 2** $O(\log{n}) \leq a_t \leq n/4$\
> In questa fase sappiamo che il numero di nodi colorati col colore di
> minoranza è [al più]{.underline} $n/4$, perciò come prima possiamo
> calcolare il suo valore medio come
>
> ```{=latex}
> \begin{align*}
>   \mathbb{E}\left[ X_{t+1} \; \vert \; X_t = a \right] &= \frac{f(a)}{n^2} \leq \frac{1}{n^2}f\left(\frac{n}{4}\right)\\
>   &= \frac{1}{n^2}\left(\frac{n}{4}\right)^2 \left(3n - \frac{n}{2}\right)\\
>   &= \frac{1}{n^2}\left(a \cdot \frac{n}{4}\right) \left(3n - \frac{n}{2}\right)\\
>   &\leq a \left(\frac{n/4}{n^2}\right) 3n \leq \frac{3}{4}a
> \end{align*}
> ```
> Applicando ancora una volta il *Chernoff bound* nello forma
> **moltiplicativa**, con parametri $$
> \mu = \frac{3}{4}a, \;\;\; \delta = \frac{1}{15}
> $$ otterremo che scarsa probabilità il numero medio di nodi colorati
> di 0 sarà maggiore di $\frac{4}{5}a > \frac{3}{4}a$. $$
> \mathcal{P}(X_{t+1} \geq (1 + \delta)\mu \vert X_t = a) = \mathcal{P}(X_{t+1} \geq \frac{4}{5}a \vert X_t = a) \leq e^{-\beta a}
> $$
>
> Finché avremo che $a \in \Omega(\sqrt{n\log{n}})$, avremo che $$
> \mathcal{P}(X_{t+1} \geq \frac{4}{5}a \vert X_t = a) \leq n^{-\beta}\\
> \implies \mathcal{P}(X_{t+1} \leq \frac{4}{5}a \vert X_t = a) \leq 1 - n^{-\beta}\\
> $$ perciò, anche in questo caso, entro $O(\log{n})$ step, il numero di
> nodi colorati di 0 decrescerà esponenzialmente a $O(\log{n})$
> **w.h.p.**
>
> **FASE 3** $a \in O(log{n}) \; to \; a = 0$ In quest\'ultima fase,
> dimostreremo che il numero $a$ di nodi colorati col colore di
> minoranza passa da $O(\log{n})$ a nessuno in un solo step avviene col
> alta probabilità.\
> Ancora una volta, dato che $a \leq c\log{n}$, avremo che
>
> ```{=latex}
> \begin{align*}
>   \mathbb{E}\left[ X_{t+1} \; \vert \; X_t = a \right] &= \frac{f(a)}{n^2} \leq \frac{f(c\log{n})}{n^2}\\
>   &= \left( \frac{c\log{n}}{n} \right)^2 (3n - 2a)\\
>   &\leq \left( \frac{c\log{n}}{n} \right)^2 3n = 3c^2 \frac{\log^2{n}}{n}
> \end{align*}
> ```
> Adesso, applicando la **disuguaglianza di Markov**, si ha che con
> scarsa probabilità \"sopravvive\" almeno un nodo colorato di 0 $$
> \mathcal{P}(X_{t+1} \geq 1 \; \vert \; X_t = a) \leq 3c^2 \frac{\log^2{n}}{n}
> $$ implicando che si passerà da $O(\log{n})$ nodi colorati di 0 a
> nessuno con alta probabilità in un solo step $\square$.

### Video dimostrazione

```{=html}
<div class="yt-video-container">
  <iframe
src="https://www.youtube.com/embed/kVVuMGG4B9Y"
allowfullscreen
>
  </iframe>
</div>
```

------------------------------------------------------------------------
