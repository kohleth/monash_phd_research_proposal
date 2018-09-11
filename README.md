# Monash PhD Research Proposal

# Censored Time Series Forecasting using State-Space Models

Most time series forecasting methods assume the observed data is uncensored. However, censored data can arise in many real life processes. For example, a business might wish to forecast the sales of a certain (physical) product using the sales history. However, if the product was out of stock for period of time, the sales history would have been capped (censored). In this example, proceeding with the usual forecasting method will likely lead to an under-prediction of the true future sales. This calls for method to model the time series which account for data censorship.

## Current state of research

#### Park et. al. (2007)
Park et. al. (2007) proposed an iterative scheme. First, the time series data is assumed to follow a multivariate gaussian distribution. Therefore, the censored portion of the time series could be imputed using the appropriate conditional distribution. Next, the parameters of the distribution are re-estimated. This process is repeated until convergence is reached.

#### Wang and Chan (2017)
Wang and Chan (2017) studied AR models with exogenous variables and censoring. They assumed the score of the most recent uncensored data block, and its expectation givens the censored data has a closed form expression. Then, exploiting the fact that scores have zero mean, they form a set of estimating equations for the model parameters.

#### Schumacher et. al. (2017)
Schumacher et. al. (2017) considered the same AR models with exogenous regressors and censoring. They used the EM algorithm to maximize the complete likelihood of both the observed data and the censoring indicator. To circumvent the difficulty in calculating the expectation, they approximate it using simulated data.

## Research Gap
As can be seen, current research mostly tackle the problem of parameter estimation in the presence of censored data, and they typically work directly with the ARIMA(X) model and pay little attention to the censoring mechanism.

We wish to approach this problem with three distinctive differences.

#### 1. Forecast first
In many situations, it is the forecast, instead of model parameters, that is of primary interest. Therefore, we think we should shift the effort from adjusting parameter estimates to adjusting forecast (or the time series) directly.

#### 2. Imputation methods
In addition to developing a full solution which forecasts in the presence of data censorship, it can be useful to develop an imputation only method. That is, a model that simply adjust the past censored data, so that the data analyst / statistician can proceed the modelling with the standard uncensored approach.

#### 3. State-Space Models
Data censorship can be modeled quite naturally if we adopt a state-space approach. In addition, both the imputation and forecast can be obtained quite naturally using sequential filtering, forecasting, and smoothing techniques that are well developed in the state-space literature.

## Proposed Models
Let $\theta$, $X_t$, and $Z_t$ be the model parameters, process value, and observed value (i.e. data) at time $t$ respectively. Further denote $\Omega_t$ to be the history of observation until time $t$, that is $\Omega_t=\{Z_i\}_{i=1\ldots t}$.

If we assume a full Bayesian approach, we can assign distributional assumption to all components. Then the full model of everything, given what we have observed is
\[
  [Z_t, X_t, \theta | \Omega_{t-1}]
\]
where $[.]$ denotes a distribution.

This full distribution can be broken down into
\[
  [Z_t, X_t, \theta | \Omega_{t-1}]
  =[Z_t|X_t, \theta, \Omega_{t-1}]
  [X_t|\theta, \Omega_{t-1}]
  [\theta|\Omega_{t-1}]
\]

where the 3 components on the RHS of the equations are the *data model*, *process model*, and *parameter model* respectively. We largely borrow this framework from Cressie & Wikle (2011).

#### Parameter model
If we don't want a fully Bayesian approach, we can simply replace the parameter model with some form of a plug-in estimate of $\theta$. Otherwise, it can come from some standard prior distribution.

#### Process model
This can be the standard uncensored model such as ARIMAX.

#### Data model
This is where we model the censorship. But the formulation is highly problem-specific. In the case of inventory/ demand / sales forecasting, which is what we are interested in, $X_t$ can be the real demand, and $Z_t$ be the actual sales. Formulated this way, the Data model could simply be
\[
  [Z_t|X_t, \theta, \Omega_{t-1}]=\min(X_t, C)
\]
where $C\in \theta$ (the right censoring limit) is some constant physical constraint such as shelf space limit.

