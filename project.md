## Variabiltty in PV Power Generation from Weather Conditions


## Introduction 

Solar photovoltaic power is one of the fastest-growing sources of renewable energy, but its output varies significantly with atmospheric conditions. This variability creates challenges for grid reliability and energy planning, since power generation depends strongly on weather and solar availability.

In this project, I analyze real world data from a utility-scale solar power plant and apply supervised machine learning techniques to study how environmental conditions influence AC power output. The dataset comes from the Solar Power Generation Data collection on Kaggle and includes inverter power measurements along with weather sensor data from a solar plant in India. The data contains solar irradiance, ambient temperature, and module temperature.

Solar irradiance plays a dominant role in determining power output, but the relationship between sunlight and AC power is not instantaneous. In practice, power output often increases gradually, reaches a maximum, and then flattens. This relationship raises important questions about system response time, efficiency limits, and the physical factors.

To investigate these effects, I use a regression-based modeling approach to analyze how changes in irradiance and temperature relate to changes in AC power output over time. I focuse on understanding why solar power generation appears to lag behind incoming sunlight and how simple machine learning models can capture this behavior. 

## Data


*Figure 1: AC Power versus Time graph.

![Figure 1: AC Power versus Time](AC%20Power%20vs%20Time%20Final.png)


*Figure 2: Irradiance and AC Power versus Time

![Figure 2: Irradiance and AC Power versus Time](Irradiance%20and%20AC%20Power%20vs%20Time.png)



*Figure 3: Lag Optimizer

![Figure 3: Lag Optimizer](Lag%20Optimizer.png)


## Modelling

I used a simple machine learning approach to model the relationship between solar irradiance and AC power output. I chose this method because the dataset shows a clear connection between sunlight and power generation, but the response does not happen instantly. The data is time-based, so the timing between input and output matters. I account for this, I figured there must be a time lag between irradiance and AC power. For each possible lag, I shifted the irradiance data forward in time and used it to predict AC power output with a linear regression model. I measured how well the model performed using mean squared error, which shows how close the predictions were to the actual power values. I repeated this process for a range of lag values to find which one produced the lowest error. This approach allows the data to determine how long the system takes to respond to changes in sunlight instead of assuming a fixed delay ahead of time. 


I cleaned the dataset by converting columns to numeric values, removing missing data,
and filtering to daytime hours where irradiance is nonzero.

## Data cleaning and daytime filtering
![Code Snippet 1](Code%20Snippet%201.png)

I then introduced a time-lag parameter and evaluated model performance using
mean squared error across different lag values.

## Lag-based regression and MSE calculation
![Code Snippet 2](Code%20Snippet%202.png)


Overall, I chose this method because it provides an interpretable way to study the timing relationship between irradiance and power output. It captures the main behavior of the photovoltaic system while remaining easy to understand and directly connected to the physical processes involved.


## Results

Figure 1 shows the AC power output of the photovoltaic system as a function of time over two diurnal cycles. The power output exhibits a clear daily pattern and increases during daylight hours. It reaches a maximum near midday, and decreases toward zero overnight. The gradual ramp-up and ramp-down indicate that power output does not respond instantaneously to changing solar conditions.

Figure 2 shows solar irradiance and AC power plotted simultaneously as a function of time. Irradiance displays high short-term variability with frequent spikes and drops because of an error with the data collection. The sensor measures the irradiance at slightly different intervals compared to the AC power data collection. Peaks in AC power generally follow peaks in irradiance. This suggests a delayed system response rather than a direct instantaneous relationship.

Figure 3 shows the mean squared error (MSE) of a linear regression model predicting AC power from irradiance as a function of imposed time lag. The regression error is minimized at a lag of approximately 30 minutes, indicating that irradiance measurements lead AC power output by this time interval. Errors also increase for both the shorter and longer lags.


## Discussion
The results demonstrate that AC power output is not a direct function of instantaneous irradiance but instead reflects a delayed and smoothed system response. The irradiance compared to the AC power output suggests the influence of physical and operational factors such as panel thermal inertia, inverter clipping, and maximum power input issues.

The optimal lag of approximately 30 minutes represents the characteristic response time of the photovoltaic system. This finding supports the hypothesis that photovoltaic systems exhibit time-dependent behavior that must be accounted for in predictive models.


## Conclusion

The results show that AC power does not respond immediately to changes in irradiance. Instead, the system takes time to translate incoming sunlight into stable power output.

Instead of assuming that power output responds instantly to sunlight, I treated the system as something that evolves over time. The results clearly show that AC power output lags behind changes in irradiance and follows a smoother, delayed pattern.

By testing different time offsets between irradiance and AC power, I found that a lag of roughly 30 minutes produces the most accurate predictions. In practical terms, this means that the system takes about half an hour to fully respond to changes in incoming sunlight. When irradiance increases, the AC power output continues rising even after the irradiance has already peaked, and when irradiance drops, power output declines more gradually. This behavior reflects real physical and operational processes, including panel thermal inertia, inverter response, and the way maximum power point tracking smooths short-term fluctuations.

This result helps explain why irradiance appears noisy and highly variable while AC power output looks much smoother in time. The photovoltaic system effectively filters rapid changes in solar input, which makes instantaneous irradiance a poor predictor of power output on its own. Incorporating a time delay captures this filtering behavior and leads to a more realistic model of how the system actually operates.

Overall, this work shows that even a simple machine learning approach can reveal meaningful physical behavior when applied carefully. Identifying the system’s response time improves predictive accuracy and provides insight into how solar power systems behave under changing environmental conditions. In future work, this approach could be extended by including additional variables such as temperature, wind speed, or historical power output, or by using more advanced models to improve short-term solar forecasting and support battery charging and energy management decisions.


## References
[1] Kaggle Solar Plant Generation in India
