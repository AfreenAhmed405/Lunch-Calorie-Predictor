# Multimodal Nutrition Intake Estimation

> This project proposes a novel multimodal machine learning approach to accurately estimate caloric intake by integrating continuous glucose monitoring (CGM) data, meal images, and demographic plus microbiome (Viome) data. The goal is to improve dietary tracking accuracy for chronic disease management and personalized nutrition.

---

## Table of Contents

- [Project Overview](#project-overview)  
- [Dataset](#dataset)  
- [Methodology](#methodology)  
  - [Data Preprocessing](#data-preprocessing)  
  - [Feature Engineering](#feature-engineering)  
  - [Model Architecture](#model-architecture)   
- [Results](#results)  

---

## Project Overview

Accurate dietary intake tracking is essential for managing health conditions such as diabetes and obesity. Traditional methods are often inaccurate or cumbersome. This project designs a multimodal model, **MultimodalNet**, that combines:

- Continuous Glucose Monitoring (CGM) time-series data  
- Meal photographs for food and portion size estimation  
- Participant demographic and microbiome data  

to predict caloric intake automatically and more accurately. This integration allows real-time, personalized dietary assessment to aid healthcare providers in nutrition recommendations.

---

## Dataset

The dataset consists of data from over 40 participants spanning up to 9 days each, split into training and testing sets with separate CSV files for each modality:

- **CGM Data**: 5-minute interval glucose readings capturing glycemic response. Missing data handled using the MAGE algorithm.  
- **Food Image Data**: Photos of breakfast and lunch meals, resized and normalized. Missing images replaced with black placeholders.  
- **Demographic & Viome Data**: Participant info (age, gender, diabetes status) and microbiome features, one-hot encoded and imputed for missing values.  
- **Meal Calorie Labels**: Ground truth caloric values for lunch meals used as prediction targets.

---

## Methodology

### Data Preprocessing

- **Images** resized to 64x64 pixels and normalized to [0, 1].  
- **CGM data** cleaned by removing incomplete records, missing values predicted using MAGE, padded for consistent input length.  
- **Demographic and Viome data** split into individual features, categorical variables one-hot encoded, and missing values imputed.

### Feature Engineering

- Principal Component Analysis (PCA) applied on Viome features to reduce dimensionality from 28 to 22 while preserving 90% of variance.

### Model Architecture - MultimodalNet

- **Image Encoder**: CNN with 2 convolutional + max-pooling layers, followed by a fully connected layer producing 256-dimensional feature vectors for breakfast and lunch images.  
- **CGM Encoder**: Two fully connected layers processing 288 time-step input down to a 64-dimensional vector.  
- **Demo-Viome Encoder**: Fully connected network reducing input to a 32-dimensional feature vector.  
- **Fusion Layer**: Concatenates all modality features and passes through fully connected layers to output a single calorie estimate.

### Training

- Model trained over 50 epochs using appropriate loss functions and optimizers to balance accuracy and avoid overfitting.

---

## Results

### Model Performance Metrics

The model was evaluated on a test dataset consisting of data from 9 subjects. The primary performance metric used was the Root Mean Squared Relative Error (RMSRE), with the model achieving a final training loss of **0.3339**. This indicates a reasonable degree of accuracy in predicting lunch calorie intake.

### Discussion

- **Interpretation of Results:**  
  The multimodal neural network effectively combines CGM data, meal images, and demographic information to estimate caloric intake with promising accuracy. The achieved training loss suggests that the model has successfully learned patterns linking these modalities to calorie values.

- **Limitations:**  
  - *Limited Dataset Size:* With only 40 participants, each contributing up to 10 days of data, the dataset size restricts the model’s generalizability and may increase overfitting risk.  
  - *Potential Overfitting:* The complex multimodal architecture, while powerful, may be prone to overfitting due to limited data.  
  - *Dependency on Multiple Data Sources:* Reliance on three distinct modalities (CGM, images, demographics) makes the model sensitive to data availability and quality, which could challenge deployment in real-world scenarios.

- **Potential Applications:**  
  - Personalized nutrition planning tailored to individual metabolic responses.  
  - Improved diabetes management through accurate calorie prediction and glucose regulation support.  
  - Enhanced weight management strategies with precise caloric intake estimation.  
  - Contribution to nutritional science research by providing deeper insights into the interaction between meal composition, individual characteristics, and caloric intake.

---

