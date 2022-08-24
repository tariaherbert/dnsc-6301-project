# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Taria Herbert, `taria23@gwu.edu`
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: missing

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 
  * Python version: 3.7.13
  * Sklearn version: 1.0.2
* **Hyperparameters or other settings of your model**:
```
DecisionTreeClassifier(max_depth=6, random_state=12345)
```
### Quantitative Analysis

#### Area Under the Curve (AUC)

| Metric | Training | Validation | Test |
| ------ | -------- | ---------- | ---- |
| AUC | 0.783722 | 0.749610 | 0.7438 |

#### Adverse Impact Ratio (AIR)

| Protective Groups | AIR |
| ----------------- | --- |
| Hispanic-to-White | 0.83 |
| Black-to-White | 0.85 |
| Asian-to-White | 1.00 |
| Female-to-Male | 1.02 |

#### Correlation Heatmap
![Correlation Heatmap](https://github.com/tariaherbert/dnsc-6301-project/blob/main/correlation%20heatmap.png)
* **The correlation heatmap demonstrates the strength of relationships between variables. This heatmap displays a strong negative correlation between race and delinquency. This implies that customers of certain race groups may encounter less positive outcomes compared to other customers.**

#### Iteration Plot
![Iteration Plot](https://github.com/tariaherbert/dnsc-6301-project/blob/main/iteration%20plot.png)

### Ethical Considerations
* **Potential negative impacts of using this model**:
   * The final model is at depth 6 because there is a balance between the area under  the curve (AUC) and adverse impact ratio (AIR). This model has good fairness and performance. This model violates Title VI of the Civil Rights Act of 1964, which prohibits discrimination against race, color, and national origin. Since the model contains demographic information to decide whether a customer’s next payment is delinquent. This is disparate impact because the model will disproportionately impact protected groups. The final model has uncertainties although the outcome has improved because the adverse impact ratio (AIR) is barely above the accepted threshold of 0.80 for the hispanics and blacks. Also, the model associates numbers to the different protected groups, which unintentionally causes the model to rank and order the different type of customers within the protected groups.
* **Potential uncertainties relating to the impacts of using this model**:
   * missing
