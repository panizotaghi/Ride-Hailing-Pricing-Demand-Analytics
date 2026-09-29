# Project Report: English Summary

This is a condensed English translation of the original Persian report, [`Project_Report_FA.pdf`](Project_Report_FA.pdf). It follows the report's structure and does not add new content. The figures linked below are the re-executed versions saved in each part's `figures/` folder.

- **Course:** Transportation Planning, Logistics and Supply Chain
- **Instructor:** Dr. Sadeghi
- **University:** Amirkabir University of Technology (Tehran Polytechnic)
- **Author:** Paniz Otaghi (team project of two students)
- **Term:** Autumn 1404 (Iranian calendar; autumn 2025)

---

## Introduction

### Project description

Assume you are studying an online taxi (ride-hailing) market and want to check how your pricing system has performed. For this purpose, a real dataset from an online taxi app is provided. It contains trip requests from passengers and has 11 columns.

### Data description

1. `price_check_time`: time at which the passenger asked for a price quote
2. `passenger`: passenger ID
3. `origin`: ID of the trip's origin zone
4. `destination`: ID of the trip's destination zone
5. `price`: offered trip price (before discount)
6. `subsidy`: discount amount
7. `distance`: trip distance (metres)
8. `expected_duration`: estimated trip duration (minutes)
9. `req_time`: time at which the trip request was sent
10. `driver`: ID of the driver who accepted the trip
11. `status`: status of the trip request
    - 1: trip completed
    - 2: trip cancelled by the passenger
    - 3: trip cancelled by the driver
    - 4: no driver accepted the trip
    - blank: no request was made. The passenger asked for a price but did not send a trip request.

---

## Part 1: Data analysis

The data were first reviewed and preprocessed: invalid records were removed, missing values were handled, and outliers were removed with the IQR method. Then the requested analyses were carried out: OD matrices, unmet demand, time-of-day analysis and cancellation analysis.

### 1. OD matrix analysis

The goal is to extract the pattern of demand between zones. The demand matrix counts all requests. The completed-trips matrix includes only status = 1. Comparing the two shows which OD pairs have both high demand and a high completion rate.

- The demand matrix is shown as a heatmap. It counts requests between each origin-destination pair, including trips that did not happen.
- Demand is not evenly distributed. Most trips involve certain zones. The largest flow is from origin 2 to destination 0.
- Some cells (shown in purple) have no demand at all. This indicates that the city has main travel hubs, such as dense commercial, office or residential areas. The transport system matters most in these areas and is under more pressure there, so they need more careful management and planning.
- Frequent OD pairs are concentrated in the block of zones 0 to 2. A subset of requests flows between zones 3 and 4.
- The drop in cell values from the demand matrix to the completed-trips matrix shows the share of cancelled or unfulfilled demand for each OD pair.
- Comparing the two matrices shows that on some routes demand did not turn into completed trips. This gap shows a weakness of the system in responding to demand. These areas can be critical points or bottlenecks.

Figures: [OD demand matrix](../Part_1_Demand_Supply_and_Pricing/figures/od_demand_matrix.png), [OD completed trips matrix](../Part_1_Demand_Supply_and_Pricing/figures/od_completed_trips_matrix.png)

### 2. Unmet demand

**What share of requests had no driver (status = 4), and how does this change over the day?**

Unmet demand rises at certain hours of the day, usually together with periods of high demand. At these times requests arrive faster than drivers can serve them. The main cause is an imbalance between demand and supply at that time. For example, the chart shows that at 05:00 and 02:00 the share of unmet demand is much higher than at other hours.

Figure: [Unmet demand rate by hour](../Part_1_Demand_Supply_and_Pricing/figures/unmet_demand_by_hour.png)

**Which OD pairs have the largest share of demand?**

Some routes take a larger share of total demand. These are usually the routes that are darker in the OD matrix. Concentrated demand can help, because good planning can raise efficiency and profitability. If it is not managed well, it can cause passenger dissatisfaction. The route from origin 2 to destination 0 has the highest demand and importance.

| Origin | Destination | Demand share (%) |
|---|---|---|
| 2 | 0 | 16.91 |
| 1 | 1 | 15.06 |
| 1 | 0 | 14.13 |
| 2 | 1 | 13.00 |
| 3 | 3 | 12.13 |

**For which OD pairs does price per km fluctuate the most?**

Some routes show much more price-per-km fluctuation than others. The report attributes this to strong traffic changes, different times of day, or pricing that varies with time and traffic. Such fluctuation is unpleasant for passengers and drivers and can raise cancellation or non-acceptance rates.

| Origin | Destination | Mean | Std | Count |
|---|---|---|---|---|
| 0 | 2 | 18,333.78 | 6,864.72 | 303 |
| 4 | 4 | 22,161.37 | 6,603.01 | 119 |
| 3 | 3 | 16,800.77 | 6,015.27 | 1,695 |
| 1 | 0 | 22,628.19 | 5,647.24 | 1,974 |
| 2 | 1 | 18,484.58 | 5,616.04 | 1,817 |
| 0 | 0 | 17,271.73 | 5,579.44 | 795 |
| 1 | 2 | 16,870.63 | 5,335.91 | 870 |
| 1 | 1 | 17,623.60 | 4,750.15 | 2,105 |
| 4 | 3 | 14,872.46 | 4,656.59 | 317 |
| 3 | 4 | 13,695.86 | 4,524.45 | 413 |

