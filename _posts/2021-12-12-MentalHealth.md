---
title: Investigating Socioeconomic and Climate Factors of Mental Health Outcomes with Data
excerpt: >-
  In this project, we investigate the interactions between socioeconomic and climate factors and mental health outcomes. Specifically we look at associations and causal interactions of these factors with self-reported poor mental health days and suicide rates in U.S. counties.
date: '2021-12-12'
layout: post
---
Authors: Abhishek Babu, Rehaan Bhimani, Alan Liu, Michael Wilson

Below is an abridged version of our work intended for a more general data science audience. You can read our full work [here]({{site.baseurl}}/images/MentalHealth/final_report.pdf). Source code for our project can be found [here](https://github.com/bhimar/cse481ds-mental-health).

![image slices]({{site.baseurl}}/images/MentalHealth/mental_health_cover.jpg)

## Introduction
Mental health is a complex aspect of everybody’s well being. There are many factors involved, including socioeconomic and climate factors. In this study we investigate how these factors interact with mental health outcomes. Previous work [1] investigates a similar research question, but does so only with regression between these factors and suicide rates, an extreme manifestation of mental distress. Our contribution to the field includes extending regression analysis to also include predicting self-reported poor mental health days and introducing a causal matching analysis to draw stronger conclusions about the associations between our socioeconomic and climate features and the mental health outcomes.


## Methods
Our socioeconomic data comes from County Health Rankings [2], and the USDA [3]. Our climate data comes from NOAA [4]. Our mental health outcomes data comes from County Health Rankings [2] and CDC Wonder [5]. We gathered and filtered data for 5 years between 2011 and 2016 (excluding 2014) for U.S. counties. We split our final dataset into an 80/20 split for training our regression models.

### Regression Analysis
Our regression analysis included training 5 different models: Lasso, Ridge, LinearGAM, Random Forest, and Gradient Boosting. We trained each of these models by using 10-fold cross validation with the training set, and computing MAE, RMSE, and R^2 values for each training fold. We then averaged these metrics across all 10 folds to get the final training metrics for each model. The training metrics are presented below.

| Model Class | Avg. MAE Poor Mental Health Days| Avg. RMSE Poor Mental Health Days | Avg. R^2 Poor Mental Health Days | Avg. MAE Suicide Rate | Avg. RMSE Suicide Rate | Avg. R^2 Suicide Rate |
| ----------- | -------- | --------- | -------- | -------- | --------- | -------- |
| Lasso Regression | 0.439 | 0.575 | 0.246 | 5.118 | 7.121 | 0.108 |
| Ridge Regression | 0.402 | 0.538 | 0.339 | 4.201 | 5.731 | 0.420 |
| LinearGAM Regression | 0.376 | 0.508 | 0.411 | 3.868 | 5.287 | 0.506 |
| **Gradient Boosting Regression** | 0.370 | 0.501 | 0.426 | 3.844 | 5.220 | 0.518 |
| **Random Forest Regression** | 0.365 | 0.497 | 0.437 | 3.865 | 5.377 | 0.491 |
| Null Regression | 0.509 | 0.663 | 0 | 5.544 | 7.583 | 0 |

We found that Random Forest was the most effective model for predicting Poor Mental Health Days, and that Gradient Boosting was the most effective model for predicting Suicide Rates. We then trained these respective model classes with the entire training set and computed the same statistics using the test set for evaluation. The results from our model evaluation are presented below.

| Model Class | Outcome | Average MAE | Average RMSE | Average R^2 |
| ----------- | ------- | ----------- | ------------ | ----------- |
| Random Forest | Poor Mental Health Days | 0.359 | 0.477 | 0.437 |
| Gradient Boosting | Suicide Rate | 3.827 | 5.238 | 0.446 |



### Matching Analysis

For each of the variables we were interested in, we split the data into two groups: a treatment group and a control group. The treatment group represented the counties that were less likely to have poor mental health based on what we know about factors like income and education in relation to mental health outcomes. The control group represented the more high-risk counties. Then, for each county in the treatment group, we tried to find the closest county in the control group such that the only major difference between the counties was the chosen treatment variable. When doing this matching, we want to make sure that the matched counties are similar in all other variables. For this we use the Standardized Mean Difference (SMD) [6] to ensure that the groups are comparable. Then, by examining the differences in the Poor Mental Health Days and Suicide Rates across those two counties, we can estimate the effect that the difference in the treatment variable had on the mental health outcomes since all the other variables were as close as we could find.

| Treatment | Treated | Max SMD | Mean SMD | Treated Matched | Treated  Unmatched | Control  Matched | Control Unmatched |
| --------- | ------- | ------- | -------- | --------------- | ------------------ | ---------------- | ----------------- |
|High school grad. | Higher | 0.106 | 0.0605 | 2353 | 1 | 1056 | 1031 |
|Some college | Higher | 0.214 | 0.132 | 2213 | 0 | 742 | 1486 |
|Household income | Higher | 0.297 | 0.123 | 2026 | 0 | 732 | 1683 |
|Unemployment | Lower | 0.0990 | 0.0459 | 2353 | 0 | 967 | 1121 |
|Pop. to MH providers | Lower | 0.3590 | 0.180 | 3659 | 124 | 527 | 131 |
|Temperature | Lower | 0.1048 | 0.0508 | 2281 | 0 | 1092 | 1068 |
|Precipitation | Lower | 0.0848 | 0.0250 | 2240 | 0 | 1146 | 1055 |
|RUCC | Lower | 0.1914 | 0.139 | 2544 | 0 | 778 | 1119 |


## Results
By looking at the feature importance and partial dependence for our final models, we found the more important features for predicting each of the mental health outcomes. We found that for predicting Poor Mental Health Days, income and college education were the most significant features. For predicting Suicide Rates, the level of urbanization was the most significant feature by far.

From our matching, we estimated the effects of each treatment on both Poor Mental Health Days and Suicide Rates. We found that counties with higher college attendance exhibited fewer Poor Mental Health Days and lower Suicide Rates. We also found that counties with higher income had fewer Poor Mental Health Days. Interestingly, we also found that counties with lower unemployment had higher suicide rates.

## Discussion
By combining the results from our regression and matching analysis, we can paint a clearer picture of mental health.

Both our regression analysis and matching analysis indicate that higher college attendance and income is associated with lower levels of poor mental health days. We also see from our matching analysis that higher college attendance leads to lower suicide rates. We believe that this is the case because secondary education is more area-focused and intensive, contributing to individuals’ abilities to find jobs and cultivating their sense of meaning in life. Notably though, we see a lower unemployment rate corresponding to higher suicide rates in our matching analysis. This suggests that stresses engendered by jobs could exceed anxiety resulting from unemployment, but this requires further research.

In the case of urbanization, our regression analysis finds that rural communities are more likely to have higher suicide rates. This is consistent with our causal inference analysis which indicates that urban communities have lower suicide rates but higher levels of poor mental health days. A possible explanation might be that the fast-paced lifestyle in urban counties causes their residents to be more anxious, but also lessens individuals’ thoughts about suicide due to the business nature. In the case of mental health resources, we see from the matching analysis that areas with more mental health providers have lower suicide rates and higher levels of poor mental health days.Such a relationship can either be a direct effect of government interventions to establish more mental health resources in areas of poor mental health or that the ease of accessing providers tends to increase individuals’ incentive to seek such help, resulting in reporting more poor mental health days. However, such resources effectively controls people’s mental health conditions and thus reduce their likelihood and tendency of committing suicide

We hope that these findings can guide further research, policy, and programs for addressing mental health issues in the United States.

## References
[1] S. Mukherjee and Z. Wei. Suicide disparities across metropolitan areas in the US: A comparativeassessment of socio-environmental factors using a data-driven predictive approach.PLOS One,16(11), Nov 2021.  
[2] County   Health   Rankings.National   Data   |   Documentation:2010-2019,    2019.  
[3] U.S. Department  of  Agriculture  (USDA)  Economic  Research  Service  (ERS).Rural-Urban Continuum Codes, Dec 2020.  
[4] National Oceanic and Atmospheric Administration (NOAA) National Centers for Environment Information. Climate at a Glance: County Time Series, Dec 2021.  
[5] Centers for Disease Control and Prevention (CDC). CDC WONDER, Dec 2021.  
[6] E. A. Stuart, B. K. Lee, and F. P. Leacy. Prognostic score–based balance measures for propensityscore methods in comparative effectiveness research.Journal of Clinical Epidemiology, 66(8):S84–S90, 2013.