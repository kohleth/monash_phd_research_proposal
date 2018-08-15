# Monash PhD Research Proposal

# Imputation for Forecasting with Censored Data

Most time series forecasting methods assume the observed data is uncensored. However, censored data can arise in many real life processes. For example, a business might wish to forecast the sales of a certain (physical) product using the sales history. However, if the product was out of stock for period of time, the sales history would have been capped (censored). In this example, proceeding with the usual forecasting method will likely lead to an under-prediction of the true future sales.

Most existing literature on the topic concerns fitting the time series model while accounting for data censorship. As we will explain below, we think imputation only approach is under-researched and under-valued.

## Current state of research


#### Park et. al. (2007)
Park et. al. (2007) proposed an iterative scheme. First, the time series data is assumed to follow a multivariate gaussian distribution. Therefore, the censored portion of the time series could be imputed using the appropriate conditional distribution. Next, the parameters of the distribution are re-estimated. This process is repeated until convergence is reached.

#### Wang and Chan (2017)
Wang and Chan (2017) studied AR models with exogenous variables and censoring. They assumed the score of the most recent uncensored data block, and its expectation givens the censored data has a closed form expression. Then, exploiting the fact that scores have zero mean, they form a set of estimating equations for the model parameters.

#### Schumacher et. al. (2017)
Schumacher et. al. (2017) considered the same AR models with exogenous regressors and censoring. They used the EM algorithm to maximize the complete likelihood of both the observed data and the censoring indicator. To circumvent the difficulty in calculating the expectation, they approximate it using simulated data.

## Research Gap
As can be seen, current research mostly starts from a generative model assumption, and typically considers ARMA type models. As a result, most energy is spent on estimating model parameters and circumventing the algebraic challenge the parametric models pose.

There are (at least) 3 related reasons why a different approach could add value.

#### 1. Forecast first
In many situations, it is the forecast, instead of model parameters, that is of primary interest. In other words, instead of adjusting the model parameters (by tackling a generally difficult parametric problem), it is more direct to adjust the forecast directly. The potential disadvantage of this approach is that traditional statistical inference might become impossible to conduct, but again in many situations such inferences is not of interest.

#### 2. Broader class of models
Gaussian models with ARMA type error have been the focus of research due to their albegra being easier to handle. However, if we were to forgo this distributional framework and the benefit it brings (such as the ability to conduct statistical inferences), we could cater for a much wider class of models (ets, gam, even machine learning type models).

#### 3. The value of Imputation only approach
We believe imputation (only) approach is under-researched and under-valued.

**Under-Researched:**
Most imputation method we see starts from a generative model, from there the conditional distribution of the censored portion given the observed data is computed / approximated, before imputation is finally made. However, if we forgo the distributional framework (for reasons discussed above), there is no reason why curve/shape-based methods (e.g. splines) could not be used.

**Under-Valued:**
 This is an argument concerning real applied practices. While theory would probably suggest that making model predictions after imputation is statistically less efficient than direct estimation / prediction accounting for the censorship, in practice, a standalone (modularized) imputation step often means the analyst will be able to connect this step with the other analysis pipeline. For example, suppose an analyst wish to use a pre-determined model, or a model for which no censored data version has been developed, a standalone imputation step would allow the desired model to be used. And this need not limit to model choice. Suppose an analyst wish to use a well-developed, well-test, and feature-rich (e.g. plotting) software package for the time series analysis, but again censored data is not catered for. A standalone imputation step would allow the analyst to enjoy both better forecast, as well as the full range of features offered by the package.


## Research Plan
1. Review literature on
    - Time series forecasting
    - Censored data modeling
    - Missing data modeling
2. Develop a systematic understanding of the relationship between censored data and missing data, and their implication to time series forecasting models.
    - Missing data can be treated as censoring with infinite censoring region. In some situation, they go hand in hand. E.g. store running out of stock -- during the last week before stock completely runs out, the sales data is likely censored (capped). Then during the following weeks, before replenishment happens, the sales data will be missing (or capped at zero).
    - Many similar studies tend to consider the case when censoring is due to the machine's detection limit. We wish to study the case when censoring is due to inherent limitation of the physical process (e.g. store run out of stock), and how, if at all, such censoring mechanism affects the time series differently to the machine detection limit case.
3. Develop model agnostic imputation method of censored time series. Specifically,
    - Develop criteria to assess the goodness of the imputation with a strong focus on its impact on forecast (distributional) accuracy.
    - Develop shape-based / non-parametric times series imputation method (e.g. splines / kernel / wavelets). But instead of the standard methods, we will incorporate the censoring information into the imputation. That is, the imputed portion must lie on the censored side of the censored value. (e.g. for left-censoring with censored region $(-\infty,c)$, the imputed value $\hat{y}$ should be no greater than $c$.)
    - Develop pooling strength type imputation method. This is applicable to cases when we observe multiple related time series (e.g. sales of the same product at multiple stores, or sales of similar products at the same store), some of which is censored. The aim is to develop some form of nearest neighbour or regression/correlation model to impute the data. This section can borrow strongly from the missing data imputation literature.
    - Compare these imputation methods with existing ones using both simulated data and real data.
5. Subject to progress of previous steps, develop software package to implement the developed methods.
    - This package should have a strong view of enabling connection with other time series modeling pipeline.

## Bibliography
Park, J., Genton, M., & Ghosh, S. (2007). Censored time series analysis with autoregressive moving average models. *Canadian Journal Of Statistics*, 35(1), 151-168. doi: 10.1002/cjs.5550350113

Schumacher, F., Lachos, V., & Dey, D. (2017). Censored regression models with autoregressive errors: A likelihood-based perspective. *Canadian Journal Of Statistics*, 45(4), 375-392. doi: 10.1002/cjs.11338

Wang, C., & Chan, K. (2017). Quasi-Likelihood Estimation of a Censored Autoregressive Model With Exogenous Variables. *Journal Of The American Statistical Association*, 1-11. doi: 10.1080/01621459.2017.1307115
