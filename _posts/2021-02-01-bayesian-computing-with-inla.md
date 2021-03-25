---
title: "Bayesian Computing with Integrated Nested Laplace Approximation: An Overview"
author: Cristian Castiglione
date: "2021-02-01"
tags: ["Bayesian statistics", "GMRF", "INLA"]
toc: true
toc_sticky: true
---

## Introduzione

Il metodo conosciuto come *integrated nested Laplace approximation* (INLA) nasce per fornire un'alternativa efficiente ad approcci computazionalmente onerosi come *Markov chain Monte Carlo* (MCMC) per la stima di modelli Bayesiani a fattori latenti. A differenza di altri metodi di approssimazione deteministica globale, come *Laplace approximation* e *variational Bayes*, cioè che mirano ad approssimare l'intera distribuzione a posteriori, INLA fornisce un approccio di approssimazione locale, ovvero che permette di calcolare tutte le distribuzioni marginali univariate (o bivariate) dei parametri e dei fattori latenti del modello, oltre alle classiche statistiche di interesse, come verosimiglianza marginale e distribuzione predittiva, evitando approssimazioni sull'intera distribuzione a posteriori. 


## Latent Gaussian Models

Si consideri il seguente modello Bayesiano gerarchico

$$
  y_i \mid x_i, \boldsymbol{\theta} \sim \pi(y_i \mid x_i, \boldsymbol{\theta}), 
    \quad\text{for}\quad i = 1, \dots, n \\
  \boldsymbol{x} \mid \boldsymbol{\theta} \sim \mathsf{N}_n(\boldsymbol{0}_n, \boldsymbol{\Sigma}_\boldsymbol{\theta}), 
  \quad \boldsymbol{\theta} \sim \pi(\boldsymbol{\theta})
$$

dove $$y_i$$ è l'$$i$$-esima osservazione del campione, $$\boldsymbol{x}$$ è un vettore di fattori latenti Gaussiani e $$\boldsymbol{\theta}$$ è un vettore di iperparametri. Inoltre si assuma che

1. $$y_i$$ sia mutualmente condizionatamente indipendente da $$y_j$$, per ogni $$j \neq i$$, dati $$x_i$$ e $$\boldsymbol{\theta}$$;
2. $$\pi(y_i \mid x_i, \boldsymbol{\theta})$$ sia una funzione log-concava, differenziabile due volte con continuità rispetto a $$x_i$$ e $$\boldsymbol{\theta}$$;
3. $$\boldsymbol{x} \mid \boldsymbol{\theta}$$ sia un *Gaussian Markov random field* (GMRF), ovvero che $$\boldsymbol{Q}_\boldsymbol{\theta} = \boldsymbol{\Sigma}_\boldsymbol{\theta}^{-1}$$ sia una matrice (molto) sparsa;
4. $$\boldsymbol{\theta}$$ sia un vettore di "piccole" dimensioni (generalmente tra 2 e 5, ma non più grande di 20).


## INLA: approssimazione di $\pi(\boldsymbol{\theta} \mid \boldsymbol{y})$

L'intuizione di base dietro ad INLA risiede nella relazione tra distribuzioni congiunta, condizionata e marignale, la quale è definita dalle equazioni

$$
  \pi(\boldsymbol{x}, \boldsymbol{\theta} \mid \boldsymbol{y}) = \pi(\boldsymbol{x} \mid \boldsymbol{\theta}, \boldsymbol{y}) \pi(\boldsymbol{\theta} \mid \boldsymbol{y})
  \quad\Leftrightarrow\quad 
  \pi(\boldsymbol{\theta} \mid \boldsymbol{y}) = \pi(\boldsymbol{x}, \boldsymbol{\theta} \mid \boldsymbol{y}) / \pi(\boldsymbol{x} \mid \boldsymbol{\theta}, \boldsymbol{y})
$$

