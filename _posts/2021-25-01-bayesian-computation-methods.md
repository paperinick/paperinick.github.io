---
title: "Computing Bayes: Bayesian Computation from 1763 to the 21st Century"
author: Emanuele Degani
date: "2021-01-25"
tags: ["Bayesian statistics", "MCMC", "Computational statistics"]
toc: true
toc_sticky: true
---

## Struttura del paper

> [...] The aim is to help new researchers in particular to make sense of the plethora of computational techniques that are now on offer; understand when and why different methods are useful; and see the links that do exist, between them all.

 1. The Beginning
 2. **Bayesian Computation in a Nutshell**
 3. Some Early Chronological Signposts
 4. The Late 20th Century: Gibbs Sampling and the MCMC Revolution
 5. **The 21st Century: A Second Computational Revolution**
 6. The Role of Computation in Model Choice and Prediction
 7. **The Future**

## Section 2: Bayesian Computation in a Nutshell

### Problemi computazionali

Gran parte delle quantità d'interesse in statistica Bayesiana si possono esprimere come

$$
\mathbb{E}(g(\boldsymbol{\theta})\vert \mathbf{y}) = \int_\Theta g(\boldsymbol{\theta}) p(\boldsymbol{\theta}\vert\mathbf{y})  d\boldsymbol{\theta}
$$

dove, a seconda della scelta di $$g(\boldsymbol{\theta})$$:

*   Momenti della *posterior*, e.g. $$g(\boldsymbol{\theta}) = (\boldsymbol{\theta} - \mathbb{E}(\boldsymbol{\theta}\vert \mathbf{y})) (\boldsymbol{\theta} - \mathbb{E}(\boldsymbol{\theta}\vert \mathbf{y}))^T$$ per $$\text{Var}(\boldsymbol{\theta}\vert \mathbf{y})$$
*   *Predictive distribution*, e.g. $$g(\boldsymbol{\theta}) = p(y^* \vert \boldsymbol{\theta}, \textbf{y})$$ per $$p(y^* \vert \mathbf{y})$$
*   *Bayesian decision theory quantities*, con *loss function* $$g(\boldsymbol{\theta}) = L(\boldsymbol{\theta}, d)$$ per una certa regola decisionale $$d$$
*   *marginal likelihood*, con $$g(\boldsymbol{\theta}) = p(\mathbf{y}\vert\boldsymbol{\theta})$$ per $$p(\mathbf{y})$$

Tecniche computazionali per svolgere l'inferenza sono necessarie perchè queste quantità sono quasi sempre **intrattabili analiticamente**.

### Classificazione delle tecniche computazionali Bayesiane

*   *Metodi di integrazione deterministici*: si scelgono $$\boldsymbol{\theta}_1, \dots, \boldsymbol{\theta}_L$$ su $$\Theta$$, e si stima (con eventuali ripesature)

$$
\widehat{\mathbb{E}(g(\boldsymbol{\theta})\vert \mathbf{y})} = \sum_{l=1}^L g(\boldsymbol{\theta}_l) p(\boldsymbol{\theta}_l \vert \mathbf{y})
$$

*   *Metodi di simulazione*: si campionano $$\boldsymbol{\theta}^{(1)}, \dots, \boldsymbol{\theta}^{(M)}$$ dalla *posterior* $$p(\boldsymbol{\theta}\vert\mathbf{y})$$, e si stima (con eventuali ripesature)

$$
\widehat{\mathbb{E}(g(\boldsymbol{\theta})\vert \mathbf{y})} = M^{-1} \sum_{m=1}^M g(\boldsymbol{\theta}^{(m)})
$$

*   *Metodi approssimanti*: si rimpiazza l'integrale che definisce $$\mathbb{E}(g(\boldsymbol{\theta})\vert \mathbf{y})$$ con una sua approssimazione.

Dimensione di $$\boldsymbol{\theta}$$ e $$\mathbf{y}$$ è cambiata nel tempo: il paper fa vedere in ordine cronologico come i metodi computazionali Bayesiani sono evoluti per tenerne conto.

## Section 5: The 21st Century: A Second Computational Revolution

### Limiti di MCMC e IS

*   Metodi non scalabili se $$\text{dim}(\Theta)$$ è elevato, con stime $$\widehat{\mathbb{E}(g(\boldsymbol{\theta})\vert \mathbf{y})}$$ potenzialmente inaccurate e computazionalmente onerose
*   La rappresentazione del numeratore della *posterior* ($$p(\mathbf{y} \vert \boldsymbol{\theta}) p(\boldsymbol{\theta})$$) richiede la *likelihood* $$p(\mathbf{y} \vert \boldsymbol{\theta})$$ in forma chiusa e \textbf{calcolabile} per ciascun $$\mathbf{y}$$. Se non lo è, MCMC e IS **non funzionano**!

