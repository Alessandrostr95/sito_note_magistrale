---
author: Alessandro Straziota
draft: true
---


Distributed Graph Coloring
==========================

In questa sezione vedremo dei metodi per il calcolo distribuito di una
*$\alpha$-colorazione* di una rete $G$. In maniera formale,
un\'*$\alpha$-colorazione* di un grafo è una funzione $$
  \phi : V \rightarrow \lbrace 1, 2, ..., \rbrace
  $$ tale che $$
  \forall (u,v) \in E \;\; \phi(v) \neq \phi(u)
  $$

Più precisamente tratteremo grafi *$\Delta$-regolari*, ovvero grafi con
grado massimo al più $\Delta$. Osservare (e convincersi del fatto) che
ogni grafo $\Delta$-regolare accetta una $(\Delta + 1)$-colorazione.

Message-Passing $\mathcal{LOCAL}$ Model
---------------------------------------

Nel modello *Message-Passing $\mathcal{LOCAL}$ Model* consideriamo una
rete distribuita di $n$ nodi, tutti identificati da un **Unique Id**, e
che si possono scambiare dei messaggi (di dimensioni illimitate) in
maniera del tutto **sincrona**. Inizialmente ogni nodo $v$ possiede come
sola conoscenza personale il suo identificatore $id(v)$. Il nodo $v$ è
abilitato a inviare e ricevere messaggi da tutti i suoi vicini,
procedendo per *time slots* **discreti** e **sincroni**. Inoltre ogni
nodo può anche eseguire una serie potenzialmente illimitata di
computazioni in ogni *time slots*. Infine tutti i nodi iniziano il
protocollo ad uno stesso istante $t_0 = 0$.\

Basic Color Reduction Tecnique
------------------------------

Restringiamoci nell\'ipotesi in cui abbiamo un grafo $\Delta$-regolare,
in cui esiste già una *$\alpha$-colorazione ammissibile*
$\phi : \left[ n \right] \rightarrow \left[ \alpha \right]$, con
$\alpha > \Delta + 1$. La **Basic Color Reduction Tecnique** consente di
ridurrre la colorazione da $\alpha$ colori a $\Delta + 1$ in tempo
$\alpha - (\Delta + 1)$. All\'inizio di ogni step, ogni nodo $v$ manda
il proprio colore a tutto il suo vicinato $N(v)$. Al primo passo,
consideriamo il nodo $v$ con colore massimo $\alpha$. Sicuramente il nel
suo vicinato $N(v)$ ci possono essere [al più]{.underline} $\Delta$ nodi
di colore diverso (dato che $G$ $\Delta$-regolare). Ovvero $$
   \vert \phi\left(N(v)\right) \vert \leq \Delta
   $$ Perciò $v$ può scegliere *u.a.r.* un nuovo colore $$
   \beta \in_U \left[\Delta + 1\right] \setminus \lbrace \phi\left(N(v)\right) \rbrace
   $$ garantendo comunque che la nuova colorazione sia ancora una
*colorazione valida*.

Osservare che la nuova colorazione non solo è consistente, ma ha un
colore in meno rispetto alla precedente. Iterando questo processo per i
colori $\alpha - 2, \alpha - 3, ..., \Delta + 2$ otteremo una
$(\Delta + 1)$-colorazione legale.

``` {.python}
Delta = max([v.degree for v in G.nodes])
alpha = max([v.color for v in G.nodes])

for v in G.nodes:
send({
    "msg": v.color,
    "dst": v.neighborhood
})

for i in range(alpha - Delta - 1):
# get the node colored by (alpha-i)
v = G.get_node(color=alpha - i)

# if exists
if v != None:
    v.color = random_choice(set(range(Delta + 1)) - set([u.color for v in v.neighborhood]))
    send({
    "msg": v.color,
    "dst": v.neighborhood
    })
```