**In peak hours (06:00-08:00 and 16:00-19:00), what is the average discount-to-price ratio, compared with off-peak hours?**

The report shows 15.68% for peak hours and 15.05% for off-peak hours. It states that the ratio in peak hours differs meaningfully from off-peak hours. It explains that in peak hours discounts are usually either reduced to avoid overcrowding, or applied in a targeted way to steer demand to certain routes or times. It concludes that pricing policies play an important role in shaping user behaviour.

### 3. Time-of-day analysis

**Distribution of requests over the hours of the day. When are requests highest and lowest?**

Requests are lower in the early morning and late at night. The largest volumes occur during daily commuting hours. This pattern follows the city's work and social activity and confirms that trip demand depends strongly on time. The maximum is at 18:00 with 1,174 requests (when offices close). The minimum is at 05:00 with 31 requests.

Figure: [Requests by hour](../Part_1_Demand_Supply_and_Pricing/figures/requests_by_hour.png)

**Price level and volatility across the day**

The mean and standard deviation of price after discount were computed for each hour and plotted as mean +/- std. The mean price changes across the day, and in some hours the spread is higher. The report links this to dynamic pricing and to changes in demand during the day. It states that prices are higher and more volatile in peak hours, to manage demand and encourage drivers to work in busy periods, and that prices are more stable in low-demand hours. It also notes that, according to the chart, the price per trip in the early hours of the day is much higher than at midday, although demand at those hours is low. Trips at those times carry a higher price so that drivers are willing to accept them. After 05:00 the price per trip drops suddenly while demand rises, which gives drivers a different kind of incentive.

Figure: [Price level and volatility by hour](../Part_1_Demand_Supply_and_Pricing/figures/price_by_hour.png)

### 4. Cancellation analysis

**Effect of distance, price and expected duration on cancellation by the passenger (without a 3D chart)**

Each factor was divided into 10 deciles, and the cancellation rate was computed in each decile. All three curves are drawn in one chart, one chart per side (passenger and driver). Decile 1 is the lowest value of the factor and decile 10 the highest. The report states that cancellation probability rises as each of these variables increases. Long, expensive trips with long expected durations have the highest cancellation rates. This shows that passengers are sensitive to cost and time.

Figure: [Passenger cancellation by decile](../Part_1_Demand_Supply_and_Pricing/figures/passenger_cancellation_deciles.png)

**The same analysis from the driver side**

According to the report, drivers prefer shorter trips, better prices and shorter expected durations. Trips that do not give drivers a good economic or time return are cancelled more often by drivers. Distance plays the strongest role in the driver's decision. Drivers are less willing to accept very short or very long trips, especially when the price per km is not attractive. Longer expected durations, especially in heavy traffic, also raise the chance of driver cancellation. Drivers care most about time efficiency and relative income.

Figure: [Driver cancellation by decile](../Part_1_Demand_Supply_and_Pricing/figures/driver_cancellation_deciles.png)

Comparing the two charts, passenger and driver cancellations follow different logic. Passengers try to reduce cost and time. Drivers focus on profitability and managing their time. If pricing and trip-allocation algorithms ignore this difference, dissatisfaction on both sides can grow and overall efficiency can fall. How to read the charts: a rising curve means that higher values of that factor go with more cancellations; a falling curve means fewer cancellations; a flat curve means the effect is weak or non-linear in this data.

---

## Part 2: Passenger and driver behaviour analysis

The questions were answered with the FITTER library and `scipy.stats` (the notebook uses `scipy.stats`). Fisher's exact test and t-tests were used at the 95% confidence level.

### Preprocessing and distribution fitting

Only real trip requests (non-empty `req_time`) were kept. Invalid records (invalid or negative price, distance or expected duration) were removed. Outliers in the key variables were removed with the IQR method. The result is the cleaned dataset used for all statistical analyses in this part.

Normal, lognormal, gamma and exponential distributions were fitted to the continuous variables and compared with AIC:

- For price after discount, the gamma distribution has the lowest AIC.
- For trip distance (km), the gamma distribution also fits best.

Both variables are right-skewed, and the normality assumption is not suitable. This justifies more robust tests such as Welch's t-test.

### Q1. Did the offered price have a significant effect on cancellation by the passenger?

- H0: the mean offered price is the same for trips cancelled by the passenger and trips not cancelled.
- H1: the two means differ.
- Welch's t-test: p-value about 0.93. Mean price, cancelled by passenger: about 50,431. Not cancelled: about 50,457.
- Since p is much larger than 0.05, H0 is not rejected. In this data the offered price had no significant effect on passenger cancellation.

### Q2. Did trip distance have a significant effect on cancellation by the driver?

