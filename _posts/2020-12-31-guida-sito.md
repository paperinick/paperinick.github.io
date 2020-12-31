---
author: Jacopo Schiavon
title: "Il blog: dietro le quinte"
date: "2020-12-31"
subtitle: "Come funziona il sito e il sistema dei commenti."
tags: ["Guide"]
toc: true
toc_sticky: true
weight: 2
---

In questo post spiego un po' come funziona il blog e come mantenerlo, principalmente per avere una reference futura.

## Le basi
Il blog in sè è hostato attraverso il servizio [GitHub Pages](https://pages.github.com/), un sistema gratuito e legato agli account github.

Il sito è scritto sfruttando [Jekyll](https://jekyllrb.com/), un sistema basato su Ruby e integrato con GitHub Pages per generare siti statici partendo da semplice markdown. Jekyll permette di utilizzare svariati temi per personalizzare il proprio sito, e quello utilizzato per il momento è [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/), altamente personalizzabile e configurabile.

Infine, la gestione dei commenti è fatta attraverso [Staticman](https://staticman.net/), un sistema per gestire i commenti (dinamici per natura) come se fossero statici attraverso dei push sulla repository che mantiene il sito. Dal momento che non è possibile utilizzare la api prodotta direttamente da Staticman, per il blog di Paperinick ne è stata creata una apposita, mantenuta su una istanza [Heroku](https://heroku.com) gratuita (dovrebbe bastare per l'attività prevista per il sito).

Nel seguito una spiegazione più dettagliata delle varie parti.

## Struttura e funzionamento del sito
Il sito, situato al link [paperinick.github.io](https://paperinick.github.io), viene generato a partire dalla repository [paperinick.github.io](https://github.com/paperinick/paperinick.github.io) su GitHub. La struttura della repository è la seguente:
```bash
paperinick.github.io
├── _data                       # data files for customizing the theme
|  ├── comments                 # folder with comments, divided in subfolder for each post
|  ├── navigation.yml           # main navigation links
|  └── authors.yml              # authors data
├── _post                       # all the various posts 
|  ├── YYYY-MM-DD-post-title.md # posts naming convention
|  └── ...
├── assets                      
|  └── images                   # images folders
|  |  ├── post-title            # the folder name should be the same 
|  |  |  └── ...                # as the post name
|  |  └── ...
├── _pages                      # service pages
|  ├── 404.html                 
|  ├── about.md
|  └── ...
├── _layouts                    # theme customizations
|  └── ...
├── _includes                   # theme customizations
|  └── ...
├── _config.yml                 # configuration page
├── staticman.yml               # configuration for comment provider
├── Gemfile                     # dependencies for Ruby
├── index.html                  # home page
└── ...
```
La principale cosa da tenere sotto controllo (a parte ovviamente i singoli post inseriti con le rispettive immagini) è il file con i dati degli autori (che andrebbe aggiornato man mano dagli autori stessi, ma va tenuto d'occhio). Per il resto, vanno tenuti d'occhio i commenti attraverso la repository GitHub (vedi la [sezione apposita](#commenti)).

### Account GitHub
La repository è mantenuta dall'account Paperinick su GitHub. Sebbene sia stato creato a questo scopo, in linea di principio Paperinick è un account Github completo, anche se soltanto con le funzionalità di tier free, ed è collegato ad una mail generata appositamente `phds.stat.unipd@gmail.com`.

Per avere accesso alla mail o all'account GitHub, basta chiedere a [Jacopo Schiavon](mailto:jacopo.schiavon.1@phd.unipd.it).





## Commenti
La gestione dei commenti è effettuata attraverso Staticman, hostato su una istanza nel free-tier di Heroku.

### Bot GitHub
L'idea di Staticman è di usare un account apposito su GitHub che abbia permesso di fare Pull Requests sulla repository del blog. Nello specifico, l'account è `staticman-paperinick`, che si appoggia su una apposita mail `staticman-paperinick@gmail.com`. Anche in questo caso, chiedete l'accesso a [Jacopo Schiavon](mailto:jacopo.schiavon.1@phd.unipd.it).

### Heroku
Heroku è un servizio che permette di hostare webapp gratuitamente (fintanto che rimangono sotto una certa soglia di risorse utilizzate). Nel nostro caso è stato configurato per mantenere Staticman e quindi fargli gestire i commenti. Da quanto si trova in giro, per rimanere nel tier gratuito basta rimanere al di sotto dei 1000 commenti al mese, limite che sembra ampiamente alla portata di Paperinick. 

Al momento la [web app](https://staticman-paperinick.herokuapp.com/) è mantenuta nell'account di Jacopo Schiavon, ma l'ownership può essere trasferita facilmente.

### ReCaptcha
Per ottenere un po' di protezione contro eventuali spambot, è stato incluso anche un sistema di controllo utilizzando il servizio reCaptcha di google. Anche questo è accessibile attraverso l'account google `phds.stat.unipd@gmail.com`.

### Come funziona il tutto
L'idea alla base di Staticman è di creare un bot che riceva il commento ed elabori una pull request per la repository del sito. Quando la pull request dovesse essere accettata da uno dei gestori del sito, il commento diventerà parte del sito statico, venendo quindi rigenerato da GitHub in pochi secondi.

Nel nostro caso, il bot ha due componenti: la prima è un account GitHub (`staticman-paperinick`) che permette di agire sulla repository effettiva, la seconda è la webapp su Heroku, che attraverso il token fornito da GitHub può agire come se fosse l'account di supporto e contiene tutti gli script per leggere il commento, preparare i file per la pull request ed effettuarla.



