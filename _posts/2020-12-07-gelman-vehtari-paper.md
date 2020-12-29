---
title: "What are the most important statistical ideas of the past 50 years?"
author: Jacopo Diquigiovanni
date: "2020-12-07"
tags: ["General discussion"]
toc: false
---


L'articolo [*What are the most important statistical ideas of the past 50 years?*](#bibliografia) esamina ed approfondisce quelle che sono, secondo i due autori Andrew Gelman ed Aki Vehtari, le 
otto idee più influenti in ambito statistico che hanno caratterizzato gli ultimi 50 anni. Queste idee sono: l'inferenza causale controfattuale, il bootstrapping e l'inferenza basata su dati simulati, i 
modelli sovraparametrizzati e le procedure di regolarizzazione, i modelli multilivello, gli algoritmi di calcolo, l'analisi decisionale adattiva, l'inferenza robusta, l'analisi 
esplorativa dei dati. 

Un primo aspetto in comune di queste otto idee è che non rappresentano una soluzione ad un problema specifico esistente, quanto rappresentano nuovi modi di intendere la statistica e 
l'analisi dei dati. Per esempio, i metodi di regolarizzazione rappresentano un nuovo modo di affrontare il noto problema di scegliere la complessità del modello (e dunque la sua dimensione) 
sulla base dell'effettiva capacità di stimarne i parametri, oppure l'inferenza robusta ha formalizzato la necessità di avere conclusioni inferenziali stabili anche quando le assunzioni 
alla base del modello sono violate. 

Un secondo aspetto che questi otto metodi hanno in comune è il fatto che facilitano l'utilizzo di un sempre maggior numero di dati e/o dati sempre più complessi. Per esempio, l'analisi 
esplorativa dei dati permette di visualizzare dati sempre più complessi, oppure le procedure di regolarizzazione permettono di includere nei modelli un sempre maggior numero di variabili 
esplicative.

Un ulteriore, interessante aspetto analizzato dagli autori è l'utilizzo che queste otto idee fanno dei moderni strumenti di calcolo. In questo caso, ci sono delle differenze tra le otto 
idee: alcune di queste necessitano indubbiamente di una capacità di calcolo sufficiente per essere utilizzate (si pensi ad esempio alle reti neurali), altre hanno avuto dei vantaggi enormi 
dallo sviluppo del calcolo moderno sebbene teoricamente fattibili anche senza una capacità di calcolo elevata (si pensi ad esempio al bootstrapping o l'analisi dei dati esplorativa), altre 
infine rappresentano sviluppi teorici che non necessitano per forza di risorse computazionali (si pensi all'inferenza causale e l'inferenza robusta).

L'articolo infine si arricchisce con alcune considerazioni che esulano dal tema principale dell'articolo ma che forniscono importanti spunti di riflessione. Qui di seguito ne trovate tre 
esempi, che riporto con le parole usate dagli autori stessi.

- *The most important aspect of a statistical analysis is not what you do with the data but what data you use.*

- *It would be tempting to say that a common feature of all these methods is catchy names and good marketing.*

- *Researcher degrees of freedom can enable researchers to routinely obtain statistical significance even from data that are pure noise.*

E voi, cosa ne pensate? Concordate con la lista delle otto idee più influenti? Ne avreste aggiunta qualcuna? Concordate con le tre affermazioni finali?

## Bibliografia

- [Gelman and Vehtari (2020)](https://arxiv.org/abs/2012.00174) What are the most important statistical ideas of the past 50 years? *arXiv preprint arXiv:2012.00174*