L'assunzione che $$p(\mathbf{y}\vert\boldsymbol{\theta})$$ si possa sempre calcolare è limitante perchè:

1.    Alcuni *DGPs* non ammettono pdf in forma chiusa: e.g. distribuzioni di probabilità definiti da quantili o da funzioni generatrici, Markov Random Fields, etc...
2.    Anche se $$p(\mathbf{y}\vert\boldsymbol{\theta})$$ ha forma chiusa (con $$\boldsymbol{\theta}$$ fissato), il calcolo punto-per-punto è $$O(n)$$-complesso e rende MCMC e IS non scalabili sui *big data*

Alcune metodologie computazionali *moderne* per ovviare a questi problemi...

### Metodologie computazionali Bayesiane moderne

*   **Pseudo-Marginal methods (PM)**
*   Metodi per inferenza Bayesiana approssimata
    +   **Approximate Bayesian computation (ABC)**
    +   **Bayesian synthetic likelihood (BSL)**
    +   **Variational Bayes (VB)**
    +   Integrated nested Laplace approximation (INLA)
*   Metodi ibridi (Section 5.4)
*   Sviluppi su algoritmi MCMC (Section 5.5)

Alcuni dettagli dei metodi evidenziati...

### Pseudo-Marginal methods (PM)

*   Se faccio *data augmentation* introducendo $$\mathbf{z}$$ latente, dovrò generare da $$p(\boldsymbol{\theta}, \mathbf{z}\vert\mathbf{y})$$ e le *draws* di $$\boldsymbol{\theta}$$ approssimano $$p(\boldsymbol{\theta}\vert\textbf{y})$$
*   Beaumont (2003) + Andrieu and Roberts (2009) notano che le *draws* di $$\mathbf{z}$$ si possono *utilizzare meglio* per simulare da $$p(\boldsymbol{\theta}\vert\textbf{y})$$
*   Idea: data $$h(\mathbf{y}\vert\boldsymbol{\theta},\mathbf{z})$$ stima non distorta di $$p(\mathbf{y}\vert\boldsymbol{\theta})$$ (cioè $$\mathbb{E}_{\textbf{z}}[h(\mathbf{y}\vert\boldsymbol{\theta},\mathbf{z})]=p(\mathbf{y}\vert\boldsymbol{\theta})$$) e $$h(\mathbf{z})$$ distribuzione della latente $$\mathbf{z}$$, 

$$
h(\boldsymbol{\theta}\vert\mathbf{y}) \propto p(\boldsymbol{\theta}) \mathbb{E}_{\textbf{z}}[h(\mathbf{y}\vert\boldsymbol{\theta},\mathbf{z})] = p(\boldsymbol{\theta}) p(\mathbf{y}\vert\boldsymbol{\theta}) \propto p(\boldsymbol{\theta}\vert\mathbf{y})
$$

*   Posso fare pseudo-marginal Metropolis Hasting (PMMH) sostituendo

$$
\alpha_{MCMC} = \min \left\lbrace \frac{p(\mathbf{y}\vert \boldsymbol{\theta}^*)p(\boldsymbol{\theta}^*) / q(\boldsymbol{\theta}^{(t-1)}\vert \boldsymbol{\theta}^*, \mathbf{y})}{p(\mathbf{y}\vert \boldsymbol{\theta}^{(t-1)})p(\boldsymbol{\theta}^{(t-1)}) / q(\boldsymbol{\theta}^* \vert \boldsymbol{\theta}^{(t-1)}, \mathbf{y})}, 1 \right\rbrace
$$

con

$$
\alpha_{PMMH} = \min \left\lbrace \frac{h(\mathbf{y}\vert \boldsymbol{\theta}^*, \mathbf{z}^*)p(\boldsymbol{\theta}^*) / q(\boldsymbol{\theta}^{(t-1)}\vert \boldsymbol{\theta}^*, \mathbf{y})}{p(\mathbf{y}\vert \boldsymbol{\theta}^{(t-1)}, \mathbf{z}^{(t-1)})p(\boldsymbol{\theta}^{(t-1)}) / q(\boldsymbol{\theta}^* \vert \boldsymbol{\theta}^{(t-1)}, \mathbf{y})}, 1 \right\rbrace
$$

### Vantaggi dei PM methods

