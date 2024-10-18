Depression Prediction and Classification Using Bayesian Logistic Regression
This repository contains an analysis of depression prediction and classification using Bayesian logistic regression models. The models are built to predict depression based on individual-level and village-level covariates, utilizing both standard and multilevel Bayesian logistic regression techniques. The code processes real-world survey data to classify individuals as depressed or not, based on various demographic factors.

Project Overview
The project implements multiple Bayesian models using the brms package for Bayesian regression in R. It focuses on exploring individual and group-level predictors of depression through logistic regression and posterior predictive checks. The models include both single-level and multilevel approaches to improve the predictive accuracy of depression cases in a given population.

Key Features
Data Preprocessing: Factorization, handling of missing values, and downsampling to balance the dataset.
Multiple Models:
M1: Bayesian binary logistic regression with two covariates.
M2: Binomial logistic regression with group-level covariates (village-based).
M3: Bayesian logistic regression with six covariates (all personal information).
M4: Bayesian logistic regression with standardized covariates and specific priors.
M5: Multilevel Bayesian logistic regression using both individual- and group-level covariates.
Model Evaluation: Posterior predictive checks, AUC (Area Under the Curve) analysis, and leave-one-out cross-validation (LOO-CV) for model comparison.
Visualization: Histograms of variables, trace plots, autocorrelation plots, and posterior predictive checks for model diagnostics.
Getting Started
Prerequisites
Before running the code, ensure you have the following R libraries installed:

r
Copy code
install.packages(c("dplyr", "tidyr", "tidyverse", "haven", "sjstats", "ROCR", "brms", "modelr", "tidybayes", "bayesplot", "ggplot2", "rstan", "caret"))
Data
The analysis uses two datasets:

Training Data: b_depressed.csv - Contains individual-level data with variables such as sex, age, marital status, number of children, education level, and depression status.
Test Data: test.csv - Used for model evaluation.
Data Preprocessing
Factorization: Convert categorical variables such as sex, Married, and depressed into factors.
Downsampling: The dataset is downsampled to create a balanced dataset for better model training.
Model Overview
M1: Bayesian Binary Logistic Regression

Predict depression using sex and Married as covariates.
Diagnostics: Trace plots, autocorrelation, and posterior predictive checks.
M2: Bayesian Binomial Logistic Regression (Group-Level)

Use the number of children per village to predict the proportion of depressed individuals.
Visualization: Explore the relationship between the number of children and depression at the village level.
M3: Bayesian Logistic Regression (with Six Covariates)

Includes covariates: sex, Age, Married, Number_children, education_level, and total_members.
Evaluate model fit with posterior predictive checks and AUC.
M4: Standardized Bayesian Logistic Regression with Priors

Standardize covariates before fitting the model and use specified priors.
Analyze parameter estimates and their uncertainty intervals.
M5: Bayesian Multilevel Logistic Regression

Incorporates random effects for villages (Ville_id) in addition to individual-level covariates.
Multilevel modeling improves predictive performance by accounting for clustering within villages.
Model Evaluation and Comparison
AUC (Area Under the Curve): Computes the AUC for each model to assess classification performance.
Posterior Predictive Checks: Compares observed and simulated proportions of depressed individuals for model validation.
Leave-One-Out Cross-Validation (LOO-CV): Compares the predictive accuracy of models using LOO-CV.
Usage
Load the Dataset: Modify the file paths to point to your dataset:

r
Copy code
df <- read.csv("path/to/b_depressed.csv")
df_test <- read.csv("path/to/test.csv")
Run Models: Each model (M1, M2, M3, M4, M5) is defined in the script. You can run them sequentially to evaluate their performance.

Model Diagnostics: Use mcmc_plot to visualize trace plots, autocorrelation plots, and parameter estimates.

Compare Models: Use the loo() function to compare models using leave-one-out cross-validation:

r
Copy code
loo(M1, M3, M4, M5)
Visualization
Histograms: Visualize the distribution of variables such as age, number of children, education level, and total members.
Trace Plots: Evaluate MCMC convergence by visualizing the trace plots for each model.
Posterior Predictive Checks: Check the model’s ability to reproduce the observed data using simulated proportions.
Example Code
r
Copy code
# Fit the M1 model (Bayesian Binary Logistic Regression)
M1 <- brm(formula = depressed ~ sex + Married,   
          data=df_updated,  
          family = bernoulli(link = "logit"))

# Summary of M1
summary(M1)

# Trace Plot for M1
mcmc_plot(M1, type = "trace")
Results
Posterior Predictive Checks: Evaluate model fit by comparing observed and simulated data.
AUC Scores: Assess model classification performance.
LOO-CV: Compare models based on cross-validation metrics.
Conclusion
This project demonstrates the use of Bayesian logistic regression models for predicting depression using real-world survey data. The multilevel model (M5) incorporating both individual- and group-level covariates showed the best predictive performance. These models provide valuable insights into the factors associated with depression and help improve the identification of individuals at risk.