<h1>Depression Prediction and Classification Using Bayesian Logistic Regression</h1>

<p>This repository contains an analysis of depression prediction and classification using Bayesian logistic regression models. The models are built to predict depression based on individual-level and village-level covariates, utilizing both standard and multilevel Bayesian logistic regression techniques. The code processes real-world survey data to classify individuals as depressed or not, based on various demographic factors.</p>

<h2>Project Overview</h2>
<p>The project implements multiple Bayesian models using the <code>brms</code> package for Bayesian regression in R. It focuses on exploring individual and group-level predictors of depression through logistic regression and posterior predictive checks. The models include both single-level and multilevel approaches to improve the predictive accuracy of depression cases in a given population.</p>

<h3>Key Features</h3>
<ul>
    <li><strong>Data Preprocessing:</strong> Factorization, handling of missing values, and downsampling to balance the dataset.</li>
    <li><strong>Multiple Models:</strong>
        <ul>
            <li><strong>M1:</strong> Bayesian binary logistic regression with two covariates.</li>
            <li><strong>M2:</strong> Binomial logistic regression with group-level covariates (village-based).</li>
            <li><strong>M3:</strong> Bayesian logistic regression with six covariates (all personal information).</li>
            <li><strong>M4:</strong> Bayesian logistic regression with standardized covariates and specific priors.</li>
            <li><strong>M5:</strong> Multilevel Bayesian logistic regression using both individual- and group-level covariates.</li>
        </ul>
    </li>
    <li><strong>Model Evaluation:</strong> Posterior predictive checks, AUC (Area Under the Curve) analysis, and leave-one-out cross-validation (LOO-CV) for model comparison.</li>
    <li><strong>Visualization:</strong> Histograms of variables, trace plots, autocorrelation plots, and posterior predictive checks for model diagnostics.</li>
</ul>

<h2>Getting Started</h2>

<h3>Prerequisites</h3>
<p>Before running the code, ensure you have the following R libraries installed:</p>
<pre><code>install.packages(c("dplyr", "tidyr", "tidyverse", "haven", "sjstats", "ROCR", "brms", "modelr", "tidybayes", "bayesplot", "ggplot2", "rstan", "caret"))
</code></pre>

<h3>Data</h3>
<p>The analysis uses two datasets:</p>
<ol>
    <li><strong>Training Data:</strong> <code>b_depressed.csv</code> - Contains individual-level data with variables such as sex, age, marital status, number of children, education level, and depression status.</li>
    <li><strong>Test Data:</strong> <code>test.csv</code> - Used for model evaluation.</li>
</ol>

<h3>Data Preprocessing</h3>
<ul>
    <li><strong>Factorization:</strong> Convert categorical variables such as <code>sex</code>, <code>Married</code>, and <code>depressed</code> into factors.</li>
    <li><strong>Downsampling:</strong> The dataset is downsampled to create a balanced dataset for better model training.</li>
</ul>

<h3>Model Overview</h3>

<h4>M1: Bayesian Binary Logistic Regression</h4>
<p>Predict depression using <code>sex</code> and <code>Married</code> as covariates. Diagnostics include trace plots, autocorrelation, and posterior predictive checks.</p>

<h4>M2: Bayesian Binomial Logistic Regression (Group-Level)</h4>
<p>Uses the number of children per village to predict the proportion of depressed individuals. Data visualization explores the relationship between the number of children and depression at the village level.</p>

<h4>M3: Bayesian Logistic Regression (with Six Covariates)</h4>
<p>Includes covariates: <code>sex</code>, <code>Age</code>, <code>Married</code>, <code>Number_children</code>, <code>education_level</code>, and <code>total_members</code>. Model fit is evaluated with posterior predictive checks and AUC.</p>

<h4>M4: Standardized Bayesian Logistic Regression with Priors</h4>
<p>Standardizes covariates before fitting the model and uses specified priors. Analyzes parameter estimates and their uncertainty intervals.</p>

<h4>M5: Bayesian Multilevel Logistic Regression</h4>
<p>Incorporates random effects for villages (<code>Ville_id</code>) in addition to individual-level covariates. Multilevel modeling improves predictive performance by accounting for clustering within villages.</p>

<h2>Model Evaluation and Comparison</h2>
<ul>
    <li><strong>AUC (Area Under the Curve):</strong> Computes the AUC for each model to assess classification performance.</li>
    <li><strong>Posterior Predictive Checks:</strong> Compares observed and simulated proportions of depressed individuals for model validation.</li>
    <li><strong>Leave-One-Out Cross-Validation (LOO-CV):</strong> Compares the predictive accuracy of models using LOO-CV.</li>
</ul>

<h2>Usage</h2>
<p>To run the code, follow these steps:</p>
<ol>
    <li><strong>Load the Dataset:</strong> Modify the file paths to point to your dataset:</li>
    <pre><code>df <- read.csv("path/to/b_depressed.csv")
df_test <- read.csv("path/to/test.csv")
</code></pre>
    <li><strong>Run Models:</strong> Each model (<code>M1</code>, <code>M2</code>, <code>M3</code>, <code>M4</code>, <code>M5</code>) is defined in the script. You can run them sequentially to evaluate their performance.</li>
    <li><strong>Model Diagnostics:</strong> Use <code>mcmc_plot</code> to visualize trace plots, autocorrelation plots, and parameter estimates.</li>
    <li><strong>Compare Models:</strong> Use the <code>loo()</code> function to compare models using leave-one-out cross-validation:</li>
    <pre><code>loo(M1, M3, M4, M5)
</code></pre>
</ol>

<h2>Visualization</h2>
<ul>
    <li><strong>Histograms:</strong> Visualize the distribution of variables such as age, number of children, education level, and total members.</li>
    <li><strong>Trace Plots:</strong> Evaluate MCMC convergence by visualizing the trace plots for each model.</li>
    <li><strong>Posterior Predictive Checks:</strong> Check the model’s ability to reproduce the observed data using simulated proportions.</li>
</ul>

<h2>Example Code</h2>
<pre><code># Fit the M1 model (Bayesian Binary Logistic Regression)
M1 <- brm(formula = depressed ~ sex + Married,   
          data=df_updated,  
          family = bernoulli(link = "logit"))

# Summary of M1
summary(M1)

# Trace Plot for M1
mcmc_plot(M1, type = "trace")
</code></pre>

<h2>Results</h2>
<ul>
    <li><strong>Posterior Predictive Checks:</strong> Evaluate model fit by comparing observed and simulated data.</li>
    <li><strong>AUC Scores:</strong> Assess model classification performance.</li>
    <li><strong>LOO-CV:</strong> Compare models based on cross-validation metrics.</li>
</ul>

<h2>Conclusion</h2>
<p>This project demonstrates the use of Bayesian logistic regression models for predicting depression using real-world survey data. The multilevel model (M5) incorporating both individual- and group-level covariates showed the best predictive performance. These models provide valuable insights into the factors associated with depression and help improve the identification of individuals at risk.</p>

<h2>References</h2>
<ul>
    <li><a href="https://cran.r-project.org/web/packages/brms/index.html">brms</a></li>
    <li><a href="https://mc-stan.org/bayesplot/">bayesplot</a></li>
    <li><a href="https://cran.r-project.org/web/packages/ROCR/index.html">ROCR</a></li>
</ul>
