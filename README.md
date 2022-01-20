# Analysis of the Telescope Gamma Dataset

## Overview of the repository
- **./report.pdf :** the report of the analysis
- **./visualization.ipynb:** it contains the code for the analysis of the dataset 
- **./processing_model.ipynb:** it contains the code for running the experiments and the two best tuned model pipelines
- **./curves.ipynb:** it contains the code for the creation of the learning curves and the roc curves 
- **./images/\* :** all the images used for the report
- **./images/outliers_experiments/\* :** the confusion matrices and roc curves of the experiments for each model
-  **./images/outliers_experiments/results_experiment.pdf :** The experiment results in a compact format

## Dataset 
The data present in this dataset are generated to simulate registration of high energy gamma particles in a ground-basedatmospheric.Cherenkov gamma telescope (CTA) observes high energy gamma rays, taking advantage of the radiation emitted by chargedparticles produced inside the electro-magnetic showers.

Depending on the energy of the primary gamma, a total of few hundreds to some 10000 Cherenkov photons get collected, inpatterns (called the shower image), allowing to discriminate statistically those caused by primary gammas (signal) from theimages of hadronic showers initiated by cosmic rays in the upper atmosphere (background).

The goal of the classification is to correctly classify the gamma signal (g) from hadron (h, background).

The dataset is composed of **19020** distinct records with **11** attributes and does not contain missing values.

|Attribute |Type |Description
|----------|------|---------
|fLength | Real | major axis of ellipse 
|fWidth | Real |minor axis of ellipse |
|fSize  | Real | 10-log of sum of content of all pixels
|fConc | Real | ratio of sum of two highest pixels over fSize
|fConc1 | Real | ratio of highest pixel over fSize
|fAsym | Real | distance from highest pixel to center, projected onto major axis 
|fM3Long | Real | 3rd root of third moment along major axis 
|fM3Trans | Real | 3rd root of third moment along minor axis 
|fAlpha | Real | angle of major axis with vector to origin 
|fDist | Real | distance from origin to center of ellipse 
|class  | Categorical | g gamma (signal), h hadron (background)

The Dataset is clearly imbalance since there are **12332 (65%)** samples labelled with Gamma and **6688 (35%)** labelled as Hadron.

## Training Pipeline
1. Local Outlier Factor 
2. Stratified Training/Test split
3. Robust Scaler
4. SMOTE / BorderLineSMOTE
5. Stratified Cross Validation
6. Random Grid Search

## Experiments 
Multiple experiments have been performed to truly understand the importance of each step in the training pipeline. More precisely, the analyzed steps are the **Outliers Removal**, the application of the **Scaler** and the type of **Up-sampling**. 
| Ablation | Parameter
|-----------|----------
|LocalOutlierFactor | [2, 3,5,10,15,20,False]
|Scaler | [True, False]
|Up-Sampling | ["BorderLineSMOTE", "SMOTE"]

To running the experiments, a simple nested loops is used in order to cover all the possibilities and then, for each model, is used a random grid search.

