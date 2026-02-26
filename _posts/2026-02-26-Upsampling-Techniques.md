---
title: "Upsampling Techniques"
date: 2026-02-26
layout: post
categories: [Data Science]
tags: [Feature Engineering]
math: true
---

# Understanding Oversampling in Machine Learning

### Random Oversampling, SMOTE, and Borderline-SMOTE

## 1. What Is Oversampling?

In many real-world classification problems, the dataset is imbalanced. One class (often the one we care about most) appears far less frequently than the other.

Examples:

* Credit default prediction (few defaults)
* Fraud detection (rare fraud cases)
* Disease diagnosis (few positive cases)

When the dataset is highly imbalanced, most machine learning models tend to favor the majority class. As a result:

* The decision boundary shifts toward the minority class
* Minority predictions are often ignored
* Accuracy may look high, but recall for the minority class is poor

Oversampling addresses this issue by **increasing the number of minority class samples**, helping the model learn a better representation of that class.

---

## 2. Random Oversampling

Random Oversampling is the simplest approach.

### 2.1 How it works:

* Randomly duplicate existing minority samples
* Continue until class balance is improved

### 2.2 Advantages:

* Very easy to implement
* Works well as a quick baseline

### 2.3 Limitations:

* Simply replicates the same data
* Can increase overfitting
* Does not expand the minority decision region

Because duplicated samples contain no new information, the model may memorize them rather than generalize.

---

## 3. SMOTE (Synthetic Minority Over-sampling Technique)

SMOTE improves upon random oversampling by **creating synthetic minority samples instead of duplicating existing ones**.

Instead of copying data points, SMOTE generates new samples between existing minority samples.

---

### 3.1 Core Idea

For a given minority sample:

1. Select one of its k-nearest minority neighbors
2. Create a new synthetic sample between the two

This is done using linear interpolation.

---

### 3.2 Mathematical Formulation

Let:

* ( $ x_i $ ) = a minority sample
* ( $ x_{nn} $ ) = one of its nearest minority neighbors
* ( $ \lambda \sim U(0,1) $ )

The synthetic sample is generated as:  

$$
x_{new} = x_i + \lambda (x_{nn} - x_i)
$$

This ensures that the new sample lies somewhere between the two minority points.

---

### 3.3 Geometric Interpretation

In a 2D space, suppose:

* Point A = (2, 3)
* Point B = (4, 7)

If ($\lambda = 0.3$), then:

![SMOTE interpolation example](/assets/images/smote-example.png)

The new point lies between A and B.

Instead of stacking identical points, SMOTE **thickens the minority region in feature space**, allowing the classifier to form a broader and more stable decision boundary.

---

### 3.4 Why SMOTE Is Better Than Random Oversampling

* Reduces overfitting compared to duplication
* Expands the minority decision region
* Encourages smoother decision boundaries

---

### 3.5 Limitations of SMOTE

1. Class Overlap Risk
   Synthetic samples may be created in regions where majority and minority classes overlap.

2. Noise Amplification
   If a minority outlier is selected, synthetic samples may be generated in incorrect regions.

3. Distance Sensitivity in High Dimensions
   Since SMOTE relies on k-nearest neighbors, it can suffer in high-dimensional feature spaces.

---

# 4. Borderline-SMOTE

## 4.1 Core Idea

Standard SMOTE generates synthetic samples uniformly across all minority samples.

However, not all minority samples contribute equally to classification performance.

* Minority samples deep inside their cluster are already well learned.
* The most difficult region for the classifier lies near the decision boundary.
* Most misclassifications occur where minority and majority classes overlap.

Borderline-SMOTE focuses on strengthening the minority class specifically in those critical boundary regions.

---

## 4.2 Safe, Danger, Noise: How Samples Are Categorized

Borderline-SMOTE first analyzes the local neighborhood of each minority sample.

For each minority point:

1. Find its **k-nearest neighbors** (including both minority and majority).
2. Count how many neighbors belong to the majority class.

Let:

* k = number of neighbors
* m = number of majority neighbors

Then each minority sample is categorized as:

### Safe

If most neighbors are minority:  

$$
m < \frac{k}{2}
$$

Interpretation:

* The sample lies inside a dense minority region.
* It is unlikely to be misclassified.
* No need to reinforce this region.

These samples are **ignored**.

---

### Danger

If roughly half or more neighbors are majority:

$$
\frac{k}{2} \le m < k
$$

Interpretation:

* The sample lies near the class boundary.
* It is at risk of misclassification.
* This is where the classifier struggles most.

These samples are **used to generate synthetic data**.

---

### Noise

If almost all neighbors are majority:

$$
m = k
$$

Interpretation:

* The minority sample is isolated inside a majority region.
* It may be an outlier or mislabeled.
* Generating synthetic samples around it would expand minority points into majority territory.

These samples are **discarded**.

Important:
Noise does not necessarily mean incorrect label.
It means the point is locally surrounded by majority samples and is structurally unstable for interpolation.

---

## 4.3 Why Only Use Danger Samples?

The intuition is strategic.

* Safe samples → Already well represented.
* Noise samples → Risk expanding minority into majority regions.
* Danger samples → Exactly where the decision boundary needs reinforcement.

Borderline-SMOTE concentrates synthetic generation on the region where the model is weakest.

Instead of thickening the entire minority cluster (like SMOTE),
it strengthens the “front line” of the minority class.

This often leads to:

* Improved recall
* Better boundary shaping
* Less unnecessary synthetic expansion

---

## 4.4 Algorithm Flow

1. For each minority sample:

   * Find k-nearest neighbors.

2. Count majority neighbors.

3. Categorize each minority sample into:

   * Safe
   * Danger
   * Noise

4. For each Danger sample:

   * Randomly select one of its minority neighbors.
   * Generate synthetic sample using SMOTE interpolation:

$$
x_{new} = x_i + \lambda (x_{nn} - x_i)
$$

5. Repeat until desired class balance is reached.

The key difference from SMOTE:

* SMOTE uses all minority samples.
* Borderline-SMOTE uses only boundary minority samples.

---

## 4.5 Advantages

### Focused Boundary Reinforcement

Strengthens the decision boundary rather than expanding the entire minority cluster.

### More Informative Synthetic Samples

Synthetic points are generated where classification is difficult.

### Often Improves Recall

Particularly effective in fraud detection, credit default modeling, and medical diagnosis.

---

## 4.6 Limitations

### Still k-NN Based

Sensitive to distance metric and curse of dimensionality.

### Sensitive to k Choice

Too small k → unstable classification of danger/noise
Too large k → boundary becomes blurred.

### Noise Amplification Risk

If boundary points are mislabeled, synthetic generation may reinforce incorrect structure.

### Does Not Fully Solve Class Overlap

If overlap is extreme, synthetic interpolation may still create ambiguous samples.

---

## 4.7 Conceptual Summary

* SMOTE expands the minority region.
* Borderline-SMOTE reinforces the minority boundary.
* The goal is not just balance, but **structural improvement of the decision surface**.

---

## 5. Summary Comparison

| Method              | Strategy                       | Risk of Overfitting | Boundary Improvement |
| ------------------- | ------------------------------ | ------------------- | -------------------- |
| Random Oversampling | Duplicate samples              | High                | No                   |
| SMOTE               | Interpolation                  | Moderate            | Yes                  |
| Borderline-SMOTE    | Boundary-focused interpolation | Lower               | Strong               |

---