﻿# EECS 738 Project 1: Distribution Fitting
By Alex Ramos

# Files

 - **`CarGMM.py`** : Script operates on the AutoMPG dataset and fits a distribution to the auto *weight*
 - **`WineGMM.py`** : operates on the WineQuality dataset and fits a distribution to the wine *sulphate*
 - **/Datasets**: contains the datasets {autompg, winequality} from the Kaggle UCI project
 - **/Results**: contains images and CSV files that my algorithm generates for each dataset
 - **/Notebooks**: contains the Jupyter Notebook I used to build the algo; I'll try to implement an HTML preview

The Datasets folder must be in the same directory of the `.py` files

## Note
The best example of the code is viewed conveniently from your web browser here:

https://github.com/e-ramos/738Project1/blob/master/Notebooks/Simple1DFinal.ipynb

Scroll through to view results

# The Tools of the Trade
The goals of this project were somewhat vague and open ended. This is not critique, but praise. There is hardly a better way to learn than to be forced to grapple with ambiguity and refine your own approach. (read: do the easiest thing possible)

A particular question, although simple, motivates just about every field of science:   

> After some observations, can we predict an outcome?

This is, of course, predicated by the more mundane:
> After some observations, what the heck am I even looking at?

Before we can make decisions about some system, we must understand its mechanics; or at the least pretend we do. We call this delusion a *model*. 

## Modeling Data with the GMM
A probability distribution most are familiar with is the Gaussian or *normal* pdf. With the central limit theorem in our back pockets, it makes sense to model a large observed dataset with a gaussian. But why stop there? 
It's helpful to model our data as the sum of many gaussians to more closely approximate the observed distribution with sub-infinite sample support. We call this the Gaussian Mixture Model. It is basically a "method of moments" type approach using the gaussian as a basis function since its integral can be calculated and its defined by only two parameters.

## Expectation Maximization
The code given is a one dimensional application of the well known Expectation Maximization algorithm. 
Given some known model order K, it is outlined in words as:

 - [x] "E Step"

Given current guesstimate of the distributions, and all data, calculate the posterior probabilities.

 - [x] "M Step"

Using these probabilities and applying them as weights to the data, find new means and variances. Maximizing the expectation for the data.

 - [x] Iterate
 
 ## Model Order Estimation
 There are more intelligent methods of determining if a model fits well, but I applied a naive exhaustive search over the space K ={1, 2,...10}. Where K is the number two gaussians ad infinitum for 2K total parameters to estimate.
 
To determine "goodness" of fit, I used the Bayesian Information Criterion (BIC) to penalize each additional parameter.

It should be obvious that it is a bad idea to estimate a line with a 26th order polynomial. Similarly, overfitting the gaussians to the training data will make our model poor in predicting new data.

BIC has a seemingly ad-hoc nature to it, and I found that, visually, models looked best when there was a sharp change in the concavity of the BIC at that model order index (K). 

## Sharp Concavity at k = 2

![Sharp Concavity at k = 2](https://github.com/e-ramos/738Project1/blob/master/Results/Cars/BIC.png)

## k=2 Fit

![k=2 Fit](https://github.com/e-ramos/738Project1/blob/master/Results/Cars/Final2.png)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2OTg5NTAwNjNdfQ==
-->