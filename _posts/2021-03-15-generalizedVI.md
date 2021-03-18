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

...e una lunga appendice tecnica! In questo breve post mi occuperò di presentare gli argomenti in **grassetto**.

## Assunzioni nell'inferenza Bayesiana
Lo scopo dell'inferenza Bayesiana è quello di trovare una distribuzione a posteriori per i parametri $q^*_B(\mathbf{\vartheta})$, data una prior $\pi(\mathbf{\vartheta})$ e un modello per i dati osservati. Assunzioni di base sono:

  1. \textbf{P}: Corretta specificazione della \textbf{P}rior, nel senso che debba rappresentare la miglior conoscenza che abbiamo
  2. \textbf{L}: Esiste un parametro per il quale la \textbf{L}ikelihood coincide esattamente con il processo generatore dei dati
  3. \textbf{C}: I tempi e la potenza \textbf{C}omputazionale sono infiniti

Se questi tre punti valgono, allora la posteriori $q^*_B(\mathbf{\vartheta})$ è l'unica distribuzione desiderabile!

Ma nella prtica...

  1. \textbf{P}: La complessità e l'ammontare di parametri rende impossibile specificare correttamente la conoscenza a priori!
  2. \textbf{L}: Tutti i modelli sono sbagliati!
  3. \textbf{C}: I tempi e la potenza computazionale limitata per modelli molto complessi!

