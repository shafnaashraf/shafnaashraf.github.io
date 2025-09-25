# k-Nearest Neighbors (KNN) Algorithm

## Table of Contents
1. [Introduction to k-Nearest Neighbors](#introduction)
2. [How KNN Works](#how-knn-works)
3. [Understanding the Algorithm with Examples](#algorithm-examples)
4. [Choosing the Right K Value](#choosing-k)
5. [Data Preprocessing for KNN](#data-preprocessing)
6. [Python Implementation](#python-implementation)
7. [The Elbow Method for Optimal K](#elbow-method)
8. [Pros and Cons of KNN](#pros-and-cons)
9. [KNN vs Linear Regression vs Logistic Regression](#comparison)
10. [Real-World Applications](#real-world-applications)

---

## 1. Introduction to k-Nearest Neighbors {#introduction}

**k-Nearest Neighbors (KNN)** is one of the simplest and most intuitive machine learning algorithms. It's a **classification algorithm** that works on a very simple principle: "**Tell me who your neighbors are, and I'll tell you who you are**."

### What is KNN?
KNN is a **lazy learning algorithm** that stores all training data and makes predictions based on the similarity of new data points to the training data. When given a new data point to classify, KNN looks at the 'k' nearest neighbors in the training data and assigns the class that appears most frequently among those neighbors.

### Key Concepts:
- **k**: The number of nearest neighbors to consider
- **Distance**: How we measure "nearness" between data points
- **Classification**: Predicting the class/category of new data points

---

## 2. How KNN Works {#how-knn-works}

### The Simple Process:

1. **Store all training data** (this is why it's called "lazy learning")
2. **For a new data point**:
   - Calculate distance from the new point to ALL training points
   - Sort all training points by distance (closest first)
   - Take the **k closest neighbors**
   - **Vote**: The class that appears most among these k neighbors wins
   - Assign that winning class to the new point

### Visual Example: Dogs vs Horses

Imagine we have data about dogs and horses with their heights and weights:

```
Training Data:
Dogs (Blue):    Heights: [20, 25, 30] inches, Weights: [15, 25, 35] lbs
Horses (Red):   Heights: [60, 65, 70] inches, Weights: [800, 900, 1000] lbs

New Animal:     Height: 28 inches, Weight: 30 lbs
```

**Question**: Is this new animal a dog or horse?

**KNN Process** (with k=3):
1. Calculate distances to all training points
2. Find 3 closest neighbors
3. If 2 out of 3 neighbors are dogs → Predict: Dog
4. If 2 out of 3 neighbors are horses → Predict: Horse

In this case, the new animal (28 inches, 30 lbs) is clearly closer to dogs, so KNN would correctly classify it as a dog.

---

## 3. Understanding the Algorithm with Examples {#algorithm-examples}

### Example 1: Simple 2D Classification

Let's say we have two classes:
- **Class A** (Yellow points): Representing one category
- **Class B** (Purple points): Representing another category

We want to classify a **new red star point**.

#### Scenario 1: k = 3
- Look at the 3 nearest neighbors to the red star
- Count: 2 purple points + 1 yellow point
- **Result**: Classify as Class B (Purple) because majority is purple

#### Scenario 2: k = 6
- Look at the 6 nearest neighbors to the red star
- Count: 4 yellow points + 2 purple points
- **Result**: Classify as Class A (Yellow) because majority is yellow

**Key Insight**: The choice of k significantly affects the prediction!

### Example 2: Real-World Scenario - Email Classification

Imagine classifying emails as "Spam" or "Not Spam" based on features:
- Number of exclamation marks
- Presence of words like "FREE", "URGENT"
- Email length

```
Training Data:
Email 1: [5 exclamation marks, 3 spam words, 50 words] → Spam
Email 2: [0 exclamation marks, 0 spam words, 200 words] → Not Spam
Email 3: [1 exclamation mark, 1 spam word, 150 words] → Not Spam
...

New Email: [2 exclamation marks, 1 spam word, 100 words] → ?
```

KNN would find the k most similar emails and classify based on their majority class.

---

## 4. Choosing the Right K Value {#choosing-k}

### The Impact of K:

#### Small K (e.g., k=1):
- **Pros**: Very sensitive to local patterns, can capture fine details
- **Cons**: **Noisy**, easily influenced by outliers
- **Risk**: **Overfitting** - too complex, memorizes training data

#### Large K (e.g., k=20):
- **Pros**: **Smoother** decision boundaries, less sensitive to noise
- **Cons**: May oversimplify, ignore local patterns
- **Risk**: **Underfitting** - too simple, misses important patterns

#### Optimal K:
- Usually an **odd number** (to avoid ties in voting)
- Found through **experimentation** and **validation**
- Balance between bias and variance

### Visual Impact of Different K Values:

```
k=1: Very jagged decision boundaries (captures noise)
k=5: Smoother boundaries (balanced)
k=20: Very smooth boundaries (may miss details)
```

---

## 5. Data Preprocessing for KNN {#data-preprocessing}

### Why Preprocessing is Critical for KNN

KNN relies heavily on **distance calculations**. If features are on different scales, those with larger values will dominate the distance calculation.

#### Example Problem:
```
Person 1: Age = 25 years, Income = $50,000
Person 2: Age = 30 years, Income = $51,000

Distance calculation without scaling:
√[(30-25)² + (51000-50000)²] = √[25 + 1,000,000] ≈ 1000

The income difference dominates completely!
```

### Solution: Standardization (Standard Scaling)

**Standard Scaling** transforms features to have:
- **Mean = 0**
- **Standard Deviation = 1**

**Formula**: `scaled_value = (original_value - mean) / standard_deviation`

#### After Scaling:
```
Person 1: Age = -0.5, Income = -0.5
Person 2: Age = 0.5, Income = 0.5

Distance = √[(-0.5-0.5)² + (-0.5-0.5)²] = √[1 + 1] = √2 ≈ 1.41
```

Now both features contribute equally to the distance!

---

## 6. Python Implementation {#python-implementation}

Let's implement KNN step by step using the classified dataset:

### Step 1: Import Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

# Machine Learning imports
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import classification_report, confusion_matrix
```

### Step 2: Load and Explore Data

```python
# Load the classified dataset
df = pd.read_csv('classified_data.csv', index_col=0)

# Check the data
print("Data shape:", df.shape)
print("\nFirst 5 rows:")
print(df.head())

# Check data info
print("\nDataset Info:")
print(df.info())

# Check target distribution
print("\nTarget class distribution:")
print(df['TARGET CLASS'].value_counts())
```

**Expected Output:**
```
Data shape: (1000, 11)

First 5 rows:
    WTT       PTI       EQW       SBI       LQE  ...  TARGET CLASS
0  0.913917  1.162073  0.567946  0.755464  0.780862  ...             1
1  0.635632  1.003722  0.535342  0.825645  0.924109  ...             1
2  0.721360  1.201493  0.921990  0.855595  0.526398  ...             1

Target class distribution:
1    500
0    500
```

### Step 3: Data Preprocessing (Standardization)

```python
# Separate features from target
X = df.drop('TARGET CLASS', axis=1)  # Features
y = df['TARGET CLASS']               # Target

# Initialize the StandardScaler
scaler = StandardScaler()

# Fit and transform the features
X_scaled = scaler.fit_transform(X)

# Convert back to DataFrame for better visualization
X_scaled_df = pd.DataFrame(X_scaled, columns=X.columns)

print("Original data (first 3 rows):")
print(X.head(3))
print("\nScaled data (first 3 rows):")
print(X_scaled_df.head(3))

print(f"\nOriginal data - Mean: {X.mean().mean():.2f}, Std: {X.std().mean():.2f}")
print(f"Scaled data - Mean: {X_scaled_df.mean().mean():.6f}, Std: {X_scaled_df.std().mean():.2f}")
```

**Expected Output:**
```
Original data - Mean: 0.87, Std: 0.31
Scaled data - Mean: 0.000000, Std: 1.00
```

### Step 4: Train-Test Split

```python
# Split the data
X_train, X_test, y_train, y_test = train_test_split(
    X_scaled, y, test_size=0.3, random_state=101
)

print(f"Training set size: {X_train.shape[0]} samples")
print(f"Testing set size: {X_test.shape[0]} samples")
```

### Step 5: Build and Train KNN Model

```python
# Create KNN classifier with k=1
knn = KNeighborsClassifier(n_neighbors=1)

# Train the model
knn.fit(X_train, y_train)

# Make predictions
predictions = knn.predict(X_test)

print("Model trained successfully!")
print(f"First 10 predictions: {predictions[:10]}")
print(f"First 10 actual values: {y_test.iloc[:10].values}")
```

### Step 6: Model Evaluation

```python
# Confusion Matrix
print("Confusion Matrix:")
print(confusion_matrix(y_test, predictions))

print("\nDetailed Classification Report:")
print(classification_report(y_test, predictions))
```

**Expected Output:**
```
Confusion Matrix:
[[142   8]
 [  7 143]]

                precision    recall  f1-score   support
           0       0.95      0.95      0.95       150
           1       0.95      0.95      0.95       150
    accuracy                           0.95       300
   macro avg       0.95      0.95      0.95       300
weighted avg       0.95      0.95      0.95       300
```

**Explanation of Results:**
- **Accuracy: 95%** - The model correctly classified 95% of test cases
- **Precision: 95%** - When model predicts class 1, it's correct 95% of the time
- **Recall: 95%** - The model correctly identifies 95% of actual class 1 cases
- **F1-Score: 95%** - Harmonic mean of precision and recall

---

## 7. The Elbow Method for Optimal K {#elbow-method}

The elbow method helps us find the optimal k value by testing multiple k values and plotting their error rates.

### Implementation:

```python
# Test different k values
error_rate = []
k_values = range(1, 40)

print("Testing k values from 1 to 39...")
print("This may take a moment...")

for k in k_values:
    # Create KNN model with current k
    knn_test = KNeighborsClassifier(n_neighbors=k)
    
    # Train the model
    knn_test.fit(X_train, y_train)
    
    # Make predictions
    pred_k = knn_test.predict(X_test)
    
    # Calculate error rate
    error = np.mean(pred_k != y_test)
    error_rate.append(error)
    
    # Progress indicator
    if k % 10 == 0:
        print(f"Tested k={k}, Error Rate: {error:.4f}")

print("Testing complete!")
```

### Visualizing the Results:

```python
# Create the elbow plot
plt.figure(figsize=(12, 6))
plt.plot(k_values, error_rate, color='blue', linestyle='--', 
         marker='o', markerfacecolor='red', markersize=6)
plt.title('Error Rate vs K Value (Elbow Method)', fontsize=14)
plt.xlabel('K Value', fontsize=12)
plt.ylabel('Error Rate', fontsize=12)
plt.grid(True, alpha=0.3)

# Find the k with minimum error
optimal_k = k_values[np.argmin(error_rate)]
min_error = min(error_rate)

plt.axvline(x=optimal_k, color='green', linestyle='-', alpha=0.7)
plt.text(optimal_k+1, min_error+0.005, f'Optimal K={optimal_k}\nError={min_error:.4f}', 
         bbox=dict(boxstyle="round,pad=0.3", facecolor="yellow", alpha=0.7))

plt.tight_layout()
plt.show()

print(f"Optimal K value: {optimal_k}")
print(f"Minimum error rate: {min_error:.4f}")
```

### Testing the Optimal K:

```python
# Use the optimal k value
knn_optimal = KNeighborsClassifier(n_neighbors=optimal_k)
knn_optimal.fit(X_train, y_train)
predictions_optimal = knn_optimal.predict(X_test)

print("Results with Optimal K:")
print("Confusion Matrix:")
print(confusion_matrix(y_test, predictions_optimal))
print("\nClassification Report:")
print(classification_report(y_test, predictions_optimal))

# Compare with k=1
k1_accuracy = np.mean(predictions == y_test)
optimal_accuracy = np.mean(predictions_optimal == y_test)

print(f"\nComparison:")
print(f"K=1 Accuracy: {k1_accuracy:.4f}")
print(f"K={optimal_k} Accuracy: {optimal_accuracy:.4f}")
print(f"Improvement: {(optimal_accuracy - k1_accuracy)*100:.2f}%")
```

---

## 8. Pros and Cons of KNN {#pros-and-cons}

### ✅ Advantages:

1. **Simple to Understand and Implement**
   - No complex mathematical concepts
   - Intuitive logic: similar things belong together

2. **No Training Period**
   - "Lazy learning" - just stores data
   - Fast to set up

3. **Works with Any Number of Classes**
   - Can handle binary, multi-class classification
   - Example: Classify movies into Comedy, Drama, Action, Horror, etc.

4. **No Assumptions About Data**
   - Doesn't assume linear relationships
   - Can capture complex patterns

5. **Easy to Add New Data**
   - Just add new points to the dataset
   - No need to retrain

6. **Few Parameters**
   - Only k and distance metric to tune
   - Less prone to parameter tuning issues

### ❌ Disadvantages:

1. **High Prediction Cost**
   - Must calculate distance to ALL training points
   - Slow for large datasets
   - Example: With 1 million training samples, each prediction requires 1 million distance calculations

2. **Poor Performance with High-Dimensional Data**
   - "Curse of dimensionality"
   - In high dimensions, all points seem equally distant
   - Example: With 100+ features, distance measurements become unreliable

3. **Sensitive to Irrelevant Features**
   - All features contribute to distance calculation
   - Noise features can mislead the algorithm

4. **Requires Feature Scaling**
   - Features on different scales can dominate
   - Preprocessing is mandatory

5. **Sensitive to Local Structure of Data**
   - Outliers can significantly impact predictions
   - Unbalanced datasets can bias results

6. **Memory Intensive**
   - Stores entire training dataset
   - Not suitable for memory-constrained environments

---

## 9. KNN vs Linear Regression vs Logistic Regression {#comparison}

Understanding when to use each algorithm is crucial for success in data science:

### 📊 Quick Comparison Table:

| Aspect | Linear Regression | Logistic Regression | KNN |
|--------|------------------|-------------------|-----|
| **Problem Type** | Continuous prediction | Binary/Multi-class classification | Classification/Regression |
| **Output** | Real numbers | Probabilities (0-1) | Classes/Categories |
| **Training** | Fast (learns parameters) | Fast (learns parameters) | No training (lazy) |
| **Prediction** | Very fast | Very fast | Slow (calculates distances) |
| **Assumptions** | Linear relationship | Linear decision boundary | None |
| **Interpretability** | High (coefficients) | High (coefficients) | Low (black box) |

### 🔍 Detailed Comparison:

#### 1. Linear Regression
**Purpose**: Predict continuous numerical values

**When to Use**:
- Predicting house prices based on size, location, age
- Estimating sales revenue based on advertising spend
- Forecasting temperature based on historical data

**Real-World Example**:
```python
# Predicting house price
# Input: [square_feet, bedrooms, age, location_score]
# Output: $245,000 (continuous value)

X = [[2000, 3, 5, 8.5]]  # 2000 sq ft, 3 bedrooms, 5 years old, good location
predicted_price = linear_model.predict(X)  # Output: $245,000
```

**Key Characteristics**:
- Assumes linear relationship between features and target
- Fast training and prediction
- Highly interpretable (can see feature importance)
- Output is any real number

#### 2. Logistic Regression
**Purpose**: Predict probabilities and classify into categories

**When to Use**:
- Email spam detection (Spam vs Not Spam)
- Medical diagnosis (Disease vs No Disease)
- Customer churn prediction (Will Leave vs Will Stay)
- Marketing response (Will Buy vs Won't Buy)

**Real-World Example**:
```python
# Email spam detection
# Input: [num_exclamation_marks, spam_words_count, email_length]
# Output: Probability and class

X = [[5, 3, 50]]  # 5 exclamation marks, 3 spam words, 50 words total
probability = logistic_model.predict_proba(X)  # Output: [0.15, 0.85] (15% not spam, 85% spam)
classification = logistic_model.predict(X)     # Output: 1 (Spam)
```

**Key Characteristics**:
- Assumes linear decision boundary
- Fast training and prediction
- Outputs probabilities (0-1 range)
- Highly interpretable
- Good for understanding feature impact

#### 3. k-Nearest Neighbors (KNN)
**Purpose**: Classify based on similarity to neighbors

**When to Use**:
- Recommendation systems (find similar users/products)
- Image recognition (similar images)
- Anomaly detection (unusual patterns)
- When data has complex, non-linear patterns
- When you have lots of data but don't understand the underlying relationships

**Real-World Example**:
```python
# Movie recommendation
# Input: [user_age, action_preference, comedy_preference, drama_preference]
# Output: Predicted rating

X = [[25, 0.8, 0.3, 0.6]]  # 25 years old, likes action, neutral on comedy/drama
# KNN finds 5 most similar users and averages their ratings for a movie
predicted_rating = knn_model.predict(X)  # Output: 4.2 stars
```

**Key Characteristics**:
- No assumptions about data relationships
- Can capture complex, non-linear patterns
- "Lazy learning" - no training phase
- Good for complex decision boundaries
- Less interpretable

### 🎯 Decision Guide: When to Choose What?

#### Choose Linear Regression When:
- ✅ You need to predict a **continuous number**
- ✅ You believe there's a **linear relationship**
- ✅ You need **fast predictions**
- ✅ **Interpretability** is important
- ✅ You have **limited data**

**Example**: Predicting salary based on years of experience

#### Choose Logistic Regression When:
- ✅ You need **classification** with **probabilities**
- ✅ You need to understand **which features matter most**
- ✅ You want **fast training and prediction**
- ✅ The decision boundary is relatively **simple/linear**
- ✅ You need to **explain predictions** to stakeholders

**Example**: Determining if a loan application should be approved

#### Choose KNN When:
- ✅ You have **complex, non-linear patterns**
- ✅ You have **lots of training data**
- ✅ **Similarity** is a meaningful concept in your domain
- ✅ You don't need to understand **why** the model makes decisions
- ✅ **Prediction speed** is not critical

**Example**: Recommending products based on user behavior patterns

### 🔄 Real-World Scenario Walkthrough:

**Scenario**: E-commerce Customer Analysis

**Problem 1**: Predict how much a customer will spend next month
- **Solution**: **Linear Regression**
- **Why**: Continuous value prediction, interpretable results for business

**Problem 2**: Predict if a customer will make a purchase (Yes/No)
- **Solution**: **Logistic Regression**
- **Why**: Binary classification, need probability scores for marketing campaigns

**Problem 3**: Recommend products to customers
- **Solution**: **KNN**
- **Why**: Find customers with similar preferences, complex behavioral patterns

---

## 10. Real-World Applications {#real-world-applications}

### 1. 🛒 Recommendation Systems

**Netflix Movie Recommendations**:
```python
# User profile: [action_movies, comedy_movies, drama_movies, sci_fi_movies]
user_profile = [0.9, 0.2, 0.7, 0.8]  # Loves action & sci-fi, some drama, little comedy

# KNN finds users with similar preferences
# Recommends movies that similar users enjoyed
```

**Why KNN Works Well**:
- Similar users tend to like similar content
- No need to understand complex preference algorithms
- Easy to add new users and movies

### 2. 🏥 Medical Diagnosis Support

**Symptom-Based Diagnosis**:
```python
# Patient symptoms: [fever, cough, fatigue, headache, nausea]
patient_symptoms = [1, 1, 0, 1, 0]  # Has fever, cough, headache

# KNN finds patients with similar symptom patterns
# Suggests most common diagnoses among similar cases
```

**Benefits**:
- Doctors can see similar historical cases
- Helps catch rare conditions by pattern matching
- Complements medical expertise with data

### 3. 🎯 Marketing and Customer Segmentation

**Customer Targeting**:
```python
# Customer profile: [age, income, shopping_frequency, online_preference]
customer = [35, 75000, 12, 0.8]  # 35 years old, $75k income, shops monthly, prefers online

# Find similar customers
# Target them with campaigns that worked for similar customers
```

### 4. 🔍 Fraud Detection

**Credit Card Fraud**:
```python
# Transaction features: [amount, time_of_day, merchant_type, location]
transaction = [850, 2.5, 1, 0]  # $850 at 2:30 AM at gas station, unusual location

# Compare to historical transactions
# Flag if similar patterns were fraudulent
```

### 5. 📸 Image Recognition

**Handwritten Digit Recognition**:
```python
# Each pixel becomes a feature
# New digit image compared to training images
# Classified based on most similar training examples
```

### 6. 🏠 Real Estate Valuation

**Property Price Estimation**:
```python
# Property features: [size, bedrooms, age, location_score, school_rating]
new_property = [2200, 4, 8, 9.2, 8.5]

# Find similar properties that sold recently
# Estimate price based on similar sales
```

---

## 🎯 Key Takeaways

### Remember These Core Concepts:

1. **KNN is Simple but Powerful**: Based on the intuitive idea that similar things belong together

2. **K Value is Critical**: 
   - Too small (k=1): Sensitive to noise
   - Too large: Over-smoothed, misses patterns
   - Use elbow method to find optimal k

3. **Preprocessing is Mandatory**: Always standardize your features for fair distance calculations

4. **Trade-offs Matter**:
   - Simple to understand ↔️ Slow predictions
   - No training needed ↔️ Stores all data
   - No assumptions ↔️ Less interpretable

5. **Choose the Right Tool**:
   - **Linear Regression**: Continuous predictions with linear relationships
   - **Logistic Regression**: Classification with interpretability
   - **KNN**: Complex patterns, similarity-based decisions

### 💡 Best Practices:

1. **Always scale your features** using StandardScaler
2. **Use cross-validation** to find optimal k
3. **Consider computational costs** for large datasets
4. **Remove irrelevant features** to improve performance
5. **Check for class imbalance** and handle accordingly

### 🚀 Next Steps:

1. Practice with different datasets
2. Experiment with different distance metrics
3. Try KNN for regression problems
4. Explore dimensionality reduction techniques
5. Learn about advanced neighbor-based algorithms

---

**Congratulations!** You've now mastered k-Nearest Neighbors algorithm. You understand when to use it, how to implement it, and how it compares to other fundamental machine learning algorithms. This knowledge forms a solid foundation for more advanced machine learning topics.

Remember: The best way to learn is by doing. Try implementing KNN on different datasets and see how changing k values and preprocessing steps affects your results!