- H0: the mean trip distance is the same for driver-cancelled trips and other trips.
- H1: the two means differ.
- Welch's t-test: p-value about 0.72. Mean distance, driver-cancelled: about 3.21 km. Not driver-cancelled: about 3.19 km.
- H0 is not rejected. In this data trip distance had no significant effect on driver cancellation.

### Q3. Are discounted requests (subsidy > 0) more likely to be accepted by the passenger than requests without a discount?

Passenger acceptance is defined as sending a real request after seeing the price (a non-empty `req_time` after `price_check_time`).

- H0: a discount has no effect on the probability that the passenger accepts.
- H1: a discount changes the probability of acceptance.
- Fisher's exact test on a 2x2 table: p-value about 0.84, odds ratio about 0.99. Acceptance rate with discount about 62.5%, without discount about 62.7%.
- H0 is not rejected. In this data, discounts did not significantly raise the probability of passenger acceptance. Acceptance rates with and without discount are almost the same.

### Conclusions of Part 2

- The offered trip price had no significant effect on passenger cancellation.
- Trip distance had no significant effect on driver cancellation.
- Discounts (subsidy > 0) did not significantly raise the probability of passenger acceptance.

Cancellation and acceptance decisions in this data are probably influenced by factors other than price, distance and discount.

---

## Part 3: Behaviour prediction

First, a linear regression model predicts the trip price. Then a logistic regression predicts whether a driver is found for a trip request (status != 4). Preprocessing (validity filters, IQR outlier removal and feature engineering) follows the earlier parts.

### 1. Linear regression for trip price

Model: `LinearRegression` in a pipeline with `StandardScaler` for numeric features and `OneHotEncoder` for categorical features.

- Numeric inputs: hour, distance_km, expected_duration, subsidy
- Categorical inputs: origin, destination

**1.1 Evaluation metrics:** MAE = 6,824, RMSE = 8,835, MAPE = 11.54%, R² = 0.594.
R² of about 0.59 means the model explains about 59% of the variation in price. The model is moderate to good, but part of the price variation cannot be explained by the available features. The other metrics show that the model has some error.

**1.2 One real record vs. prediction** (test-set record, index 17239): real price 50,000, predicted 59,891, absolute error 9,891, percentage error 19.8%. The error is larger than the MAE, so it is less acceptable than the model's average error.

**1.3 Most important features (coefficients).** By absolute coefficient size, the most important factors are distance and expected duration, followed by the discount (subsidy).

Figure: [Largest coefficients](../Part_3_Price_and_Driver_Match_Prediction/figures/price_model_coefficients.png)

**1.4 Model performance on long trips (residual vs. distance).** Residuals were plotted against distance to check whether the model behaves differently on long trips. If the error spread grows at high distances, or the mean error in distance bins drifts positive or negative, the model is less reliable or biased on longer trips. The report finds that the error spread grows with distance, but the mean error stays around zero and there is no systematic over- or under-estimation on long trips. The model therefore has no significant bias for long distances, although prediction uncertainty is higher there. The report shows these plots computed with two different methods.

Figures: [Residual vs. distance](../Part_3_Price_and_Driver_Match_Prediction/figures/residuals_vs_distance.png), [Mean residual by distance bin](../Part_3_Price_and_Driver_Match_Prediction/figures/mean_residual_by_distance_bin.png)

### 2. Logistic regression for driver acceptance

Target: `accepted_by_driver = 1` if a driver was found (status != 4), and 0 otherwise.

- Numeric inputs: hour, distance_km, subsidy (with `StandardScaler`)
- Categorical inputs: origin, destination (with `OneHotEncoder`)
- `class_weight='balanced'` was used to reduce the effect of class imbalance.

**2.1 Evaluation metrics (as reported):** Accuracy = 0.571, Precision = 0.980, Recall = 0.574, F1 = 0.724, ROC-AUC = 0.511.
A ROC-AUC close to 0.5 shows that the model's power to separate the classes is low (close to random). High precision means that when the model predicts "driver found" it is usually right. The moderate recall shows that it misses many "driver found" cases.

Figure: [Confusion matrix](../Part_3_Price_and_Driver_Match_Prediction/figures/driver_match_confusion_matrix.png)

**2.2 Coefficients: effect of discount and distance.** In logistic regression, the sign of a coefficient shows the direction of the effect (after standardisation). The odds ratio, exp(coef), gives a more direct reading of the change in odds.

- subsidy: coef = 0.016, OR = 1.016, which raises the probability of finding a driver.
- distance_km: coef = -0.029, OR = 0.972, which lowers the probability of finding a driver.

Conclusion: according to this model, the discount has a positive (though small) effect and longer distance has a negative effect. Longer trips usually find a driver less easily.

> Note: running the notebook in this repository from top to bottom gives Accuracy = 0.575, Precision = 0.981, Recall = 0.578, F1 = 0.727, ROC-AUC = 0.509, subsidy coef = 0.022 (OR = 1.023) and distance_km coef = -0.035 (OR = 0.965). The direction of the effects is the same as in the report.
