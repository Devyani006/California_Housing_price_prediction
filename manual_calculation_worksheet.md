# Manual Calculation Worksheet – Activity 1 (typed)

Sample: 6 training observations, features X1 = median_income, X2 = housing_median_age, target y_k = median_house_value / 1000.

## Observations
|   obs |   row id |   median_income |   housing_median_age |   y_k |
|------:|---------:|----------------:|---------------------:|------:|
|     1 |    10349 |          7.5488 |                   25 | 278.1 |
|     2 |     9657 |          1.9732 |                   32 |  69.4 |
|     3 |     7631 |          3.5094 |                   11 | 162.9 |
|     4 |     1790 |          1.0521 |                   40 |  69.1 |
|     5 |     4569 |          2.3438 |                   42 | 213.3 |
|     6 |     8236 |          4.6629 |                   25 | 162.5 |

## Design matrix X (1, X1, X2)
            1.0000          7.5488         25.0000
            1.0000          1.9732         32.0000
            1.0000          3.5094         11.0000
            1.0000          1.0521         40.0000
            1.0000          2.3438         42.0000
            1.0000          4.6629         25.0000

## X^T X
            6.0000         21.0902        175.0000
           21.0902        101.5367        547.5619
          175.0000        547.5619      5,759.0000

## X^T y
          955.3000
        4,138.2965
       26,750.3000

## det(X^T X) = 80,276.8780

## (X^T X)^-1
            3.5493         -0.3193         -0.0775
           -0.3193          0.0489          0.0051
           -0.0775          0.0051          0.0020

## Solution b = (X^T X)^-1 X^T y
b0 = -3.7690, b1 = 32.5755, b2 = 1.6622

Fitted equation: y_k = -3.769 + 32.576*median_income + 1.662*housing_median_age

## Worked prediction and residual (observation 1)
y_hat = 283.693, actual = 278.100, residual = -5.593
