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
\begin{itemize}
	\item {\bf Presentazione di un paradigma unificato per l'inferenza Bayesiana}
	\item {\bf Risultato di ottimalità sulla "standard" Variational Inference (VI)}
	\item {\bf Generalized Variational Inference (GVI) e le sue proprietà}
	\item {\bf Algoritmo Black-Box per l'inferenza}
	\item Applicazioni alle Bayesian Neural Networks e a Processi Gaussiani
\end{itemize}
... e una lunga appendice tecnica! In questo breve post mi occuperò di presentare gli argomenti in \textbf{grassetto}.


