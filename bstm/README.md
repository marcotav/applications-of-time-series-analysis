</p>
<p align="center">
  <img src="images/Causal Effects on Time Series-logo-black.png" 
       width="500">
</p>
\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![Image title](https://img.shields.io/badge/CausalImpact-v1.2.1-blue.svg) ![Image title](https://img.shields.io/badge/R-v3.5.0-green.svg) 

### Notebook by [Marco Tavora](http://www.marcotavora.me)

## Table of contents

1. [Introduction](#Introduction)
2. [Bayesian structural time series](#Bayesian-Structural-Time-Series)
3. [Application](#Application)
4. [Installing packages](#Installing-packages)
5. [Google Analytics Metrics Definitions](#Google-Analytics-Metrics-Definitions)
6. [An Example](#An-Example)
7. [Structural time-series models](#Structural time-series models)

6. [BSTS Model](#BSTS Model)

<p align="center">
  <a href="#Introduction"> Introduction </a> • 
  <a href="#bsts"> Bayesian structural time series </a> • 
</p>

## Introduction
[[go back to the top]](#Table-of-contents)

In this repo I will show how to use the R package `CausalImpact` which used Bayesian structural time series.

## Bayesian structural time series
[[go back to the top]](#Table-of-contents)

First we will introduce some basic concepts regarding [Bayesian structural time series](https://en.wikipedia.org/wiki/Bayesian_structural_time_series). BSTS is a machine learning technique with a variety of useful applications. Here we will discuss two of them, namely, forecasting and causal inference. The model has three main components namely: 
- Kalman filter: where feature selection takes place using classical time series decomposition
- Spike-and-slab method: the most relevant predictors are chosen
- Bayesian model averaging: results are combined and predictions are made.

## Application
[[go back to the top]](#Table-of-contents)

Let us try to estimate the impact of the [Deepwater Horizon oil spill](https://en.wikipedia.org/wiki/Deepwater_Horizon_oil_spill) on [BP plc](https://en.wikipedia.org/wiki/BP) (formerly British Petroleum) stock prices using Bayesian structural time series [Sengul (2018)](#References) (see section [BSTS Model](#BSTS-Model) for a slightly more formal explanation). 

### The oil was photographed from space by a NASA satellite in 24 May of 2010 and is [shown below](https://en.wikipedia.org/wiki/Deepwater_Horizon_oil_spill)

<br>
</p>
<p align="center">
  <img src="images/oil-spill.jpg" 
       width="600">
<br>

## Executive Summary of the BSTS Technique
[[go back to the top]](#Table-of-contents)

