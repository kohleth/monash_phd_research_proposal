# Monash PhD Research Proposal

# Business Forecasting with Censored Data

Most time series forecasting methods assume the observed data is uncensored. However, censored data can arise in many real life processes. For example, a business might wish to forecast the sales of a certain (physical) product using the sales history. However, if the product was out of stock for period of time, the sales history would have been censored. In this example, proceeding with the usual forecasting method will likely lead to an under-prediction of the true future sales.

To date, there has been limited research on this topic. Park et. al. (2007) proposed an iterative scheme where the censored portion is imputed, and then parameters estimated using standard methods, iteratively and repeatedly until convergence. Wang and Chan (2017) proposed an EM-type algorithm (they called it quasi-likelihood) to estimate the parameters of autoregressive models with regressors, without any imputation. Schumacher et. al. (2016) proposed a stochastic EM algorithm, where the expectation is approximated stochastically using simulated data.

While the advancement is significant, these research typically do not present a strong business focus. In fact, many examples are drawn from environmental modelling, where the process causing the censorship (e.g. machine detection limit) could be very different to that in the business context (product out of stock).

With this in mind, the aim of this project is to,
- evaluate the current state of research,
- identify any area of potential advancement in forecasting methodology, and make contribution that is both academically sound and business-applicable,
- subject to research progress, develop software packages to share the result with the wider data analytics communities.


## Bibliography
Park, J., Genton, M., & Ghosh, S. (2007). Censored time series analysis with autoregressive moving average models. *Canadian Journal Of Statistics*, 35(1), 151-168. doi: 10.1002/cjs.5550350113

Schumacher, F., Lachos, V., & Dey, D. (2017). Censored regression models with autoregressive errors: A likelihood-based perspective. *Canadian Journal Of Statistics*, 45(4), 375-392. doi: 10.1002/cjs.11338

Wang, C., & Chan, K. (2017). Quasi-Likelihood Estimation of a Censored Autoregressive Model With Exogenous Variables. *Journal Of The American Statistical Association*, 1-11. doi: 10.1080/01621459.2017.1307115
