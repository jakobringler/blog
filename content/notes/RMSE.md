---
title: RMSE
draft: false
tags:
  - machinelearning
  - math
  - houdini
---
Root Mean Squared Error (RMSE) is a widely used metric in statistics and machine learning to evaluate the accuracy of a model's predictions. It measures the average magnitude of the errors between predicted values and actual values, giving more weight to larger errors due to the squaring of differences.

### How to Calculate RMSE

1. **Calculate the Errors**: For each predicted value $(\hat{y}_i​)$and actual value $({y}_i​)$, compute the difference (error):
    
    $ei=\hat{y}_i - y_i​$
    
1. **Square the Errors**: Square each error to remove negative signs and give more weight to larger errors:
    
    $e_i^2$
    
1. **Calculate the Mean of Squared Errors**: Find the average of these squared errors:
    
    $\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} e_i^2$
    
1. **Take the Square Root**: Finally, take the square root of the mean squared error:
    
    $\text{RMSE} = \sqrt{\text{MSE}} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} e_i^2}$

### Now in VEX

// point wrangle

```C
float input;
float output;

float error = output - input;
@squarederror = pow(error, 2);
@rse = sqrt(@squarederror); // this is the local error 
```

// detail wrangle

```C
float value;
float values[];

for (int i=0; i<@numpt; i++)
{
	value = point(geoself(), "squarederror", i);
	append(values,value);
}
  
float mse = avg(values);

@rmse = sqrt(mse);
```

(alternatively you can promote the attribute to detail using average mode)

### What RMSE Tells You

- **Units**: The RMSE is in the same units as whatever you’re predicting, making it easier to understand.
- **Sensitivity**: RMSE really pays attention to outliers; big errors can swing the RMSE value quite a bit.
- **Comparison**: A lower RMSE means your model is doing a better job. It’s often compared to other metrics, like Mean Absolute Error (MAE), to get a fuller picture of performance.

In short, RMSE is a handy way to summarize how accurate your predictions are, especially in regression tasks!

---

sources / further reading:
- [What is Root Mean Square Error?](https://c3.ai/glossary/data-science/root-mean-square-error-rmse/)

