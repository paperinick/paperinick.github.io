---
title: "Generalized Variational Inference"
subtitle: A summary of Knoblauch et al, 2019
author: Nicolas Bianco
date: "2021-03-18"
tags: ["Bayesian inference", "Variational Bayes"]
toc: true
toc_sticky: true
---


## Struttura del paper
L'articolo dal titolo "Generalized Variational Inference: Three arguments for deriving new Posteriors" di Jeremias Knoblauch, Jack Jewson e Theodoros Damoulas disponibile su [arXiv](https://arxiv.org/abs/1904.02063) è strutturato nei seguenti punti:

  1. **Presentazione di un paradigma unificato per l'inferenza Bayesiana**
  2. **Risultato di ottimalità sulla "standard" Variational Inference (VI)**
  3. **Generalized Variational Inference (GVI) e le sue proprietà**
  4. **Algoritmo Black-Box per l'inferenza**
  5. Applicazioni alle Bayesian Neural Networks e a Processi Gaussiani

...e una lunga appendice tecnica! 

In questo breve post mi occuperò di presentare gli argomenti in **grassetto**.

## Assunzioni nell'inferenza Bayesiana
Lo scopo dell'inferenza Bayesiana è quello di trovare una distribuzione a posteriori per i parametri $q^*_B(\mathbf{\vartheta})$, data una prior $\pi(\mathbf{\vartheta})$ e un modello per i dati osservati. Assunzioni di base sono:

  1. \textbf{P}: Corretta specificazione della \textbf{P}rior, nel senso che debba rappresentare la miglior conoscenza che abbiamo
  2. \textbf{L}: Esiste un parametro per il quale la \textbf{L}ikelihood coincide esattamente con il processo generatore dei dati
  3. \textbf{C}: I tempi e la potenza \textbf{C}omputazionale sono infiniti

Se questi tre punti valgono, allora la posteriori $q^*_B(\mathbf{\vartheta})$ è l'unica distribuzione desiderabile!

Ma nella pratica...

  1. \textbf{P}: La complessità e l'ammontare di parametri rende impossibile specificare correttamente la conoscenza a priori!
  2. \textbf{L}: Tutti i modelli sono sbagliati!
  3. \textbf{C}: I tempi e la potenza computazionale limitata per modelli molto complessi!

## La regola del 3
Un primo contributo degli autori consiste nel giustificare la scrittura di un qualsiasi problema Bayesian in un modo molto generale $P(\ell_n,\mathcal{D},\Pi)$:

$$q_B^*(\boldsymbol{\vartheta}) = \arg\min_{q\in\Pi}\,\mathcal{L}(q|\mathbf{x},\ell_n,\mathcal{D}),
$$

e in particolare:

$$\mathcal{L}(q|\mathbf{x},\ell_n,\mathcal{D})=\mathbb{E}_{q(\boldsymbol{\vartheta})}\left[\ell_n(\boldsymbol{\vartheta},\mathbf{x})\right] + \mathcal{D}(q||\pi)$$,

specificando solo 3 quantità fondamentali:
  1. **Loss** $\ell_n$: collega i parametri alle osservazioni.
	2. **Uncertainty quantifier** $\mathcal{D}$: solitamente una divergenza. Ha la funzione di determinare l'incertezza nella posteriori.
	3. **Admissible posteriors** $\Pi \subseteq \mathcal{P}(\Theta)$: un insime di possibili distribuzioni valide tra cui cercare la posteriori.

Un caso particolare si ha quando $\Pi$ è una famiglia di distribuzioni variazionali $\mathcal{Q}$ e l'inferenza che si ottiene viene chiamata (Generalized) Variational Inference.


## "Standard" Variational Inference (VI)
Con la dicitura "standard" ci riferiamo al caso in cui la loss function sia la negative log-likelihood e la divergenza scelta sia la Kullback-Leibler  $P(\ell_n,\mathsf{KL},\mathcal{Q})$. Solitamente si forniscono due principali interpretazioni:

  1. VI come massimizzazione dell'evidenza (ELBO)
  2. VI come minimizzazione della divergenza
  
Grazie alla scrittura del problema Bayesiano mediante la **regola del 3**, gli autori suggeriscono di interpretare VI come un **problema di ottimizzazione vincolato**.

![VI as a $\mathcal{Q}$-constrained optimization](../assets/images/generalizedVI/Opt.png){:width="450px"}

