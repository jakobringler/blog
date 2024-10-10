---
title: RMSE
draft: false
tags:
  - machinelearning
  - math
---
Root Mean Squared Error (RMSE) is a widely used metric in statistics and machine learning to evaluate the accuracy of a model's predictions. It measures the average magnitude of the errors between predicted values and actual values, giving more weight to larger errors due to the squaring of differences.
### Formula

The RMSE is calculated using the following steps:

1. **Calculate the Errors**: For each predicted value $(\hat{y}_i​)$and actual value $({y}_i​)$, compute the difference (error):
    
    $ei=\hat{y}_i - y_i​$
    
1. **Square the Errors**: Square each error to remove negative signs and give more weight to larger errors:
    
    $e_i^2$
    
1. **Calculate the Mean of Squared Errors**: Find the average of these squared errors:
    
    $\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} e_i^2$
    
1. **Take the Square Root**: Finally, take the square root of the mean squared error:
    
    $\text{RMSE} = \sqrt{\text{MSE}} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} e_i^2}$
### Interpretation

- **Units**: RMSE is in the same units as the target variable, which makes it interpretable.
- **Sensitivity**: RMSE is sensitive to outliers; larger errors have a disproportionately high impact on the RMSE value.
- **Comparison**: Lower RMSE values indicate better model performance. It’s often compared against other metrics like Mean Absolute Error (MAE) for a comprehensive evaluation.

Overall, RMSE provides a useful summary measure of prediction accuracy and is commonly employed in regression tasks.

---

sources / further reading:
- [What is Root Mean Square Error?](https://c3.ai/glossary/data-science/root-mean-square-error-rmse/)