We could extend this model further, for example, instead of a constant $C$, we might have $C_t$ which represents the available stock at time $t$. And then
\[
  C_t=C_0-Z_1-Z_2-\ldots-Z_{t-1}
\]
where $C_0$ denotes some initial stock level.

We could further introduce other complication such as random stock loss due to theft or write-off:
\[
  [Z_t|X_t, \theta, \Omega_{t-1}]=\min(X_t, \phi C)
\]

Or perhaps the case when not all demand is fulfilled,
\[
  [Z_t|X_t, \theta, \Omega_{t-1}]=\min(\psi X_t, \phi C)
\]

where $\phi,\psi \in \theta$ and $\phi, \psi \in [0,1]$.

This is just a sample of possible extension. The potential is great.

### Forecasting (and Filtering)
With the model formulated, we can use the typical technique adopted in the state-space models, which is to do the forecast sequentially.

For example, for notational simplicity, suppose the process model is AR(1).

Then the Forecasting distribution becomes
\[
  [Y_t|\Omega_{t-1}]=\int{[Y_t|Y_{t-1}][Y_{t-1}|\Omega_{t-1}]}dY_{t-1}
\]
The first component in the integral is the AR(1) distribution, whereas the second component is the Filtering distribution, where
\[
  [Y_{t}|\Omega_t] \propto [Z_t|Y_t][Y_t|\Omega_{t-1}]
\]
because of Bayes' Theorem.

This means, assuming we have some starting value $[Z_0, Y_0]$, we can sequentially compute the Filtering distribution and thus the Forecasting distribution.

### Imputation (Smoothing)
The imputation can likewise be done sequentially. The imputation step is just the smoothing step, and its distribution at time $t\leq T$ is
\[
  [Y_t|\Omega_T]=\int{[Y_t|Y_{t+1},\Omega_T][Y_{t+1}|\Omega_T]}dY_{t+1}
\]
where
\[
  [Y_t|Y_{t+1},\Omega_T]=[Y_t|Y_{t+1},\Omega_t]
  \propto [Y_{t+1}|Y_t][Y_t|\Omega_t]
\]
where the right-most relationship holds because again of Bayes' Theorem.

This means, again, starting from the end of time $T$, we can sequentially obtain the Imputation (Smoothing) distribution by doing filtering and applying the process model.

### Computation
Since our Data model is non-linear, it is likely that we will have to go down the simulation path. We could use Approximate Bayesian Computation to facilitate this step.


## Research Plan
1. Review literature on
    - Time series forecasting
    - Censored / Missing data modeling
    - State-Space models
    - Approximate Bayesian Computation
2. Develop computation algorithms with basic parameter, process, and data models.
3. Examine model assessment techniques under data censorship.
4. Further develop / extend models to cater for more realistic settings.
5. Subject to progress of previous steps, develop software package to implement the developed methods.

## Bibliography
Cressie, N. and Wikle, C. (2011). *Statistics for Spatio-Temporal Data*. Hoboken, N.J.: Wiley.

Park, J., Genton, M., & Ghosh, S. (2007). Censored time series analysis with autoregressive moving average models. *Canadian Journal Of Statistics*, 35(1), 151-168. doi: 10.1002/cjs.5550350113

Schumacher, F., Lachos, V., & Dey, D. (2017). Censored regression models with autoregressive errors: A likelihood-based perspective. *Canadian Journal Of Statistics*, 45(4), 375-392. doi: 10.1002/cjs.11338

Wang, C., & Chan, K. (2017). Quasi-Likelihood Estimation of a Censored Autoregressive Model With Exogenous Variables. *Journal Of The American Statistical Association*, 1-11. doi: 10.1080/01621459.2017.1307115