In particolare, si noti che, date le osservazioni e il modello, $$\pi(\boldsymbol{x}, \boldsymbol{\theta} \mid \boldsymbol{y})$$ e $$\pi(\boldsymbol{x} \mid \boldsymbol{\theta}, \boldsymbol{y})$$ sono completamente note, sebbene possano avere espressioni analitiche complesse. Inoltre, tali relazioni valgono per qualunque valore di $$\boldsymbol{x}$$, purchè esso appartenga al supporto di $$\pi(\boldsymbol{x} \mid \boldsymbol{\theta})$$, ovvero che $$\boldsymbol{x} \in \{\boldsymbol{x} : \pi(\boldsymbol{x} \mid \boldsymbol{\theta}) > 0 \}$$, tuttavia numericamente risulta preferibile che $$\boldsymbol{x}$$ venga scelto in un intorno del massimo di $$\pi(\boldsymbol{x} \mid \boldsymbol{\theta}, \boldsymbol{y})$$.

Nell'algoritmo INLA, la distribuzione marginale $$\pi(\boldsymbol{\theta} \mid \boldsymbol{y})$$ viene approssimata sostituendo a $$\pi(\boldsymbol{x} \mid \boldsymbol{\theta}, \boldsymbol{y})$$ la sua approssimazione Laplace, ovvero 

$$
  \pi(\boldsymbol{\theta} \mid \boldsymbol{y}) 
  \approx \widetilde{\pi}(\boldsymbol{\theta} \mid \boldsymbol{y})
  = \frac{\widetilde{\pi}(\boldsymbol{\theta}, \boldsymbol{y})}{\widetilde{\pi}(\boldsymbol{y})}
  \quad\text{e}\quad
  \widetilde{\pi}(\boldsymbol{\theta}, \boldsymbol{y}) 
  = \frac{
    \pi(\boldsymbol{\theta}) \pi(\boldsymbol{x} \mid \boldsymbol{\theta}) 
    \pi(\boldsymbol{y} \mid \boldsymbol{x}, \boldsymbol{\theta})}{
    \phi_n(\boldsymbol{x} \mid \boldsymbol{\mu}_{\boldsymbol{\theta}}, \boldsymbol{P}_{\boldsymbol{\theta}})
  } \Bigg|_{\boldsymbol{x} = \boldsymbol{\mu}_{\boldsymbol{\theta}}}
$$

deve definiamo

$$
  \boldsymbol{\mu}_{\boldsymbol{\theta}} \equiv \text{argmax}_{\boldsymbol{x}} 
    \big\{ \log \pi(\boldsymbol{x} \mid \boldsymbol{\theta}, \boldsymbol{y}) \big\},
  \quad
  \boldsymbol{P}_{\boldsymbol{\theta}} \equiv \boldsymbol{Q}_{\boldsymbol{\theta}} + \text{diag}\bigg[ 
    \frac{\partial^2}{\partial \boldsymbol{x}^2} 
    \log \pi(\boldsymbol{x} \mid \boldsymbol{\theta}, \boldsymbol{y}) 
  \bigg]_{\boldsymbol{x} = \boldsymbol{\mu}_{\boldsymbol{\theta}}} 
  \quad\text{e}\quad
  \widetilde{\pi}(\boldsymbol{y}) = \int_\Theta \widetilde{\pi}(\boldsymbol{\theta}, \boldsymbol{y}) \,d\boldsymbol{\theta}.
$$

Chiaramente la verosimiglianza marginale $$\widetilde{\pi}(\boldsymbol{y})$$ non può essere calcolata analiticamente, ma può essere ottenuta via quadratura numerica. Gli autori ??? suggeriscono di adottare il metodo di integrazione di Gauss-Hermitte, sfruttando così nuovamente la geometria di $$\widetilde{\pi}(\boldsymbol{\theta}, \boldsymbol{y})$$ intorno al massimo.


In questa fase entrano in gioco in maniera cruciale tutte le assunzioni fatte sul modello, in particolare:

