---
title: "Underfitting and Overfitting: How Models Go Wrong"
date: 2025-12-06
tags: ["foundations", "ml-basics", "generalisation"]
draft: true
---

Model $(X,Y) = x + y \frac{a}{b} \mathcal{XYD}$

When we train a model, we want it to **generalise**: perform well not only on the data it has seen, but also on new, unseen data.

Two of the most common ways this can go wrong are:

- **Underfitting** – the model is too simple to capture the underlying pattern  
- **Overfitting** – the model fits the training data too closely, including noise, and fails to generalise  

Understanding these two behaviours, and how to recognise them in practice, is one of the most important skills in machine learning.

---

## 1. What do we actually want from a model?

Given some data, we usually split it into:

- **Training set** – used to fit the model  
- **Validation set** – used to tune choices (hyperparameters, model complexity, etc.)  
- (Optionally) **Test set** – used only at the very end to estimate final performance  

A “good” model is one that:

- has **reasonable error on the training set**, and  
- has **similar and low error on the validation set**

If training error is high → the model cannot even fit the data it sees → likely **underfitting**.  
If training error is very low but validation error is high → the model memorised the training data → likely **overfitting**.

---

## 2. Underfitting: the model is too simple

**Underfitting** happens when the model cannot capture the structure of the data, no matter how long you train it.

Intuitively:

- Think of fitting a **straight line** through points that clearly follow a curve.  
- No matter how you place the line, it will always be far away from many points.

Typical signs:

- High error on the **training set**  
- Similar high error on the **validation set**  
- The model makes **systematic mistakes** (clear patterns in the residuals)

Common causes:

- Model is too simple (e.g. linear model for a highly non-linear problem)  
- Too few features or features that do not carry enough information  
- Too strong regularisation (you forced the model to be overly constrained)  
- Too little training (early stopping too early, especially for complex models)

How it feels in practice:

> No matter what you try with hyperparameters, both training and validation performance stay bad.

---

## 3. Overfitting: the model memorises instead of generalising

**Overfitting** happens when the model fits the training data too closely, including noise and idiosyncrasies, but fails to generalise.

Intuitively:

- Think of fitting a **very wiggly polynomial** that passes exactly through every training point.  
- On the training data it looks perfect, but small changes or new points reveal that it does not capture a stable pattern.

Typical signs:

- **Very low error** (or very high accuracy) on the **training set**  
- **Much worse performance** on the **validation set**  
- As you increase model complexity, training error keeps decreasing, but validation error starts increasing

Common causes:

- Model is too complex for the amount of data (many parameters, few samples)  
- Insufficient regularisation  
- Training for too long without proper early stopping or monitoring  
- Data leakage or target leakage (model secretly learning shortcuts specific to training data)

How it feels in practice:

> The model looks “amazing” on the training data, but as soon as you evaluate on new data it breaks.

---

## 4. Bias, variance and the trade-off

Underfitting and overfitting are often described in terms of **bias** and **variance**:

- **High bias** → model is too rigid; tends to underfit  
- **High variance** → model is too sensitive to the specific data; tends to overfit  

In very rough terms:

- Underfitting ≈ **high bias, low variance**  
- Overfitting ≈ **low bias, high variance**

Good models find a **balance**: low enough bias to capture structure, and low enough variance to be robust to noise.

You do not need to compute bias and variance explicitly to understand this trade-off; it is mainly a way to think about how model complexity and regularisation affect behaviour.

---

## 5. How to detect underfitting and overfitting in practice

### 5.1 Compare training and validation performance

A simple but powerful diagnostic:

- Compute an error metric (or accuracy) on:
  - training set  
  - validation set  

Then look at:

1. **Training error high, validation error high**  
   - Model is struggling on both → likely **underfitting**.  

2. **Training error low, validation error high**  
   - Model fits training well but fails on validation → likely **overfitting**.  

3. **Training error moderate/low, validation error similar and low**  
   - Model generalises reasonably well.

### 5.2 Learning curves

Another useful tool is to plot **learning curves**:

- Fix a model and hyperparameters  
- Train it on increasingly larger subsets of the training data (e.g. 10%, 20%, 40%, …)  
- For each subset, measure training and validation error  

Patterns:

- **Underfitting**:
  - Training error stays **high** even when using all data  
  - Validation error is also high and close to training error  

- **Overfitting**:
  - Training error is **low** and decreases as you add more data  
  - Validation error is much higher and may not improve (or may even get worse)  

Learning curves help you decide whether collecting more data will help and whether you should adjust model complexity.

---

## 6. How to fix underfitting

If you suspect **underfitting**, possible actions include:

- **Use a more flexible model**  
  - e.g. move from linear to polynomial features; from a shallow tree to a deeper tree; from a tiny neural network to a slightly larger one  

- **Add more informative features**  
  - feature engineering for tabular data  
  - using better representations for text, images, etc.

- **Reduce regularisation strength**  
  - decrease L2/L1 penalty, dropout rate, or other constraints  
  - allow the model more freedom to fit the data  

- **Train for longer** (when using models that need substantial training)  
  - in neural nets, sometimes the model is simply undertrained  

The key idea: **give the model more capacity or signal**, but keep an eye on the validation set to avoid swinging to overfitting.

---

## 7. How to fix overfitting

If you suspect **overfitting**, typical strategies are:

- **Get more data**  
  - When possible, this is usually the most effective remedy  

- **Simplify the model**  
  - reduce depth or width of a network  
  - prune a tree  
  - reduce polynomial degree  

- **Increase regularisation**  
  - stronger L2/L1 penalties  
  - increase dropout in neural networks  
  - add data augmentation for images, text, etc.

- **Use early stopping**  
  - monitor validation loss during training and stop when it stops improving  

- **Improve the validation strategy**  
  - use cross-validation, especially on smaller datasets  
  - check for data leakage (features that “peek” at the target)  

Here the key idea is: **reduce the effective capacity of the model or make it less sensitive to noise**, while maintaining enough flexibility to capture the true pattern.

---

## 8. A mental checklist

When a model is not performing as expected, you can go through a quick checklist:

1. **Is training performance already poor?**  
   - Yes → suspect **underfitting**  
   - No → check validation performance  

2. **Is validation performance much worse than training?**  
   - Yes → suspect **overfitting**  

3. **Could there be data leakage or split issues?**  
   - Same user/transaction appearing in both train and validation?  
   - Features derived from the target?  

4. If underfitting:  
   - try a more flexible model  
   - add features  
   - reduce regularisation  

5. If overfitting:  
   - add data (or augment it)  
   - simplify the model  
   - increase regularisation  
   - use early stopping  

Overfitting and underfitting are not abstract textbook concepts; they show up **every time you train a model**. Being able to recognise them quickly and react appropriately is a core practical skill in machine learning.