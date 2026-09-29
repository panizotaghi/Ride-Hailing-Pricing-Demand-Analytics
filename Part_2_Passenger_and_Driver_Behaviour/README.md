# Part 2: Passenger and Driver Behaviour

Notebook: [`part2_passenger_and_driver_behaviour.ipynb`](part2_passenger_and_driver_behaviour.ipynb) (English) | [`part2_passenger_and_driver_behaviour_FA.ipynb`](part2_passenger_and_driver_behaviour_FA.ipynb) (original, Persian)

## Business questions

1. Do passengers cancel more when the offered price is higher?
2. Do drivers cancel more when the trip is longer?
3. Do discounts make passengers more likely to book after they see the price?

All tests use a 5% significance level.

## Approach

- Same cleaning as Part 1: real ride requests only, invalid records removed, IQR outlier removal. 13,974 requests remain.
- **Distribution fitting.** Normal, lognormal, gamma and exponential distributions were fitted to price after discount and to trip distance with `scipy.stats`, and compared with AIC (lower is better).
- **Q1 and Q2: Welch's t-test.** It compares two group means without assuming equal variances. It was chosen because both variables are right-skewed.
- **Q3: Fisher's exact test** on a 2x2 table (discount yes/no vs. booked yes/no). Here "booked" means that a ride request was sent after the price check. This works as a price-quote-to-request conversion rate. For this question all price checks are used (not only requests), with the validity filters and IQR outlier removal on price, distance and expected duration.

## Results

### Distribution fitting (AIC)

| Distribution | AIC, price after discount | AIC, distance (km) |
|---|---|---|
| Gamma | **302,725** | **46,406** |
| Lognormal | 302,765 | 46,632 |
| Normal | 303,815 | 48,111 |
| Exponential | 311,473 | 53,659 |

The gamma distribution fits both variables best. Both are right-skewed, so a test that does not rely on equal variances (Welch) was used.

### Hypothesis tests

| Question | Groups compared | Result | p-value | Decision |
|---|---|---|---|---|
| Q1. Price and passenger cancellation | Mean price after discount: cancelled by passenger 50,431 vs. all other requests 50,457 | Almost identical | 0.93 | No significant effect |
| Q2. Distance and driver cancellation | Mean distance: cancelled by driver 3.21 km vs. completed or passenger-cancelled 3.19 km | Almost identical | 0.72 | No significant effect |
| Q3. Discount and booking (conversion) | Booking rate: with discount 62.5% vs. without discount 62.7% (odds ratio 0.99) | Almost identical | 0.84 | No significant effect |

Contingency table for Q3:

| | Booked | Did not book |
|---|---|---|
| Discount (subsidy > 0) | 13,558 | 8,119 |
| No discount | 1,911 | 1,135 |

## Takeaways for decision makers

- **Price level did not explain passenger cancellations.** Cancelled and non-cancelled requests had almost the same average price.
- **Trip length did not explain driver cancellations.** Driver-cancelled trips were only 0.02 km longer on average.
- **Discounts did not change conversion.** About 63% of price quotes turned into ride requests, with or without a discount. Most quotes (21,677 of 24,723, about 88%) carried a discount, so discount spending was widespread but was not linked to more bookings in this data.
- **Look for other causes.** The report concludes that cancel and booking decisions in this data are likely driven by factors other than price, distance and discount.
