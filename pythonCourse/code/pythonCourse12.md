# Logistic Regression for Classification

## Table of Contents
1. [Introduction to Classification](#introduction-to-classification)
2. [Understanding Logistic Regression](#understanding-logistic-regression)
3. [The Sigmoid Function](#the-sigmoid-function)
4. [Model Evaluation with Confusion Matrix](#model-evaluation-with-confusion-matrix)
5. [Hands-on Project: Titanic Survival Prediction](#hands-on-project-titanic-survival-prediction)
6. [Data Exploration and Visualization](#data-exploration-and-visualization)
7. [Data Cleaning and Preprocessing](#data-cleaning-and-preprocessing)
8. [Building and Training the Model](#building-and-training-the-model)
9. [Summary and Best Practices](#summary-and-best-practices)

---

## Introduction to Classification

### What is Classification?

Classification is a fundamental machine learning problem where we predict which category or class a new observation belongs to, based on training data with known outcomes.

**Key Characteristics:**
- Predicts discrete categories (not continuous values)
- Uses labeled training data
- Output is a class or category

### Real-World Examples of Classification Problems

1. **Email Spam Detection**: Classify emails as "spam" or "not spam"
2. **Medical Diagnosis**: Determine if a patient has a disease or not
3. **Loan Default Prediction**: Predict if someone will default on their loan
4. **Image Recognition**: Classify images as containing cats, dogs, etc.

### Binary vs Multi-class Classification

- **Binary Classification**: Two possible outcomes (0 or 1, Yes or No)
- **Multi-class Classification**: More than two possible outcomes

In this tutorial, we'll focus on binary classification using the convention:
- **Class 0**: Negative outcome (e.g., no disease, loan default)
- **Class 1**: Positive outcome (e.g., has disease, loan approved)

---

## Understanding Logistic Regression

### Why Not Use Linear Regression for Classification?

Linear regression predicts continuous values, but classification requires discrete categories. If we try to use linear regression for binary classification:

**Problems with Linear Regression for Classification:**
- Can predict probabilities below 0% or above 100%
- Poor fit to binary data
- Doesn't handle the S-shaped relationship well

### The Solution: Logistic Regression

Logistic regression transforms linear regression output to always stay between 0 and 1, making it perfect for probability predictions.

**Key Advantages:**
- Output always between 0 and 1 (valid probability)
- Can handle any input value
- Natural interpretation as probability of belonging to class 1

---

## The Sigmoid Function

### Understanding the Sigmoid Function

The sigmoid (logistic) function is the mathematical foundation of logistic regression:

**Formula:** `σ(z) = 1 / (1 + e^(-z))`

Where:
- `σ(z)` = sigmoid function output
- `z` = any real number input
- `e` = Euler's number (≈ 2.718)

### Key Properties of the Sigmoid Function

1. **Range**: Always outputs values between 0 and 1
2. **S-Shape**: Creates a smooth S-shaped curve
3. **Interpretable**: Output can be interpreted as probability

### How Logistic Regression Works

1. Start with linear model: `z = β₀ + β₁x₁ + β₂x₂ + ...`
2. Apply sigmoid function: `P(y=1) = σ(z) = 1 / (1 + e^(-z))`
3. Set classification threshold (usually 0.5):
   - If P(y=1) ≥ 0.5 → Predict Class 1
   - If P(y=1) < 0.5 → Predict Class 0

---

## Model Evaluation with Confusion Matrix

### What is a Confusion Matrix?

A confusion matrix is a table that describes the performance of a classification model on test data where true values are known.

### Basic Terms in Classification

For binary classification, we have four possible outcomes:

| **Actual** | **Predicted** | **Term** | **Description** |
|------------|---------------|----------|-----------------|
| 1 (Positive) | 1 (Positive) | True Positive (TP) | Correctly predicted positive |
| 0 (Negative) | 0 (Negative) | True Negative (TN) | Correctly predicted negative |
| 0 (Negative) | 1 (Positive) | False Positive (FP) | Incorrectly predicted positive (Type I Error) |
| 1 (Positive) | 0 (Negative) | False Negative (FN) | Incorrectly predicted negative (Type II Error) |

### Example Confusion Matrix

Let's say we're predicting disease presence with 165 patients:

```
                 PREDICTED
              No    Yes
ACTUAL   No   50     10    (60 actual negatives)
        Yes    5    100    (105 actual positives)
              (55)  (110)
```

### Important Metrics

#### 1. Accuracy
**Formula**: `(TP + TN) / Total`
**Meaning**: Overall, how often is the classifier correct?
**Example**: `(50 + 100) / 165 = 0.91 = 91%`

#### 2. Misclassification Rate (Error Rate)
**Formula**: `(FP + FN) / Total`
**Meaning**: Overall, how often is the classifier wrong?
**Example**: `(10 + 5) / 165 = 0.09 = 9%`

### Memory Trick for Type I vs Type II Errors

- **Type I Error (False Positive)**: "Crying Wolf" - Saying there's danger when there isn't
  - Example: Telling a man he's pregnant
- **Type II Error (False Negative)**: "Missing the Wolf" - Missing real danger
  - Example: Telling a pregnant woman she's not pregnant

---

## Hands-on Project: Titanic Survival Prediction

### Project Overview

We'll predict whether passengers survived the Titanic disaster based on their characteristics like age, gender, passenger class, etc.

**Dataset**: Famous Titanic dataset from Kaggle
**Goal**: Binary classification (Survived = 1, Did not survive = 0)
**Features**: Passenger class, age, gender, fare, family size, etc.

### Setting Up the Environment

```python
# Import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Set up matplotlib for inline plotting
%matplotlib inline
```

### Loading the Data

```python
# Load the Titanic dataset
train = pd.read_csv('titanic_train.csv')

# Display basic information about the dataset
print(train.head())
```

**Expected Output:**
```
   PassengerId  Survived  Pclass    Name     Sex  Age  SibSp  Parch  Ticket  Fare Cabin Embarked
0            1         0       3  Braund    male   22      1      0   A/5    7.25   NaN        S
1            2         1       1  Cumings female   38      1      0   PC      71.28  C85        C
2            3         1       3  Heikkinen female 26      0      0   STON    7.93   NaN        S
```

### Dataset Features Explained

- **PassengerId**: Unique identifier for each passenger
- **Survived**: Target variable (0 = died, 1 = survived)
- **Pclass**: Passenger class (1 = first class, 2 = second class, 3 = third class)
- **Name**: Passenger's name
- **Sex**: Gender (male/female)
- **Age**: Age in years
- **SibSp**: Number of siblings/spouses aboard
- **Parch**: Number of parents/children aboard
- **Ticket**: Ticket number
- **Fare**: Passenger fare paid
- **Cabin**: Cabin number
- **Embarked**: Port of embarkation (C = Cherbourg, Q = Queenstown, S = Southampton)

---

## Data Exploration and Visualization

### Checking for Missing Data

```python
# Create a heatmap to visualize missing data
plt.figure(figsize=(10, 6))
sns.heatmap(train.isnull(), yticklabels=False, cbar=False, cmap='viridis')
plt.title('Missing Data Heatmap')
plt.show()
```

**What this shows**: Yellow areas indicate missing data. We can see:
- Age has some missing values
- Cabin has many missing values
- Embarked has very few missing values

### Exploring Survival Patterns

#### Overall Survival Count
```python
# Set seaborn style
sns.set_style('whitegrid')

# Count plot of survival
plt.figure(figsize=(6, 4))
sns.countplot(x='Survived', data=train)
plt.title('Survival Count')
plt.xlabel('Survived (0 = No, 1 = Yes)')
plt.show()
```

#### Survival by Gender
```python
# Survival by gender
plt.figure(figsize=(8, 5))
sns.countplot(x='Survived', hue='Sex', data=train, palette='RdYlBu_r')
plt.title('Survival Count by Gender')
plt.show()
```

**Key Insight**: Women had much higher survival rates than men, following the "women and children first" maritime protocol.

#### Survival by Passenger Class
```python
# Survival by passenger class
plt.figure(figsize=(8, 5))
sns.countplot(x='Survived', hue='Pclass', data=train)
plt.title('Survival Count by Passenger Class')
plt.show()
```

**Key Insight**: First-class passengers had higher survival rates, likely due to better access to lifeboats.

### Age Distribution Analysis

```python
# Age distribution
plt.figure(figsize=(10, 6))
train['Age'].hist(bins=30, edgecolor='black', alpha=0.7)
plt.title('Age Distribution of Passengers')
plt.xlabel('Age')
plt.ylabel('Number of Passengers')
plt.show()
```

### Family Size Analysis

```python
# Siblings/Spouses count
plt.figure(figsize=(8, 5))
sns.countplot(x='SibSp', data=train)
plt.title('Number of Siblings/Spouses Aboard')
plt.show()
```

**Key Insight**: Most passengers traveled alone or with one family member.

### Fare Distribution

```python
# Fare distribution
plt.figure(figsize=(10, 6))
train['Fare'].hist(bins=40, figsize=(10, 4), edgecolor='black', alpha=0.7)
plt.title('Fare Distribution')
plt.xlabel('Fare')
plt.ylabel('Number of Passengers')
plt.show()
```

---

## Data Cleaning and Preprocessing

### Handling Missing Data

#### Age Imputation Strategy

Instead of using the overall mean age, we'll use a smarter approach by imputing age based on passenger class:

```python
# Check average age by passenger class
plt.figure(figsize=(10, 7))
sns.boxplot(x='Pclass', y='Age', data=train)
plt.title('Age Distribution by Passenger Class')
plt.show()
```

**Observation**: First-class passengers tend to be older, suggesting wealth accumulates with age.

#### Creating Age Imputation Function

```python
def impute_age(cols):
    Age = cols[0]
    Pclass = cols[1]
    
    if pd.isnull(Age):
        if Pclass == 1:
            return 37  # Average age for 1st class
        elif Pclass == 2:
            return 29  # Average age for 2nd class
        else:
            return 24  # Average age for 3rd class
    else:
        return Age

# Apply the imputation function
train['Age'] = train[['Age', 'Pclass']].apply(impute_age, axis=1)
```

#### Removing Unnecessary Columns

```python
# Drop cabin column (too many missing values)
train.drop('Cabin', axis=1, inplace=True)

# Drop rows with remaining missing values
train.dropna(inplace=True)

# Verify no missing data remains
sns.heatmap(train.isnull(), yticklabels=False, cbar=False, cmap='viridis')
plt.title('Missing Data After Cleaning')
plt.show()
```

### Converting Categorical Variables

Machine learning algorithms need numerical inputs. We'll convert categorical variables to dummy variables:

#### Converting Sex to Dummy Variable

```python
# Convert sex to dummy variable
sex = pd.get_dummies(train['Sex'], drop_first=True)
print(sex.head())
```

**Output:**
```
   male
0     1
1     0
2     0
3     0
4     1
```

#### Converting Embarked to Dummy Variables

```python
# Convert embarked to dummy variables
embarked = pd.get_dummies(train['Embarked'], drop_first=True)
print(embarked.head())
```

**Output:**
```
   Q  S
0  0  1
1  0  0
2  0  1
3  0  1
4  0  1
```

#### Combining All Features

```python
# Concatenate dummy variables with main dataset
train = pd.concat([train, sex, embarked], axis=1)

# Drop original categorical columns and unnecessary columns
train.drop(['Sex', 'Embarked', 'Name', 'Ticket', 'PassengerId'], axis=1, inplace=True)

# Display final cleaned dataset
print(train.head())
```

**Final Dataset Structure:**
- All numerical features
- Dummy variables for categorical features
- No missing data
- Ready for machine learning

---

## Building and Training the Model

### Preparing Features and Target

```python
# Separate features (X) and target (y)
X = train.drop('Survived', axis=1)  # All columns except 'Survived'
y = train['Survived']  # Target variable

print("Features shape:", X.shape)
print("Target shape:", y.shape)
```

### Train-Test Split

```python
from sklearn.model_selection import train_test_split

# Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.30, random_state=101
)

print("Training set size:", X_train.shape[0])
print("Test set size:", X_test.shape[0])
```

### Creating and Training the Model

```python
from sklearn.linear_model import LogisticRegression

# Create logistic regression model
logmodel = LogisticRegression()

# Train the model
logmodel.fit(X_train, y_train)

# Make predictions
predictions = logmodel.predict(X_test)
```

### Model Evaluation

#### Classification Report

```python
from sklearn.metrics import classification_report

# Generate detailed classification report
print(classification_report(y_test, predictions))
```

**Example Output:**
```
              precision    recall  f1-score   support

           0       0.83      0.85      0.84       105
           1       0.78      0.75      0.76        72

    accuracy                           0.81       177
   macro avg       0.80      0.80      0.80       177
weighted avg       0.81      0.81      0.81       177
```

**Interpreting the Results:**
- **Precision**: Of all positive predictions, how many were actually positive?
- **Recall**: Of all actual positives, how many did we predict correctly?
- **F1-score**: Harmonic mean of precision and recall
- **Support**: Number of actual occurrences of each class

#### Confusion Matrix

```python
from sklearn.metrics import confusion_matrix

# Create confusion matrix
cm = confusion_matrix(y_test, predictions)
print("Confusion Matrix:")
print(cm)

# Visualize confusion matrix
plt.figure(figsize=(6, 4))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')
plt.title('Confusion Matrix')
plt.ylabel('Actual')
plt.xlabel('Predicted')
plt.show()
```

### Understanding Model Performance

With our example results:
- **Accuracy**: 81% - The model correctly classifies 81% of passengers
- **Class 0 (Did not survive)**: Higher precision and recall
- **Class 1 (Survived)**: Slightly lower performance

This suggests our model is reasonably good at predicting survival, with some room for improvement.

---

## Summary and Best Practices

### Key Concepts Covered

1. **Classification vs Regression**: Understanding the difference and when to use each
2. **Logistic Regression**: Using the sigmoid function to transform linear output to probabilities
3. **Data Preprocessing**: Handling missing data and categorical variables
4. **Model Evaluation**: Using confusion matrices and classification metrics

### Best Practices for Classification Projects

#### Data Preparation
- Always explore your data first with visualizations
- Handle missing data appropriately (imputation vs deletion)
- Convert categorical variables to dummy variables
- Remove irrelevant features

#### Model Building
- Split data into training and testing sets
- Use appropriate evaluation metrics for your problem
- Consider the business impact of false positives vs false negatives

#### Model Improvement Ideas

1. **Feature Engineering**: Create new features from existing ones
   - Extract title from passenger names (Mr., Mrs., Dr., etc.)
   - Create family size feature (SibSp + Parch)
   - Extract deck information from cabin numbers

2. **Try Different Approaches**: 
   - Treat passenger class as categorical vs numerical
   - Use different imputation strategies for age
   - Include interaction terms between features

3. **Advanced Techniques**:
   - Cross-validation for more robust evaluation
   - Feature selection to identify most important variables
   - Hyperparameter tuning to optimize model performance

### When to Use Logistic Regression

**Good for:**
- Binary classification problems
- When you need probability estimates
- When interpretability is important
- As a baseline model for comparison

**Consider alternatives when:**
- You have very complex, non-linear relationships
- You have a large number of features
- You need to handle multiple classes efficiently

### Next Steps

After mastering logistic regression, consider exploring:
- Decision Trees and Random Forests
- Support Vector Machines
- Neural Networks
- Feature selection techniques
- Cross-validation strategies

Remember: Most of data science work involves data cleaning and exploration. The actual model building is often the smallest part of the project, but proper preparation leads to better results.

---

## Practice Exercises

1. **Feature Engineering**: Try creating a "family size" feature by combining SibSp and Parch
2. **Class Treatment**: Experiment with treating Pclass as categorical vs numerical
3. **Different Thresholds**: Try different classification thresholds instead of 0.5
4. **Title Extraction**: Extract titles from passenger names and use as features
5. **Cross-Validation**: Implement k-fold cross-validation for more robust evaluation

## Additional Resources

- Scikit-learn documentation for LogisticRegression
- Kaggle Titanic competition for more advanced techniques
- "Introduction to Statistical Learning" by James, Witten, Hastie, and Tibshirani (Sections 4-4.3)

---

*This tutorial provides a comprehensive introduction to logistic regression and classification. Practice with different datasets to reinforce these concepts and build your machine learning skills.*
