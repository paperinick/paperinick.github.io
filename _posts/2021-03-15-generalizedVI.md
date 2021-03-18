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


