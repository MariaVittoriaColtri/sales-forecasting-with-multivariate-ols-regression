# Sales Forecasting with OLS Regression: Iced Tea

## Overview

This project analyses 52 weeks of ice tea sales data to identify the key drivers of weekly revenue and build a forecasting model for marketing budget planning.

The central challenge was a structural one: all three advertising channels ran exclusively during the summer months, exactly when temperature was at its peak. This overlap made it difficult to isolate the advertising effect from seasonal demand.

## Libraries

Python: pandas, numpy, statsmodels, matplotlib, seaborn, scipy

## Methodology

I started with exploratory data analysis before building any model. Revenue followed a clear seasonal pattern, but holiday weeks showed consistently higher sales throughout the entire year and not just in summer. This indicated that the holiday effect was independent from the seasonal trend.

I recoded Price_delta as a categorical variable, because it takes only three fixed values (−10, 0, +10). Treating it as continuous would have imposed a linear relationship between price and revenue, which does not reflect the actual pricing structure.

Before running the regression, I assessed multicollinearity using VIF. Search scored 15.47, which falls in the severe range. Newspaper and Temperature were both moderate. I kept Search in the model to document the issue transparently because dropping a variable with high VIF does not solve the underlying problem, it just hides it.

I first ran a baseline model using only Newspaper (R^2 = 0.72) as a reference point, then the full model with all six predictors (R^2 = 0.97). The Newspaper coefficient dropped from 8.67 to 4.50 once Temperature was included. The baseline was absorbing part of the seasonal demand and attributing it to advertising. This is a textbook case of omitted variable bias.

For validation, I used a chronological train/test split, because the model should predict future weeks from past data. The test set covers mid-autumn to year-end, when there was no active advertising and temperatures dropped to 33 F. These conditions are very different from the training period, so a 5% MAPE on unseen data is a meaningful result.

Residual analysis confirmed that both linearity and normality hold, so the model coefficients are reliable.

## Results

Five out of six predictors were statistically significant. Holiday weeks had the largest impact at +804 euro per week, followed by Temperature (+27.61 euro per F), Newspaper (+4.50 euro per euro spent), and Social Media (+3.71 euro per euro spent). Price discount had a negative effect of −752 euro per tier. Search was not significant (p = 0.62).

Search was not significant due to severe multicollinearity with Temperature. This does not mean that Search advertising does not work, it means that the data cannot tell us, because Search and Temperature moved together the entire year.

Full-model R²: 0.97, Test MAPE: 5%

## Recommendations for the Marketing Team

- **Activate campaigns on every public holiday and not just in summer.** The model shows that holiday weeks generate 804 euro more in revenue compared to a regular week and this holds true in every season. This means that a holiday week in January performs significantly better than a regular summer week. Holiday weeks should have their own dedicated budget rather than being absorbed into the general seasonal plan.

- **Concentrate the advertising budget between weeks 20 and 36.** Sales are strongly driven by temperature and the data shows that revenue can vary by up to 1.220 euro between the coldest and warmest week of the year. Spending the same budget evenly across 52 weeks means wasting money in winter, when demand has a natural ceiling that no amount of advertising can overcome. The sweet spot is weeks 20 to 36, where high temperature and holiday weeks often overlap.

- **Prioritise Newspaper and Social Media, but treat their ROI estimates as a range and not exact numbers.** Newspaper returns an estimated 4.50 euro per euro spent and Social Media 3.71 euro. However, because both channels only ran in summer, their ROI may be slightly overstated: part of what looks like an advertising effect could still be seasonal demand. The real return is likely a bit lower, but both channels remain the most efficient ones available in this dataset.

- **Do not cut Search without testing it first in a low-season week.** Search appeared statistically irrelevant in this analysis, but that is a data problem, not a channel problem. Search only ran in summer, at the same time as temperature peaked and the other two channels were also active. It was impossible to separate its effect from everything else happening at the same time. Before making any budget decision on Search, it should be tested in a quiet period like a non-holiday week in autumn or winter where its impact can actually be measured in isolation.

- **Use discounts sparingly and only when the volume uplift justifies them.** Each discount tier costs 752 euro in lost revenue on average. Given that the typical order in this dataset is worth between 15 and 23 euro, you would need a very high number of extra orders just to cover that loss. Discounts should be used only for specific goals like reactivating dormant customers, not as a default promotional tool.
