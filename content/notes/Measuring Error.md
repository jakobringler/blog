---
title: Measuring Error
draft: false
tags:
  - machinelearning
  - math
---
There are several common ways to measure error mathematically, especially in the context of machine learning and statistics. A lot of these come in handy for all sorts of procedural setups that need to compare a bunch of numbers. Here are a few of the most widely used methods:

1. **Mean Absolute Error (MAE)**: This measures the average absolute differences between predicted and actual values. It’s simple to understand and gives equal weight to all errors.

$MAE=1n∑i=1n∣yi−y^i∣$

2. **Mean Squared Error (MSE)**: This measures the average of the squared differences between predicted and actual values. Squaring the errors means larger errors have a bigger impact, which can be useful in some cases.

$MSE=1n∑i=1n(yi−y^i)2$

3. **Root Mean Squared Error ([[notes/RMSE|RMSE]])**: This is the square root of the MSE, bringing it back to the same scale as the original values. It’s sensitive to outliers like MSE but provides a more interpretable measure of error.

$\text{RMSE} = \sqrt{\text{MSE}}$

4. **Mean Absolute Percentage Error (MAPE)**: This expresses the error as a percentage, which can be useful for understanding the error relative to the size of the values being predicted.

$\text{MAPE} = \frac{1}{n} \sum_{i=1}^{n} \left|\frac{y_i - \hat{y}_i}{y_i}\right| \times 100$

5. **Huber Loss**: This combines the benefits of MAE and MSE. It behaves like MSE when errors are small and like MAE when they are large, making it robust to outliers.

$L_{\delta}(y, \hat{y}) = \begin{cases} \frac{1}{2}(y - \hat{y})^2 & \text{if } |y - \hat{y}| \leq \delta \\\delta (|y - \hat{y}| - \frac{1}{2}\delta) & \text{otherwise}\end{cases}$

6. **Cross-Entropy Loss**: Commonly used in classification problems, this measures the difference between the predicted probability distribution and the true distribution. It’s particularly useful for multi-class classification tasks.

---

sources / further reading:
- [All about loss functions like MSE, MAE, RMSE etc - Abhishek Jain](https://medium.com/@abhishekjainindore24/all-about-loss-functions-like-mse-mae-rmse-etc-36596e3802f5)