* le assunzioni 1. e 3. permettono di calcolare in maniera efficiente $$\boldsymbol{P}_\boldsymbol{\theta}$$, la quale costituisce una perturabzaione sulla diagonale di $$\boldsymbol{Q}_\boldsymbol{\theta}$$, che rimane così una matrice sparsa, la cui inversione richiede un numero di operazioni pari a $$O(n)$$ anzichè $$O(n^3)$$;
* l'assunzione 2. è fondamentale per poter applicare l'usuale approssimazione Gaussiana intorno al massimo, la quale richiede l'esistenza di un massimo, e delle derivate prime e seconde in un suo intorno;
* l'assunzione 4., infine, permette di calcolare la costante di integrazione (verosimiglianza marignale) tramite integrazione numerica.


## INLA: approssimazione di $$\pi(x_i \mid \boldsymbol{y})$$

Resta da trovare la distribuzione marginale di $$x_i \mid \boldsymbol{y}$$, la quale, può essere calcolata come

$$
  \pi(x_i \mid \boldsymbol{y}) 
    \approx \int_\Theta \widetilde{\pi}(x_i \mid \boldsymbol{\theta}, \boldsymbol{y}) \,\widetilde{\pi}(\boldsymbol{\theta} \mid \boldsymbol{y}) \,d\boldsymbol{\theta}
$$

dove $$\widetilde{\pi}(x_i \mid \boldsymbol{\theta}, \boldsymbol{y})$$ è un'adeguata approssimazione di $$\pi(x_i \mid \boldsymbol{\theta}, \boldsymbol{y})$$, che può essere ottenuta, tra le altre possibilità, con un'approssimazione di Laplace univariata di terzo ordine, o con una proiezione di $$\pi(x_i \mid \boldsymbol{\theta}, \boldsymbol{y})$$ nello spazio delle distribuzioni Gaussiane asimmetriche. 

Come per la verosimiglianza marginale, anche il calcolo di $$\widetilde{\pi}(x_i \mid \boldsymbol{y})$$ richiede un'integrazione numerica implementabile attraverso l'algoritmo di Gauss-Hermite. Tuttavia, se il numero di marginali da calcolare è molto elevato, come è ipotizzabile succeda in un'applicazione reale, una quadratura numerica con un alto numerodi nodi di integrazione può risultare inefficiente. Una possibile soluzione è di scegliere un numero di nodi basso, ma la cui posizione sia scelta in modo tale questi giacciano approssimativamente su una circonferenza intorno al punto di massimo determinata dalle curve di livello della funzione $$\widetilde{\pi}(x_i \mid \boldsymbol{\theta}, \boldsymbol{y}) \,\widetilde{\pi}(\boldsymbol{\theta}$$.

Analogamente, la stessa strategia può essere applicata per marginalizzare sia $x$, che $\boldsymbol{\theta}$ nel calcolo della distribuzione predittiva approssimata

$$
  \widetilde{\pi}(y \mid \boldsymbol{y}) 
  = \iint_{\mathbb{R} \times \Theta} \pi(y \mid x, \boldsymbol{\theta}) 
    \,\widetilde{\pi}(x \mid \boldsymbol{\theta}, \boldsymbol{y}) 
    \,\widetilde{\pi}(\boldsymbol{\theta} \mid \boldsymbol{y}) \,dx \,d\boldsymbol{\theta}
$$



## Bibliografia

- Rue, Havard, Martino, Sara (2007). Approximate Bayesian inference for hierarchical Gaussian Markov random field models. *Journal of statistical planning and inference*. 137(10): 3177--3192.

- Rue, Havard, Martino, Sara, Chopin, Nicolas (2009). Approximate Bayesian inference for latent Gaussian models by using integrated nested Laplace approximations. *Journal of the royal statistical society: Series b (statistical methodology)*. 71(2): 319--392.

- Rue, Havard, Riebler, Andrea, Sorbye, Sigrunn H., Illian, Janine B., Simpson, Daniel P., Lindgren, Finn K. (2017). Bayesian computing with INLA: a review. *Annual Review of Statistics and Its Application*. 4: 395--421.







