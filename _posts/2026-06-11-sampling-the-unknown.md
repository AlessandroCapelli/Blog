---
layout: post
title: "Sampling the unknown"
date: 2026-06-12
---

È credenza comune pensare che gli LLM siano sì in grado di risolvere problemi concreti, ma che l'ambito delle nuove invenzioni e delle scoperte resti fuori dalla loro portata: dato che il modello è stato allenato su dati esistenti, non potrà che pescare da quella base di conoscenza, al massimo rimescolandola in modo elegante. Detta così suona quasi ovvia, però descrive male quello che succede davvero. Semplificando, un modello crea una propria rappresentazione della realtà all'interno di uno spazio vettoriale n-dimensionale enorme. Quanto enorme? Prendendo modelli open source, DeepSeek-V3 lavora con vettori da 7168 dimensioni che attraversano 61 layer di transformer, Llama 3.1 405B sale a 16384 dimensioni su 126 layer. Quando deve generare nuovi token, il modello effettua un sampling proprio a partire da tale spazio: il contesto viene mappato in un punto, da quel punto esce una distribuzione di probabilità sul vocabolario e da lì si pesca il token successivo, uno alla volta.

E attenzione che non si tratta nemmeno di un unico spazio. Ogni layer del transformer ha il suo spazio latente, e la rappresentazione dell'input viene trasformata layer dopo layer, partendo da feature superficiali tipo sintassi e ortografia per arrivare a concetti sempre più astratti man mano che si sale. Dentro questi spazi i concetti vivono come direzioni: i lavori di interpretabilità confermano che la stessa logica vale anche negli spazi latenti dei transformer moderni, per migliaia di concetti che il modello può comporre anche se non li ha mai visti combinati nei dati. Ecco perché "pescare dalla base di conoscenza" è la parte fuorviante della credenza: il modello non pesca dai dati, pesca da una funzione continua costruita comprimendo quei dati, e una funzione continua si può campionare anche dove i dati non c'erano.

Immaginiamoci uno spazio vettoriale n-dimensionale che rappresenta la funzione obiettivo, quella che idealmente risolve qualsiasi problema "reale". Chiamiamolo S. Definiamo altri due insiemi: M, ovvero la funzione che il modello rappresenta davvero, quella imparata durante il training con tutta la sua complessità e "indecifrabilità" (con M si intende l'insieme dei punti che il modello può effettivamente raggiungere campionando quella funzione), e U, cioè le "scoperte" che sono già state trovate nella realtà dall'umanità. Su tutti questi insiemi possiamo fare sampling, e ogni inferenza del modello è esattamente un campione pescato da M.

A questo punto la domanda "un LLM può scoprire?" diventa una domanda geometrica: esiste un pezzo di M che sta dentro S ma fuori da U? Vediamo i casi possibili.

## Ipotesi 1: M è un sottoinsieme di S

Lo scenario più pessimista: il modello non coprirà mai tutto lo spazio delle soluzioni. Però M e U sono cresciuti in modi completamente diversi, uno comprimendo una quantità assurda di dati e l'altro seguendo secoli di storia umana, quindi non hanno nessun motivo di coincidere. Alcune aree sono già state trovate nella realtà, alcune no, e altre possono essere campionate nell'intersezione. Resta comunque una zona di M dentro S ma fuori da U, fatta di punti validi che nessuno ha mai trovato, e campionare lì significa scoprire/inventare.

![Ipotesi 1]({{ "/assets/images/ipotesi-1.svg" | relative_url }})

## Ipotesi 2: M copre quasi tutto S

Scenario ottimista e probabilmente irrealistico: U è storicamente minuscolo rispetto a S, quindi praticamente tutto il resto è in attesa di essere campionato.

![Ipotesi 2]({{ "/assets/images/ipotesi-2.svg" | relative_url }})

## Ipotesi 3: M non è contenuto in S

Lo scenario realista: una parte di M sta fuori dalle soluzioni valide (sono le allucinazioni), e allo stesso tempo restano zone di S che il modello non raggiunge. L'intersezione dipende dal costo: serve un verificatore che filtri i campioni, che sia un compilatore, un esperimento o una dimostrazione. I benchmark tendono a saturare proprio nei campi dove verificare costa poco, tipo codice e matematica, dove un test o un proof assistant danno un verdetto in millisecondi. Dove invece il verificatore è un esperimento di laboratorio o una dimostrazione che richiede anni, l'intersezione esiste lo stesso, ma campionarla in modo utile diventa lentissimo.

![Ipotesi 3]({{ "/assets/images/ipotesi-3.svg" | relative_url }})

## Campionare l'ignoto

Mettendo in fila le tre ipotesi salta all'occhio una cosa: in tutti e tre i casi esiste un'area di M che cade dentro S e fuori da U. Geometricamente, la risposta alla domanda di partenza è quindi sì: un LLM può campionare punti validi che nessuno ha ancora trovato. Il "non può inventare" non regge, perché confonde i dati con la funzione che li ha compressi.

E allora perché non vediamo una pioggia di scoperte? Perché il collo di bottiglia si è spostato. Generare un campione da M costa un'inferenza, ma stabilire se quel campione sta davvero in S costa quanto il verificatore che si ha a disposizione. Dove verificare è quasi gratis, il ciclo "campiona, verifica, scarta" mostra risultati impensabili già oggi. Quando invece richiede un esperimento, un trial clinico o anni di revisione, l'intersezione c'è lo stesso, ma raggiungerla in modo utile resta lento e caro.

La domanda interessante quindi non è più "un LLM può scoprire?", ma "quanto costa verificare e con che grado di certezza?". Il modello propone l'ignoto, il verificatore stabilisce quale parte di quell'ignoto è reale.