Da questa tecnica è anche possibile ricavare un *Independent Set*
[Massimale]{.underline}[^1]. Iniziamo con un insieme $U$ vuoto. Ad ogni
passo $i$, tutti i nodi $v$ di colore $\phi(v) = i$ controllano [in
parallelo]{.underline} se hanno vicini in $U$. Se non ne hanno allora
possono inserirsi in $U$ e comunicarlo al loro vicinato. Banalmente tale
procedura ritorna un *Independent Set* in quanto per costruzione un nodo
si inserisce in $U$ solo se non ha nodi al suo interno. Inoltre è
massimale in quanto entro la fine tutti i nodi avranno avuto
l\'opportunità di entrare. Perciò se un nodo $u$ non si inserisce in
$U$, vuol dire che $U \cup \lbrace u \rbrace$ non è un independent set.

### Analisi

Per costruzione l\'algorimo impiega $\alpha - (\Delta + 1)$ iterazioni,
ognuna delle quali eseguibile in tempo costante. Nel caso peggiore però,
l\'$\alpha$-colorazione iniziale potrebbe essere una biezione in
$\left[ n \right]$. Perciò running time dell\'algoritmo è $$
    \Theta(n - \Delta) \in \Theta(n)
    $$

Parallelizzazione di Khun-Wattenhofer
-------------------------------------

Nella precedente sezione abbiamo visto come trasformare
un\'$\alpha$-colorazione ammissibile di un grafo $\Delta$- regolare, in
una $\Delta + 1$-colorazione in $\alpha - (\Delta + 1)$ step. Il metodo
di *Khun-Wattenhofer* consente di farlo in tempo
$O\left( \Delta \log{\left( \frac{\alpha}{\Delta} \right)}\right)$.\
La procedura inizia dividendo $V$ in
$k = \lceil \frac{\alpha}{\Delta + 1} \rceil$ sottoinsiemi disgiunti
$V_1, V_2, ..., V_k$. Per ogni $i \in \left[ k \right]$, il sottoinsieme
$V_i$ ha i nodi di colore $$
   (i-1)(\Delta + 1) \leq \phi(v) \leq i(\Delta + 1)
   $$

Consideriamo il sottografo *indotto* dagli insiemi $V_1 \cup V_2$, esso
avrà certamente una $(2(\Delta + 1))$-colorazione valida. Perciò
possiamo applicare la *Basic Color Reduction Tecnique* sul grafo indotto
$G(V_1 \cup V_2)$ per ottenere una $(\Delta + 1)$-coloraione $\phi_{12}$
in tempo $(Delta + 1)$. Possiamo fare la stessa cosa anche per il
sottografo $G(V_3 \cup V_4)$, accurandoci solamente di far scegliere i
colori nell\'intervallo $\left[ 3(\Delta + 1), 4(\Delta + 1) \right]$,
così da garantire che alla fine il grafo intero $G$ abbia una
colorazione accettabile. Possiamo fare la stessa cosa anche per i
sottografi $V_5 \cup V_6, ..., V_{k-1} \cup V_k$ (se $k$ è dispari,
all\'ora basterà fare $V_{k-2} \cup V_{k-1} \cup V_k$). Dato che stiamo
in una rete distribuita, possiamo fare tutte queste perazioni **in
parallelo**. Così facendo avremo ottenuto le $(\Delta + 1)$-colorazioni
$\phi_{12}, \phi_{34}, \phi_{56}, ..., \phi_{k-1,k}$.\
È facile convincersi che dopo una iterazione di questa procedura
parallela, il grafo intero $G$ avrà
un\'$\left(\frac{\alpha}{2}\right)$-colorazione
$\phi_{12} \cup \phi_{34} \cup \phi_{56} \cup ... \cup \phi_{k-1,k}$.
Così facendo, in tempo $O(\Delta)$ abbiamo *dimezzato* i colori della
colorazione iniziale.\
Ripetendo, ogni $O(\Delta)$ si dimezza il numero sottoinsiemi, e dopo
$\log_2{\left( \frac{\alpha}{\Delta + 1} \right)}$ volte rimarrà
solamente una $(\Delta + 1)$-colorazione del grafo totale $G$.\
Perciò, sempre nel caso peggiore in cui $\alpha$ è una biezione in $n$,
il running time di questa procedurà sarà $$
   O\left( \Delta \log{\left( \frac{\alpha}{\Delta} \right)}\right) \in O\left( \Delta \log{\left( \frac{n}{\Delta} \right)}\right)
   $$

------------------------------------------------------------------------

[^1]: massimale indica un [massimo locale]{.underline}, e non globale.