1.    Quando $$\boldsymbol{\theta}$$ e $$\mathbf{z}$$ sono fortemente correlate, utilizzare PMMH anzichè Gibbs Sampler (GS) può essere **più efficiente**. Inoltre, nel PMMH non è necessario campionare da $$p(\mathbf{z}\vert \boldsymbol{\theta}, \mathbf{y})$$, a differenza del GS
2.    Quando si può generare dal processo latente solo facendo *forward simulation* (e.g. SSM) ed il calcolo punto-per-punto di $$p(\mathbf{y},\mathbf{z}\vert\boldsymbol{\theta})$$ è infattibile, PMMH rimane **comunque possibile**
3.    Quando $$\text{dim}(\mathcal{Y})$$ è grande, una stima non distorta di $$p(\mathbf{y}\vert\boldsymbol{\theta})$$ basata su *subsambles* può essere utilizzata per produrre un valido schema PMMH, ad un **costo computazionale nettamente minore** di qualunque altro schema di generazione (vedasi Quiroz et al. 2018, 2019)

### Inferenza Bayesiana approssimata (ABC, BSL, VB e INLA)

*   Nei metodi computazionali Bayesiani *classici* si stima la $$p(\boldsymbol{\theta}\vert\mathbf{y})$$ in modo *esatto* (per modo di dire...)
*   Inferenza Bayesiana approssimata intende \textbf{approssimare} la $$p(\boldsymbol{\theta}\vert\mathbf{y})$$, ammettendo dunque un certo margine di errore, nel modo più accurato possibile
*   Benefici: per ABC e BSL non è necessario saper calcolare $$p(\mathbf{y}\vert\boldsymbol{\theta})$$, mentre per INLA e VB ottengono guadagni computazionali in problemi *high-dimensionals*, sostituendo il concetto di *simulazione* con quello di *ottimizzazione*

### Approximate Bayesian computation (ABC)

Vedasi Marin et al. (2011), Sisson and Fan (2011), Sisson et al. (2019) per approfondimenti.

*   Obbiettivo: approssimare $$p(\boldsymbol{\theta}\vert\mathbf{y})$$ in casi in cui si può simulare $$p(\mathbf{y}\vert\boldsymbol{\theta})$$, anche se non è calcolabile
*   Algoritmo ABC nella sua essenza prevede:
    1.    Simulo $$\boldsymbol{\theta}^*_i$$ da $$p(\boldsymbol{\theta})$$ e $$\mathbf{x}_i^*$$ da $$p(\cdot \vert \boldsymbol{\theta}_i^*)$$ per $$i=1, \dots, M$$
    2.    Utilizzo $$\mathbf{x}_i^*$$ per costruire una *summary statistic* $$\eta(\mathbf{x}_i^*)$$ (eventualmente vettoriale), per $$i=1, \dots, M$$
    3.    Conservo tutti i $$\boldsymbol{\theta}^*_i$$ associati alle $$\eta(\mathbf{x}_i^*)$$ tali per cui $$d(\eta(\mathbf{x}_i^*), \eta(\mathbf{y})) < \epsilon$$
    
Se $$\eta(\mathbf{y})$$ è **statistica sufficiente** per $$\boldsymbol{\theta}$$ e $$\epsilon \rightarrow 0$$, ABC produce *draws* dalla vera *posterior* $$p(\boldsymbol{\theta}\vert\mathbf{y})$$.

Nella pratica, siccome $$\epsilon > 0$$ e $$M < \infty$$, e spesso la statistica sufficiente per $$\boldsymbol{\theta}$$ è ignota, ABC produce $$\hat{p_\epsilon}(\boldsymbol{\theta}\vert\eta(\mathbf{y}))$$, ossia una **approssimazione di** $$p(\boldsymbol{\theta}\vert\eta(\mathbf{y}))$$.

#### Differenza tra $$\hat{p_\epsilon}(\boldsymbol{\theta}\vert\eta(\mathbf{y}))$$ e $$p(\boldsymbol{\theta}\vert\mathbf{y})$$

Due componenti rendono le due quantità diverse:

1.    Differenza tra $$p(\boldsymbol{\theta}\vert\eta(\mathbf{y}))$$ e la vera $$p(\boldsymbol{\theta}\vert\mathbf{y})$$
2.    Differenza tra $$\hat{p_\epsilon}(\boldsymbol{\theta}\vert\eta(\mathbf{y}))$$ e $$p(\boldsymbol{\theta}\vert\eta(\mathbf{y}))$$

