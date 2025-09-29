# Python for Data Science Tutorial 3: Tree-Based Algorithms

## Table of Contents
1. [Introduction to Tree Methods](#introduction)
2. [What are Decision Trees?](#decision-trees)
3. [Understanding Tree Components](#tree-components)
4. [How Decision Trees Make Decisions](#decision-making)
5. [Random Forests](#random-forests)
6. [Implementation with Python](#python-implementation)
7. [Practical Example: Medical Data](#practical-example)
8. [Model Evaluation](#model-evaluation)
9. [Key Takeaways](#key-takeaways)

---

## 1. Introduction to Tree Methods {#introduction}

Tree-based methods are powerful machine learning algorithms that make predictions by learning simple decision rules from data. Think of them as a flowchart that helps make decisions based on asking a series of questions.

**Why Use Tree Methods?**
- Easy to understand and interpret
- Handle both numerical and categorical data
- Require little data preprocessing
- Can capture non-linear relationships
- Works well with real-world data

---

## 2. What are Decision Trees? {#decision-trees}

### The Tennis Example

Let's understand decision trees with a simple real-world scenario:

**Scenario:** You play tennis every Saturday and invite a friend. Whether your friend shows up depends on various factors like weather, temperature, humidity, and wind conditions.

**Sample Data:**

| Day | Temperature | Outlook  | Humidity | Windy | Friend Showed Up? |
|-----|-------------|----------|----------|-------|-------------------|
| 1   | Mild        | Sunny    | 80%      | No    | Yes               |
| 2   | Hot         | Sunny    | 75%      | Yes   | No                |
| 3   | Cool        | Overcast | 65%      | No    | Yes               |
| 4   | Mild        | Rain     | 70%      | Yes   | No                |
| 5   | Cool        | Overcast | 60%      | No    | Yes               |

### Building a Decision Tree

From this data, we can build a decision tree that looks like this:

```
                    [Outlook]
                   /    |    \
              Sunny  Overcast  Rain
               /        |        \
         [Humidity]    YES    [Windy]
          /      \             /     \
      High      Low          Yes     No
       /          \          /        \
      NO         YES        NO       YES
```

**How to Read This Tree:**
1. Start at the top (root node): Check the Outlook
2. If Outlook is "Overcast" → Friend will come (YES)
3. If Outlook is "Sunny" → Check Humidity
   - If Humidity is High → Friend won't come (NO)
   - If Humidity is Low → Friend will come (YES)
4. If Outlook is "Rain" → Check if it's Windy
   - If Windy is Yes → Friend won't come (NO)
   - If Windy is No → Friend will come (YES)

---

## 3. Understanding Tree Components {#tree-components}

### Key Components of a Decision Tree

1. **Root Node**: The topmost node that represents the entire dataset and performs the first split
   - In our example: "Outlook" is the root node

2. **Internal Nodes (Decision Nodes)**: Nodes that split the data based on feature values
   - In our example: "Humidity" and "Windy" are internal nodes

3. **Branches (Edges)**: The outcome of a split connecting nodes
   - Lines connecting "Outlook" to "Sunny", "Overcast", and "Rain"

4. **Leaf Nodes (Terminal Nodes)**: Final nodes that predict the outcome
   - In our example: "YES" and "NO" are leaf nodes

### Visual Representation

```
Root Node (Where we start)
    ↓
Internal Nodes (Where we make decisions)
    ↓
Leaf Nodes (Where we get our answer)
```

---

## 4. How Decision Trees Make Decisions {#decision-making}

### The Concept of Splitting

Decision trees work by finding the best features to split the data. But how do they decide which feature is "best"?

### Example: Choosing the Best Split

Imagine you have data with three features (X, Y, Z) trying to predict two classes (A or B):

**Dataset:**
```
Feature X | Feature Y | Feature Z | Class
----------|-----------|-----------|------
    1     |     5     |    10     |   A
    2     |     5     |    15     |   A
    8     |     15    |    12     |   B
    9     |     15    |    18     |   B
```

**Scenario 1: Splitting on Feature Y**
```
If Y < 10:
    → Class A instances (perfect separation!)
If Y ≥ 10:
    → Class B instances (perfect separation!)
```

**Scenario 2: Splitting on Feature X**
```
If X < 5:
    → Mix of Class A and B (poor separation)
If X ≥ 5:
    → Mix of Class A and B (poor separation)
```

**Result:** Feature Y gives us the best split because it perfectly separates the classes!

### Information Gain and Entropy

**Entropy** measures the impurity or disorder in the data:
- High entropy = Mixed classes (impure)
- Low entropy = Mostly one class (pure)

**Information Gain** measures how much a feature reduces entropy:
- Higher information gain = Better feature for splitting

**Formula (for understanding, not memorization):**
```
Information Gain = Entropy(parent) - Weighted Average Entropy(children)
```

**Key Principle:** Decision trees always choose the feature with the highest information gain for splitting!

---

## 5. Random Forests {#random-forests}

### What are Random Forests?

A Random Forest is an ensemble (collection) of many decision trees working together. Think of it as "wisdom of the crowd" - many trees voting on the final prediction.

### Why Random Forests?

**Problem with Single Decision Trees:**
- High variance (small changes in data → completely different trees)
- Can overfit the training data
- One strong feature dominates all trees

**Solution: Random Forests**
- Build multiple trees using different samples of data
- Each tree sees random features at each split
- Average predictions across all trees
- More stable and accurate predictions

### How Random Forests Work

**Step-by-Step Process:**

1. **Bootstrap Sampling**
   - Take random samples from training data WITH replacement
   - Each tree gets a different subset of data
   
   ```
   Original Data: [1, 2, 3, 4, 5]
   
   Tree 1 Sample: [1, 2, 2, 4, 5]
   Tree 2 Sample: [1, 1, 3, 3, 5]
   Tree 3 Sample: [2, 3, 4, 4, 5]
   ```

2. **Random Feature Selection**
   - At each split, consider only M random features (not all P features)
   - For classification: M = √P (square root of total features)
   - This decorrelates the trees
   
   ```
   If you have 9 features total:
   - At each split, randomly select √9 = 3 features
   - Choose best split from only those 3 features
   ```

3. **Build Multiple Trees**
   - Create 100s or 1000s of trees
   - Each tree is slightly different

4. **Make Predictions**
   - Classification: Majority vote from all trees
   - Regression: Average prediction from all trees

### Random Forest Example

```
Suppose we want to predict if it will rain:

Tree 1 predicts: Yes
Tree 2 predicts: No
Tree 3 predicts: Yes
Tree 4 predicts: Yes
Tree 5 predicts: No

Final Prediction: Yes (3 out of 5 trees voted Yes)
```

### Why Random Features Matter

**Without Random Features:**
```
Suppose "Temperature" is the strongest predictor:

Tree 1: Splits on Temperature first
Tree 2: Splits on Temperature first
Tree 3: Splits on Temperature first
...
Result: All trees are highly similar (correlated)
```

**With Random Features:**
```
Tree 1: Temperature not available, splits on Humidity
Tree 2: Temperature available, splits on Temperature
Tree 3: Temperature not available, splits on Wind
...
Result: Trees are different (decorrelated) → Better predictions!
```

---

## 6. Implementation with Python {#python-implementation}

### Setting Up the Environment

```python
# Import essential libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# For Jupyter notebooks
%matplotlib inline
```

**What each library does:**
- `pandas`: Data manipulation and analysis
- `numpy`: Numerical operations
- `matplotlib` & `seaborn`: Data visualization
- `%matplotlib inline`: Display plots in Jupyter notebook

### Loading Data

```python
# Load the dataset
kyphosis_df = pd.read_csv('kyphosis.csv')

# View first few rows
kyphosis_df.head()
```

**Understanding the Dataset:**
- **Kyphosis**: Medical spinal condition (Target variable)
  - "absent" = Surgery was successful
  - "present" = Condition still exists after surgery
- **Age**: Age of patient in months
- **Number**: Number of vertebrae involved in operation
- **Start**: Number of first vertebra operated on

### Exploratory Data Analysis

```python
# Check dataset information
kyphosis_df.info()
```

**Output:**
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 81 entries, 0 to 80
Data columns (total 4 columns):
Kyphosis    81 non-null object
Age         81 non-null int64
Number      81 non-null int64
Start       81 non-null int64
dtypes: int64(3), object(1)
```

**What this tells us:**
- Small dataset: Only 81 patients
- No missing values (all columns have 81 entries)
- 3 numerical features + 1 target variable

### Visualizing Relationships

```python
# Create pairplot to see feature relationships
sns.pairplot(kyphosis_df, hue='Kyphosis')
plt.show()
```

**What to look for in pairplot:**
- Diagonal: Distribution of each feature
- Off-diagonal: Relationships between features
- Colors: Different colors for "absent" vs "present"

---

## 7. Practical Example: Medical Data {#practical-example}

### Preparing the Data

```python
# Separate features and target
X = kyphosis_df.drop('Kyphosis', axis=1)  # All columns except target
y = kyphosis_df['Kyphosis']                # Only target column

# Split into training and testing sets
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, 
    test_size=0.3,  # 30% for testing, 70% for training
    random_state=101
)
```

**Why split the data?**
- **Training Set (70%)**: Teach the model patterns
- **Testing Set (30%)**: Evaluate how well model learned
- Like studying with practice problems, then taking a real exam!

### Training a Decision Tree

```python
from sklearn.tree import DecisionTreeClassifier

# Create and train the model
dtree = DecisionTreeClassifier()
dtree.fit(X_train, y_train)
```

**What happens during training:**
1. Algorithm examines training data
2. Finds best features to split on
3. Builds the tree structure
4. Creates rules for predictions

### Making Predictions with Decision Tree

```python
# Make predictions on test set
predictions = dtree.predict(X_test)

# Evaluate performance
from sklearn.metrics import classification_report, confusion_matrix

print("Confusion Matrix:")
print(confusion_matrix(y_test, predictions))
print("\n")

print("Classification Report:")
print(classification_report(y_test, predictions))
```

**Sample Output:**
```
Confusion Matrix:
[[15  1]
 [ 5  4]]

Classification Report:
              precision    recall  f1-score   support

      absent       0.75      0.94      0.83        16
     present       0.80      0.44      0.57         9

    accuracy                           0.76        25
   macro avg       0.77      0.69      0.70        25
weighted avg       0.77      0.76      0.73        25
```

**Understanding the Results:**

**Confusion Matrix:**
```
                Predicted
                Absent  Present
Actual Absent     15      1        (15 correct, 1 wrong)
       Present     5      4        (4 correct, 5 wrong)
```

**Metrics Explained:**
- **Precision (Absent)**: 75% - When model predicts "absent", it's correct 75% of the time
- **Recall (Absent)**: 94% - Model correctly identifies 94% of actual "absent" cases
- **F1-Score**: Harmonic mean of precision and recall
- **Accuracy**: 76% - Overall, model is correct 76% of the time

### Training a Random Forest

```python
from sklearn.ensemble import RandomForestClassifier

# Create Random Forest with 200 trees
rfc = RandomForestClassifier(n_estimators=200)
rfc.fit(X_train, y_train)

# Make predictions
rfc_pred = rfc.predict(X_test)

# Evaluate performance
print("Confusion Matrix:")
print(confusion_matrix(y_test, rfc_pred))
print("\n")

print("Classification Report:")
print(classification_report(y_test, rfc_pred))
```

**Sample Output:**
```
Confusion Matrix:
[[16  0]
 [ 3  6]]

Classification Report:
              precision    recall  f1-score   support

      absent       0.84      1.00      0.91        16
     present       1.00      0.67      0.80         9

    accuracy                           0.88        25
   macro avg       0.92      0.83      0.86        25
weighted avg       0.90      0.88      0.87        25
```

**Comparison:**

| Metric | Decision Tree | Random Forest | Winner |
|--------|---------------|---------------|--------|
| Accuracy | 76% | 88% | Random Forest ✓ |
| Precision (absent) | 75% | 84% | Random Forest ✓ |
| Recall (absent) | 94% | 100% | Random Forest ✓ |

**Why Random Forest Performed Better:**
- Multiple trees voting → More stable predictions
- Random feature selection → Less overfitting
- Bootstrap sampling → Better generalization

---

## 8. Model Evaluation {#model-evaluation}

### Understanding the Confusion Matrix

```
                    Predicted
                 Negative  Positive
Actual Negative     TN        FP      ← Type I Error (False Positive)
       Positive     FN        TP      ← Type II Error (False Negative)
```

**Definitions:**
- **True Positive (TP)**: Correctly predicted positive
- **True Negative (TN)**: Correctly predicted negative
- **False Positive (FP)**: Wrongly predicted positive (Type I Error)
- **False Negative (FN)**: Wrongly predicted negative (Type II Error)

### Key Metrics Explained

**1. Accuracy**
```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```
- Overall correctness of the model
- Good for balanced datasets
- Can be misleading with imbalanced data

**2. Precision**
```
Precision = TP / (TP + FP)
```
- "When model predicts positive, how often is it correct?"
- Important when false positives are costly
- Example: Email spam detection (don't want to mark important emails as spam)

**3. Recall (Sensitivity)**
```
Recall = TP / (TP + FN)
```
- "Of all actual positives, how many did model catch?"
- Important when false negatives are costly
- Example: Disease detection (don't want to miss sick patients)

**4. F1-Score**
```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```
- Harmonic mean of precision and recall
- Good for imbalanced datasets
- Balances both metrics

### Real-World Example

**Medical Diagnosis:**
```python
# Check class distribution
print(y.value_counts())
```

**Output:**
```
absent     64
present    17
```

**Imbalanced Dataset!**
- 64 absent cases (79%)
- 17 present cases (21%)

**Why this matters:**
- Model could predict "absent" all the time and get 79% accuracy
- But it would miss all "present" cases (dangerous!)
- Need to look at precision and recall for each class

---

## 9. Key Takeaways {#key-takeaways}

### When to Use Decision Trees

**Advantages:**
- ✓ Easy to understand and interpret
- ✓ Visual representation possible
- ✓ Handles both numerical and categorical data
- ✓ Requires little data preprocessing
- ✓ Can capture non-linear relationships

**Disadvantages:**
- ✗ Can overfit easily
- ✗ High variance (unstable)
- ✗ Biased toward features with more levels
- ✗ Not optimal for all problems

### When to Use Random Forests

**Advantages:**
- ✓ Usually better accuracy than single tree
- ✓ Reduces overfitting
- ✓ Handles high-dimensional data well
- ✓ Provides feature importance
- ✓ Works well "out of the box"

**Disadvantages:**
- ✗ Less interpretable than single tree
- ✗ Slower to train and predict
- ✗ Requires more memory
- ✗ Can still overfit noisy data

### Best Practices

**1. Data Preparation**
```python
# Always split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

# Check for class imbalance
print(y.value_counts())
```

**2. Model Selection**
```python
# Start with Random Forest for quick baseline
rfc = RandomForestClassifier(n_estimators=100)

# Tune parameters if needed
rfc = RandomForestClassifier(
    n_estimators=200,      # More trees
    max_depth=10,          # Limit tree depth
    min_samples_split=5    # Minimum samples to split
)
```

**3. Evaluation**
```python
# Use multiple metrics
from sklearn.metrics import accuracy_score, precision_score, recall_score

accuracy = accuracy_score(y_test, predictions)
precision = precision_score(y_test, predictions, pos_label='present')
recall = recall_score(y_test, predictions, pos_label='present')

print(f"Accuracy: {accuracy:.2f}")
print(f"Precision: {precision:.2f}")
print(f"Recall: {recall:.2f}")
```

### Quick Reference Guide

**Decision Tree vs Random Forest:**

```python
# DECISION TREE - Fast, interpretable, less accurate
from sklearn.tree import DecisionTreeClassifier
model = DecisionTreeClassifier()

# RANDOM FOREST - Slower, less interpretable, more accurate
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(n_estimators=100)

# Both use the same methods:
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

### Common Parameters

**RandomForestClassifier Parameters:**
- `n_estimators`: Number of trees (default=100, more is usually better)
- `max_depth`: Maximum depth of trees (None=unlimited)
- `min_samples_split`: Minimum samples to split node (default=2)
- `min_samples_leaf`: Minimum samples in leaf node (default=1)
- `random_state`: Seed for reproducibility

### Real-World Applications

**Classification Tasks:**
- Medical diagnosis (disease present/absent)
- Customer churn prediction (will leave/stay)
- Loan default prediction (will default/repay)
- Image classification (cat/dog/bird)
- Spam detection (spam/not spam)

**Regression Tasks:**
- House price prediction
- Sales forecasting
- Stock price prediction
- Temperature prediction

### Next Steps

1. **Practice**: Try different datasets
2. **Experiment**: Tune hyperparameters
3. **Compare**: Test against other algorithms
4. **Learn**: Study feature importance
5. **Apply**: Build real-world projects

---

## Summary

Tree-based methods are powerful tools in machine learning:

- **Decision Trees**: Simple, interpretable models that split data based on features
- **Random Forests**: Ensemble of decision trees for better accuracy and stability
- **Key Concepts**: Information gain, entropy, bootstrap sampling, feature randomization
- **Implementation**: Easy with scikit-learn in just a few lines of code
- **Evaluation**: Use confusion matrix, precision, recall, and F1-score

Random Forests are often the first choice for data scientists because they:
- Work well without much tuning
- Provide good baseline accuracy
- Handle various types of data
- Are robust to overfitting

**Remember:** Start simple with a single decision tree to understand the data, then move to Random Forests for better predictions!

---

## Additional Resources

- **Scikit-learn Documentation**: https://scikit-learn.org/
- **Decision Tree Theory**: Chapter 8, Introduction to Statistical Learning
- **Practice Datasets**: UCI Machine Learning Repository
- **Visualization**: Graphviz for decision tree visualization (optional advanced topic)

Happy Learning! 🌲📊🐍
