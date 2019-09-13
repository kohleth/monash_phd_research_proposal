# Monash PhD Research Proposal

# Improving Intermittent Time Series Forecast by Pooling Information Across Groups

Forecasting intermittent time series has been challenging. Classical methods such as exponential smoothing or arima, and even dedicated models such as Croston's method (1972) often struggle to produce forecast that are  better than naive methods (e.g. using historical means, or random walk model). However, we observe that intermittent time series often (though not always) arise as part of a larger group of time series. For example, a retailer might wish to forecast the demand of each of their product at each of their store locations. Unless the product is a fast-moving item (e.g. milk), these series will typically be intermittent, but there will be many of them. Furthermore, while these base series often appear erratic, their aggregate (e.g. the demand of a product at the national level) often display modellable signal. We believe we can exploit these kind of relationship (i.e. pool information) to create better forecast.

## Current state of research

Here we summarize the relevant researches to date.

#### Croston type method
Croston et. al. (1972) proposed an innovative method to forecast intermittent time series. Specifically, the raw series is decomposed into two time series: that of the event (non-zero observation), and the time between successive events. Each of these series are non-intermittent, thus allowing classical forecast methods to be applied. After this, all that is left is to recombine the forecasts from both time series to produce the actual forecast. Since Croston's, variants have been proposed which correct the bias in the original method (Syntetos & Boylan (2001); Kostenkov & Hyndman (2005)). These methods do not provide interval estimate (because there is no distributional assumption) but they are nonetheless useful in practice.

#### INGARCH
Regression approach to this problem is best exemplified by the INGARCH type model (Ferland et. al. 2006). Noting that most often intermittent series are time series of count, one can use generalized linear model (possison or negative binomial family) to model such data. The glm framework opens the door for regression against covariates, although in the time series setting these covariates are typically lagged version of the observation or parameter, and ARMA type error.

#### Hierarchical times series
While Croston's and INGARCH type model attempt to extract as much information from the time series itself, hierarchical time series seek to borrow strength from neighbouring series. Hyndman et. al. (2011) proposed regression based method to reconcile the forecasts in the hierarchy, and these methods in theory also improve the forecast. Athanasopolous et. al. (2016) applied the same method but to temporal hierarchy (aggregate) instead and also reported gain in forecast accuracy. 

An alternative method of exploting the temporal aggregate for forecasting, especially with intermittent series, is proposed by Petropoulos & Kourentzes (2015), where they use exponential smoothing to forecast at the aggregated level, before decomposing it back to the base level.

#### Gaussian-Cox process
A Cox process is essentially a (inhomogenous) poisson process where the intensity (rate parameter) is itself a stochastic process. A Gaussian-Cox process is a poisson process where the stochastic intensity parameter  is a gaussian process. Although more commonly used in spatial statistics (specifically point process model), it can be adopted to intermittent times series forecast as well. In particular, the Cox (poisson) component can be used to model the time series of count, with the intensity parameter modelled at the aggregated level using classical time series methods (which naturally produces a gaussian process). While these methods often result in in-tractable likelihood models (Adams et. al. 2009), advances in simulation base method (Teng et. al. 2017) can alleviate the problem at the expense of a heavier computation burden.

#### Forecastability
There has been limited research to date on this topic -- at what point (what level of intermittent-ness) do we conclude that  there is just no (non-trivial) way of forecasting the series? 

In the context of choosing between Croston's method and its variant, Syntetos & Boylan (2001), and Kostenkov & Hyndman (2005) came up with guidelines based on the coefficient of variation of the non-zero part of the seires, as well as its inter-event interval. In the field of supply chain management, there is also XYZ analysis which attempts to quantify the forecastability of a times series by its coefficient of variation. With the exception of Petropoulos & Kourentzes (2015), all these attempts only look at the forecastability of the series by itself, instead of in the context of a group of series with shareable information. Therefore, it calls for a deeper investigation.


## Research plan
We believe we can contribute to the body of knkowledge in two areas:

#### Forecastability
Essentially, we seek to find ways to determinte whether a time series is forecastable (by non-trivial method). Such rules would be useful in situation where a large number of time series needs to be modelled, since they can be used as a screening test to save model time. To achieve this we need to expand existing research beyond just looking at Croston type method to also include hierarchical forecasting methods as well. We will also need to seek better quantification of intermittent-ness and forecastability beyond simply the coefficient of variation, and the inter-event time interval.

If successful, we can also use this to determine the regions / space of time series where, by themselves they are not forecastable, but when information are pooled they become admissable. And all following research will concentrate in this space.

#### Improving intermittent time series forecast by pooling information across groups
We can think of three broad ways of tackling this problem. The first two will probably fail but since this is research we should try all and fail fast.

1. Hierarchical croston -- can we pool information in the non-zero demand series and the inter-event time interval series? Natually if we can see the aggregated series peaking, we should expect a higher non-zero demand, and shorter inter-event time interval at the base level.

2. Integer-programming -- instead of using regression to pool information, we use integer programming with a suitable cost function. This will at least respect the count nature of the problem, but it is unclear whether it will actually help the forecast. This is an extension of the GTOP method of van Erven & Cugliari (2015) with the addition of an integer constraint.

3. Gaussian-Cox process -- we see this as an amalgamation of hierarchical forecasting and INGARCH. Reiterating the paragraph above, suppose we can produce reasonably accurate forecast at the aggregate level using classical methods (producing a gaussian estimate), this can feed into the intensity parameter of the possion distribution at the base level. The difficulty of doing this is in balancing the contribution of this information with that from the series itself (i.e. the lagged covariates). This might mean we need to introduce additional weight parameter. The computation aspect can be handle by bayesian computation.

We shall point out that in this research our primary concern is in improving forecast accuracy, not reconciliating forecast. Therefore, we will venture beyond the space of aggregate-consistent forecast methods. And since we are not reconciliating forecast, we do not necessarily need forecasts from all aggregation level in the model. This opens up a further research question -- can we only consider a subset of all aggregation level when pooling information? If so, how can we determine these levels?