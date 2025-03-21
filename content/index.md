---
title: Note Magistrale
author: Alessandro Straziota
---
> [!warning] 🚧 WORK IN PROGRESS 🚧
> Questo sito è ancora in costruzione, e perciò le diverse pagine riportano parecchi bug nel rendering (soprattutto delle formule).


## [Autore di questo sito 🌱](https://alessandrostr95.github.io/alessandro-straziota/)

# Introduzione
Queste note nascono come appunti personali, scritti subito dopo le lezioni durante la mia magistrale. Non erano pensate per essere condivise, ma semplicemente per aiutarmi a fissare i concetti nel momento in cui li avevo appena sentiti. Di conseguenza, non hanno la chiarezza e la struttura di un testo pensato per un pubblico: ci sono errori, parti incomplete e scelte organizzative che riflettono più il mio modo di apprendere che una logica di facile consultazione.

Con il tempo, però, ho scoperto che molti studenti trovavano utili questi appunti, nonostante le loro imperfezioni. Per questo ho deciso di raccoglierli e pubblicarli, mantenendo però il loro carattere originale[^1].

Va detto che le note non hanno una struttura uniforme, perché nel corso della magistrale ho cambiato sia il metodo con cui le prendevo sia gli strumenti che utilizzavo.
Infatti potrete notare due stili completamente differenti:

- Nel 1° anno, ho usato [**Emacs**](https://www.gnu.org/software/emacs/) come editor di testo, ed [**org-mode**](https://orgmode.org/) come formato. Per ogni lezione creavo **un unico file**, che poteva contenere anche più argomenti, oppure un argomento poteva risultare spezzato su più file se trattato in lezioni differenti. Successivamente, con uno script in **Elisp**, generavo una versione HTML delle note e le pubblicavo (come molti di voi già sanno);
- Nel 2° anno invece, sono passato a [**Obsidian**](https://obsidian.md/) e ho adottato un approccio più strutturato: ogni argomento, grande o piccolo che fosse, aveva un suo file dedicato, con collegamenti ipertestuali ad altri concetti correlati.


Di conseguenza, le note del primo anno hanno una struttura più **"procedurale"**, mentre quelle del secondo sono più organizzate e navigabili. Tuttavia, entrambe riflettono lo stesso principio:

> prendere appunti in modo veloce e funzionale al mio studio personale.

Chiunque voglia usare queste note, deve quindi tenere a mente queste differenze e il loro carattere informale. Non sono un riferimento definitivo, ma spero possano essere un punto di partenza utile per chi studia gli stessi argomenti.

## Costruzione del sito

Per **unificare** le note in un unico vault Obsidian, ho dovuto convertire tutti i file `.org` in `.md` utilizzando [**Pandoc**](https://pandoc.org/).
Tuttavia, la conversione non è stata perfetta: circa il **30%** dei contenuti ha richiesto correzioni manuali, principalmente a causa delle differenze tra org-mode e Obsidian nella gestione degli **iperlink**, delle **formule matematiche in LaTeX**, delle tabelle, delle immagini, e di altri dettagli di formattazione.

In realtà la situazione è **molto peggio** di quello che sembra. Se aprite queste note come un vault Obsidian, esse verranno renderizzate per bene (escludendo gli errori di formattazione dovuti ai file generati a partire da quelli `.org`).
Se invece stai visualizzando queste note tramite browser, praticamente quasi tutte verranno renderizzate male, e saranno quasi illeggibili.
Questo perché per generare questo sito ho utilizzato [Quartz](https://quartz.jzhao.xyz/): un ulteriore strumento che converte il mio vault obsidian nel sito che stai visualizzando.

In poche parole, molte parti di queste note ancora presentano problemi di compatibilità e richiedono aggiustamenti.
Per esempio queste note [[1 - Erdos-Renyi Random Graph]], [[2 - Power Law]] e [[4 - Grafi geometrici aleatori]] sono state fixate, e sono perfettamente leggibili.
Molte altre invece, come [[Adaboost]] o [[Probabilistic Ranking Principle]] sono ben lontane dall'essere utilizzabili.

Sistemare tutte le note, mi richiederà un ==grosso effort==, e svariate ore di lavoro.
Dato che:
1. nessuno mi paga per fare questo lavoro;
2. non ho nessun guadagno nel condividere le mie note con molti studenti che nemmeno conosco;
3. ho già finito la magistrale anni fa;
4. ho molte cose più interessanti da fare;
5. a quanto pare queste note sono state parecchio utili ad altri studenti in passato, e magari potranno essere utili anche per te;

richiedo un aiuto/incentivo economico per portare avanti questo progetto e offrire a tutti delle note ben leggibili ed usufruibili.
Insomma, tutto questo lavoro, almeno una birra me la potete offrire.

> [!important] Offerta libera
> [Link PayPal](https://paypal.me/AlessandroStraziota?country.x=IT&locale.x=it_IT)


A tal punto, sarei anche grato se voi mi notificasse eventuali errori, aprendo delle issue sulla [repository github](https://github.com/Alessandrostr95/sito_note_magistrale/issues).

-----

# Indice

## Primo anno
-  [[AR/0 -Analisi di reti|Analisi di reti]]
-  [[ADRC|Algoritmi distribuiti e Reti Complesse]] 🚧 ==coming soon== 🚧
	- [[GT|Game Theory]] 🚧 ==coming soon== 🚧

## Secondo anno
- [[0 - Inferenza Statistica e Teoria dell'Informazione|Inferenza Statistica e Teoria dell'Informazione]]
- [[0 - Information Retrieval|Information Retrieval]]
- [[0 - Machine Learning|Machine Learning]]
- [[0 - Natural Language Processing|Natural Language Processing]]


-------

# Changelog

Fixed pages:
- [[1 - Erdos-Renyi Random Graph]]
- [[2 - Power Law]] 
- [[4 - Grafi geometrici aleatori]]
- [[6 - Small World]]
- [[7 - Communities - Part 1]]
- [[8 - Communities - Part 2]]
- [[9 - Processi di diffusione - Part 1]]



[^1]: in realtà non mi va di rivedermi tutte le note prese in magistrale e di sistemarle tutte quante in maniera seria.