La 1. è quella critica delle due, e dipende dall'informatività della $$\eta(\cdot)$$ scelta: tanto più vicina $$\eta(\cdot)$$ è alla statistica sufficiente per $$\boldsymbol{\theta}$$, tanto più $$p(\boldsymbol{\theta}\vert \eta(\mathbf{y}))$$ è *vicina* a $$p(\boldsymbol{\theta}\vert \mathbf{y})$$. Una buona scelta è definire $$\eta(\mathbf{y})$$ come funzione di $$\hat{\boldsymbol{\theta}}_{MLE}$$ (Drovandi et al., 2011, 2015).

La 2. è tanto meno marcata quanto più $$\epsilon$$ è piccolo e $$M$$ è grande, qualunque sia la $$\eta(\cdot)$$ scelta.

Ricerca nell'ambito ABC riguarda il **criterio** di accettazione/rifiuto di $$\boldsymbol{\theta}_i^*$$ e capire le proprietà dell'ABC se si utilizzano **misure empiriche alternative** alle *summary statistics* (tipo statistiche d'ordine, etc...). 

### Bayesian synthetic likelihood (BSL)

*   ABC simula da $$p(\boldsymbol{\theta}\vert\eta(\mathbf{y})) \propto p(\eta(\mathbf{y})\vert\boldsymbol{\theta}) p(\boldsymbol{\theta})$$, con $$p(\eta(\mathbf{y})\vert\boldsymbol{\theta})$$ essa stessa **approssimazione della *likelihood*** $$p(\mathbf{y}\vert\boldsymbol{\theta})$$ quando (come nella maggior parte dei casi) $$\eta(\mathbf{y})$$ è non-sufficiente
*   Questa è esprimibile come

$$
p_\epsilon(\eta(\mathbf{y})\vert\boldsymbol{\theta}) = \int_{\mathcal{X}} p(\mathbf{x}\vert\boldsymbol{\theta}) \mathbb{I}(d(\eta(\mathbf{y}), \eta(\mathbf{x})) \leq \epsilon) d\mathbf{x},
$$

ed è approssimabile con

$$
\hat{p}_\epsilon(\eta(\mathbf{y})\vert\boldsymbol{\theta}^*_i) = \mathbb{I}(d(\eta(\mathbf{y}), \eta(\mathbf{x}^*_i)) \leq \epsilon) 
$$

per una certa *draw* $$\boldsymbol{\theta}^*_i$$ e associato $$\eta(\mathbf{x}^*_i)$$.

*   Seguendo Andrieu and Roberts (2009) + Bornn et al. (2017), $$\hat{p}_\epsilon(\eta(\mathbf{y})\vert\boldsymbol{\theta}^*_i)$$ funge da stima della *likelihood* all'interno di uno schema PMMH (chiamato ABC-MCMC) per campionare da $$p_\epsilon(\boldsymbol{\theta}\vert\eta(\mathbf{y})) \propto p_\epsilon(\eta(\mathbf{y})\vert\boldsymbol{\theta}) p(\boldsymbol{\theta})$$.

*   Price et al. (2018) adottano un'approssimazione parametrica per $$p(\eta(\mathbf{y})\vert\boldsymbol{\theta})$$, ovvero
$$p_a(\eta(\mathbf{y})\vert\boldsymbol{\theta}) \equiv N(\eta(\mathbf{y}); \mu(\boldsymbol{\theta}), \Sigma(\boldsymbol{\theta}))$$
con $$\mu(\boldsymbol{\theta}) = \mathbb{E}(\eta(\mathbf{y}))$$ e $$\Sigma(\boldsymbol{\theta}) = \text{Var}(\eta(\mathbf{y}))$$. 
*   Da questa si ottiene la *ideal BSL posterior* $$p_a(\boldsymbol{\theta}\vert\eta(\mathbf{y})) \propto p_a(\eta(\mathbf{y})\vert\boldsymbol{\theta}) p(\boldsymbol{\theta})$$, ovvero un'approssimazione alternativa di $$p(\boldsymbol{\theta}\vert\eta(\mathbf{y}))$$.
*   $$\mu(\boldsymbol{\theta})$$ e $$\Sigma(\boldsymbol{\theta})$$ vengono rimpiazzate dalle stime ottenute via simulazione Monte-Carlo $$\mu_m(\boldsymbol{\theta})$$ e $$\Sigma_m(\boldsymbol{\theta})$$, da cui la *ideal BSL posterior* diventa la ***target BSL posterior*** $$p_{a,m}(\boldsymbol{\theta}\vert\eta(\mathbf{y})) \propto p_{a,m}(\eta(\mathbf{y})\vert\boldsymbol{\theta}) p(\boldsymbol{\theta})$$ con

$$
\small
p_{a,m}(\eta(\mathbf{y})\vert\boldsymbol{\theta}) = \int_{\mathcal{X}} N(\eta(\mathbf{y}); \mu_m(\boldsymbol{\theta}), \Sigma_m(\boldsymbol{\theta})) \prod_{j=1}^m p(\eta(\mathbf{x}_j)\vert\boldsymbol{\theta}) d\mathbf{x}_1\dots d\mathbf{x}_m
$$

*   Drovandi et al. (2015) mostrano che $$p_{a,m}\longrightarrow p_a$$ per $$m \rightarrow \infty$$, quindi BSL risulta in una forma di metodo PMMH.

### Variational Bayes (VB)

*   Ormerod & Wand (2010) o Blei et al. (2017) per chi vuole approfondire.
*   Classe di algoritmi tha approssimano $$p(\boldsymbol{\theta}\vert\mathbf{y})$$ andando alla ricerca della sua *migliore approssimazione* $$q^*(\boldsymbol{\theta})$$ all'interno di una famiglia $$\mathcal{Q}$$ di distribuzioni $$q(\boldsymbol{\theta})$$
*   Approccio più seguito prevede di individuarla come

$$
q^*(\boldsymbol{\theta}) \equiv \underset{q(\boldsymbol{\theta})\in\mathcal{Q}}{\arg \min}\: \text{KL}(q(\boldsymbol{\theta}) \lVert p(\boldsymbol{\theta}\vert\mathbf{y}))
$$

*   Siccome $$\text{KL}(q(\boldsymbol{\theta}) \lVert p(\boldsymbol{\theta}\vert\mathbf{y})) = \log p(\mathbf{y}) - \text{ELBO}(q(\boldsymbol{\theta}))$$, dove $$\log p(\mathbf{y})$$ è ignoto e $$\text{ELBO}(q(\boldsymbol{\theta}))\equiv \mathbb{E}_q(\log p(\boldsymbol{\theta},\mathbf{y}) - \log q(\boldsymbol{\theta}))$$,

  $$
  q^*(\boldsymbol{\theta}) \equiv \underset{q(\boldsymbol{\theta})\in\mathcal{Q}}{\arg \max}\: \text{ELBO}(q(\boldsymbol{\theta}))
  $$

  è ciò che si utilizza, perchè più facile da risolvere.

*   VB quindi permette di fare inferenza Bayesiana con $$q^*(\boldsymbol{\theta})\approx p(\boldsymbol{\theta}\vert\mathbf{y})$$. Particolarmente efficace quando $$\dim(\Theta)$$ è elevata, e MCMC è difficile da implementare o lento nel convergere.

*   **Mean-field variational Bayes (MFBV)**: scomporre $$q(\boldsymbol{\theta}) = \prod_{m=1}^M q_m(\boldsymbol{\theta}_m)$$ in una partizione di $$\boldsymbol{\Theta}$$ per facilitare l'ottimizzazione. Occhio a non partizionare troppo, altrimenti si approssima male la *posterior*.

*   Metodi alternativi con divergenze diverse dalla KL. Ad esempio, **Expectation-Propagation (EP)** massimizza la *KL rovesciata*, $$q^*(\boldsymbol{\theta}) \equiv \underset{q(\boldsymbol{\theta})\in\mathcal{Q}}{\arg \min}\: \text{KL}(p(\boldsymbol{\theta}\vert\mathbf{y})\lVert q(\boldsymbol{\theta}))$$.

## Section 7: The Future


### Alcuni paper **importanti** secondo gli autori

Due grandi **hot topics**: modelli *miss-specified* e tecniche di simulazione per *pseudo-likelihood* generate da *loss functions*.

1.    Lyddon et al. (2019) e Syring and Martin (2019): tecniche bootstrap per il calcolo di *general Bayesian posteriors*, in cui la *likelihood* di un possibile modello *miss-specified* viene sostituita da una trasformazione di una *loss function*
2.    Frazier et al. (2020): analizzano proprietà teoriche dell'ABC sotto *model miss-specification*
3.    Wang and Blei (2019): analizzano VB sotto *model miss-specification* e proprietà asintotiche
4.    Knoblauch et al. (2019): propongono *generalized variational inference* estendendo la specificazione del VB per fare in modo che ammetta *loss functions* e costruendo strumenti computazionali di ottimizzazione per farci inferenza
5.    Bornn et al. (2019): derivano uno schema MCMC per campionare da varietà *lower-dimensional* implicate dal condizionamento su particolari momenti della distribuzione.


## Commenti?