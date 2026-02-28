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

