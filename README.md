</p>
<p align="center">
  <img src="images/Causal Effects on Time Series-logo-black.png" 
       width="500">
</p>
<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![Image title](https://img.shields.io/badge/CausalImpact-v1.2.1-blue.svg) 


<p align="center">
  <a href="#Introduction"> Introduction </a> • 
  <a href="#bsts"> Bayesian Structural Time Series </a> • 
</p>

## Introduction

In this repo I will show how to use the R package `CausalImpact` to measure how ad campaigns affect outcomes such as sales or ROI. It will also be useful as an introduction to using R in a Jupyter Notebook. 


## Bayesian structural time series

First we will introduce some basic concepts regarding [Bayesian structural time series](https://en.wikipedia.org/wiki/Bayesian_structural_time_series). BSTS is a machine learning technique with a variety of useful applications. Here we will discuss two of them, namely, forecasting and causal inference. The model has three main components namely: 
- Kalman filter: where feature selection takes place using classical time series decomposition
- Spike-and-slab method: the most relevant predictors are chosen
- Bayesian model averaging: results are combined and predictions are made.
