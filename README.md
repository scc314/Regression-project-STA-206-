					Analysis of Plasma Dataset



Introduction

1.2. Description of Dataset

The dataset for the following analysis is the Plasma dataset. The dataset was obtained to investigate the possible relationship between lifestyle and dietary decisions and the concentration of beta carotene and retinol in Plasma, given that most prior studies have only investigated associations of those two and cancer. The study involved 315 patients who underwent an elective surgical procedure to remove a lesion from a part of their body. For each patient, the lesion tissue was analyzed, and the patient’s dietary and lifestyle choices were noted to obtain the overall data. There are 14 variables in the  dataset, 3 of which are categorical. 

1.2. Objective

Our goal is to analyze this dataset for potential relationships between each of our response variables to the provided predictors. The response variables will be the plasma concentration of β-carotene and the plasma concentration of Retinol. The following analysis will look to identify the variables that may be optimal predictors for each one of them.
	

Exploratory Data Analysis

2.1. Data Distributions & Transformations

We first plot a histogram of numerical variables and pie charts of the categorical variables. For numerical variables, we see a right skew for the distributions of RETDIET, QUETELET, and BETADIET.  Furthermore, the distributions for BETAPLASMA and ALCOHOL are decaying distributions. As a significant number of patients do not drink alcohol, this would make sense. All other variables seem to be approximately normal.


To remedy this, we take the log of each variable. Making sure to add 1 to the observation value before taking the log to account for values that may be 0 (e.g, when a person does not drink alcohol). The figure below shows the log-transformed distributions of those variables. While the distribution of most of those variables appears to be more normal now, the distribution for ALCOHOL still shows a strong right skew.


When looking at the categorical variables, we notice a fairly normal class distribution for the categorical variables for smoking status (SMOKSTAT) and vitamin A supplementation (VITUSE). However, we notice that for the sex variable (SEX), it appears that 87% of the patients are female. Because of this bias in this variable, we choose to narrow the scope of our subsequent analysis to look at females only and exclude any observations for males.




To make a more meaningful interpretation, we also chose to standardize certain dietary-related variables. Specifically, we standardize the variables FIBER, CHOLESTEROL, and FAT using the CALORIES variable. The transformed variables for CHOLESTEROL and FIBER represent the ratio of each of those nutrients to the patient’s total daily caloric intake. Furthermore, for the FAT variable, we multiply this proportion by 9, as there are 9 calories per gram of fat. So the final variable for fat represents the percentage of calories consumed in fat. We apply these transformations specifically:

CholesterolCali =  CHOLESTEROLi / CALORIESi
CaloriesFati =  9  x FATi / CALORIESi
FiberCali =  FIBERi / CALORIESi


Filtering out observations for male patients and performing the aforementioned transformation yields the following transformation for the new variables.







	2.2. Scatterplot and Correlation Matrix

We now obtain the scatterplot matrix and box plots of the resulting variables. We exclude the original version of the transformed variables, as these would be highly collinear with the transformed variables and consequently would increase sampling variability if included in the models. 




Looking at the scatterplot matrix, there are no strong correlations among the predictor variables(i.e, variables that aren’t retinol plasma and log of beta-carotene plasma). Although it may be beneficial to explore the inclusion of interaction terms for each pair of variables that were standardized with the calories variable. This is due to the relatively higher correlation coefficients among these specific variables. With a correlation coefficient of -0.53 between FiberCal and CaloriesFat, 0.33 between CholesterolCal and CaloriesFat, and -0.2 between FiberCal and CholesterolCal. These correlations could be accounted for with the inclusion of an interaction term. 





Next, we compare each of the plasma concentration response variables to the different predictors. For beta carotene plasma concentrations, we see relatively high correlations with dietary beta carotene intake, fiber-to-calories ratio, percent calories consumed in fat, Quetelet, and age. There’s also a slight positive correlation with categorical variables like vitamin A use and smoking status. 








For retinol plasma concentrations, there are fewer predictors with any significant correlations. Specifically, there’s a relatively higher correlation with percent calories consumed in fat and with age. In subsequent sections, we use stepwise regression to fit the optimal model for each of the plasma concentration variables.



3. Model Fitting for Plasma Beta-Cerotine


    3.1 Variables and Model Selection

To find a suitable linear model that represents the relationship between the dependent variable of plasma beta-carotene and the independent variables, a  ‘greedy search’ approach was taken due to the moderate number of variables in the dataset. As discussed in the previous section, logarithmic transformations were performed on the dependent variable plasma beta-carotene, as well as the independent variables dietary retinol consumed, dietary beta-carotene consumed, alcoholic drinks consumed per week, and the subject’s BMI/Quetlet measurements to better normalize their distributions. Additionally, the variables measuring fat, cholesterol, and fiber were transformed by dividing their original measurements by the subject’s caloric intake, which better reflects their dietary habits and substantially reduces correlation with the caloric intake variable. The variable measuring plasma retinol was not included due to its use as a dependent variable in a separate analysis.

The procedure decided upon to arrive at our final model was a stepwise regression search followed by removing variables with a p-value greater than 0.05. Both a forward and a backward stepwise procedure were used, the former starting with a null model with only the intercept, and the latter beginning with the full model. In each pass, the stepwise procedure will add or remove a variable to minimize the Akaike Information Criterion (AIC), a measurement for balancing model bias and variance. The purpose of running two stepwise procedures starting from different ‘ends’ is to reduce the likelihood of arriving at a local minimum, a pitfall of the stepwise procedure. If the two stepwise searches arrive at different models, we can select the model with the lower AIC. The purpose of this analysis is to learn more about the relationship between lifestyle factors and plasma beta-carotene as opposed to prediction, so we will be excluding potentially statistically insignificant variables with a p-value greater than 0.05 from our final model if any exist after the stepwise procedure.

Null Model:
 log(Plasma Beta Carotene + 1) = B0 + ε

Full Model:
log(Plasma Beta Caroteneng/ml + 1) = B0 + B1(Age) + B2(Former Smoker) + B3(Never Smoked) + B4(Infrequent Vitamin Use) + B5(Frequent Vitamin Use)  + B6(Calories) + B7(log(Dietary Retinolmcg + 1)) + B8(log(Quetelet + 1)) + B9(log(Dietary Beta Carotenemcg + 1)) + B10(log(Weekly Alcoholic Drinks + 1)) + B11(9*Fatg/Calories) + B12Fiberg/Calories + B13(Cholesterolmg/Calories) + ε


3.2 Stepwise Results

Both the forward and backward stepwise procedures yielded the same model, minimizing AIC at -228.3. The variables selected for the model were the following: age, smoke status, vitamin use, quetelet, dietary beta carotene, alcohol consumption, and fiber consumption, with calories, dietary retinol, fat consumption, and cholesterol consumption excluded. The intercept, coefficients, t values, and rejection coefficients can be seen in table [number here].




Estimated Coefficients
Standard Error
t value
pr(>|t|)
(Intercept)
5.975304
0.787741
7.585
5.79e-13
Age
0.005496
0.002924
1.880
0.06128
Former Smoker
0.268962
0.131605
2.044
0.04198
Never Smoked
0.369977
0.127026
2.913
0.00389
Infrequent Vitamin Use
0.223249
0.101681
2.196
0.02900
Frequent Vitamin Use
0.244807
0.097045
2.523
0.01224
log(Quetelet)
-0.964101
0.199594
-4.830
2.32e-06
log(Dietary- Beta Carotenemcg)
0.156721
0.061994
2.528
0.01206
log(Alcohol)
0.079343
0.049179
1.613
0.10787l
Fiberg/Cal
33.383963
15.930983
2.096
0.03708



When interpreting these results it is important to note the ‘reference’ for the categorical dummy variables is a current smoker who does not take vitamins. Additionally, the fiber variable has an exceptionally high coefficient and standard error as daily calories consumed will be substantially higher than grams of fiber consumed per day. Finally, we cannot reject the possibilities of age and alcohol consumption being statistically insignificant at a 95% confidence level, so those variables will be excluded from our final model. 



3.3 Final Plasma Beta Carotene Model and Interpretation

After excluding the variables that were neither statistically significant at the 95% confidence level or were not included in the model selected by the stepwise search, we can construct our final model.

Final Model:
log(Plasma Beta Caroteneng/ml + 1) = 6.46087 + 0.31196(Former Smoker) + 0.40187(Never Smoked) + 0.20711(Infrequent Vitamin Use) + 0.22802(Frequent Vitamin Use)  - 1.05273(log(Quetelet + 1)) + 0.16557(log(Dietary Beta Carotenemcg + 1)) + 38.7768(Fiberg/Calories) + ε

Adjusted R2: 0.2068



Coefficient Estimate
Standard Error
t value
pr(>|t|)
(Intercept)
6.46087
0.76046
8.496
1.45e-15
Former Smoker
0.31196
0.13133
2.375
0.01824
Never Smoked
0.40187
0.12689
3.167
0.00172
Infrequent Vitamin Use
0.20711
0.10209
2.029
0.04350
Frequent Vitamin Use
0.22802
0.09638
2.366
0.01871
log(Quetelet)
-1.05273
0.19206
-5.481
9.86e-08
log(Dietary Beta Carotenemcg)
0.16557
0.06233
2.656
0.00838
Fiberg/Calories
38.77608
15.75574
2.461
0.01449


As with the model found by stepwise-selection, the reference is a current smoker who does not take vitamins. From the coefficients, we can see smoking and high Quetelet/BMI have a significant negative impact on plasma beta-carotine while vitamin use and diets high in beta-carotene and fiber have a positive effect. From the stepwise procedure, we can conclude it is likely these dietary habits are more significant for estimating plasma beta-carotene than others such as fat or cholesterol heavy diets. Variables such as age and alcohol consumption would reduce bias if included in the model, and it is likely these variables would become significant in an analysis on a larger sample.

3.4 Analysis of Q-Q Plot, Fitted Residuals, and Residuals vs. Leverage

To understand the limitations of our model, we created Q-Q, Residuals vs. Fitted Values, and Residuals vs Leverage plots to identify non-normality, unequal error variances, and detect outliers. The plots can be seen in figure 3.4 in the appendix. 

The Q-Q plot shows some signs of nonnormality, noticeably the heavy tails on both ends. This indicates our data has more outliers than we would expect from a normal distribution. Observations 233, 97, and 262 were the most noticeable outliers on the extremes, but others also fit this trend. Similarly, while the fitted values plot is mostly evenly distributed, the same outliers in the Q-Q plot stand out due to their large distance from the expected outcome. Their position is not extreme, but it does suggest there is some factor not accounted for in the dataset. Finally, in the residuals vs. leverage plot, some observations in the bottom left corner are near, but not beyond, Cook’s distance, so they likely do not significantly impact the regression coefficients or fitted values.

3. Model Fitting for Plasma Retinol

	3.1 Variables and Model Selection

Since this model will focus on Retinol Plasma levels as the response variable, we are trimming the initial dataset in terms of variable scope. The variables to be removed include: the categorical variable 'Sex' since there is only one factor level remaining now that we have removed the males, the original versions of the transformed variables (Retdiet, Quetelet, Betadiet, Fat, Fiber, Cholesterol, Alcohol), and lastly the alternate response variable to be examined in our other model (Betaplasma, logBetaplasma.) We then use the trimmed data set to fit an initial first-order regression model to check model assumptions before proceeding with the greedy search.

Initial model:
Plasma Retinolng/ml + 1) = B0 + B1(Age) + B2(Former Smoker) + B3(Never Smoked) + B4(Infrequent Vitamin Use) + B5(Frequent Vitamin Use)  + B6(Calories) + B7(log(Dietary Retinolmcg + 1)) + B8(log(Quetelet + 1)) + B9(log(Dietary Beta Carotenemcg + 1)) + B10(log(Weekly Alcoholic Drinks + 1)) + B11(9*Fatg/Calories) + B13(Cholesterolmg/Calories) + B12Fiberg/Calories + ε

Looking at these results from the initial model, we can see that the summary table indicates that the coefficients for the age and CaloriesFat variables are both significantly different from 0 albeit at varying alpha levels (~0 and 0.05, respectively.) Additionally, note the adjusted R-squared of 0.04412 which appears quite low at first glance. 
Switching gears to the ANOVA table for the initial model, it shows age as a variable that significantly improves the model as well CaloriesFat which also improves the model albeit much less significantly.






Now, we examine the fitted values vs. residuals plot as well as the QQ plot. The points on the fitted vs. residuals plot are generally randomly scattered with no real discernible pattern indicating linearity of the model and somewhat constant variance. There are 3 outliers that show up in the QQ plot, giving it a sharp upward curve/heavy tail on the upper right end. They are all well above the main cluster however they are pretty evenly spread across the range of fitted values, so we choose to proceed with these variables into the stepwise analysis based on residual plots.


3.2 Stepwise Results

Next, we will run forward stepwise regression, backward stepwise, forward selection and backwards elimination based on AIC which ends up being minimized at a value of 3,616.259.

AIC criteria:

Based on AIC, the 4 main stepwise/selection variations all arrive at the same model that includes the predictor variables age, CaloriesFat, and FiberCalories.

	3.3 Final Plasma Retinol Model and Interpretation

Plasma Retinolng/ml = B0 + B1(Age) + B2(9*Fatg/Calories) + B3(Fiberg/Calories) + ε

	


Comparing this output with that from the initial model, the summary table shows similar results but the p-value for the CaloriesFat variable is much lower (almost crossing into the threshold for alpha = 0.01 level significance.) Also noteworthy is that the R-squared adjusted is actually marginally higher than that of the initial model even though the model includes way less variables, further validating the model selection process. The ANOVA table for the stepwise model also has a higher significance level for the CaloriesFat variable.
Overall, we can interpret these results as age by far being the most significant factor in determining the levels of Retinol in blood plasma for females, followed by CaloriesFat and then lastly FiberCalories. Based on our final model, for every additional year of age predicted blood retinol concentration will increase by 3.1253 ng/ml with all other factors held constant. The results also project that every additional percentage of caloric intake a person consumes in Fat and Fiber will reduce predicted Retinol plasma by 4.3796  and 72.3862 ng/ml respectively.

3.4 Analysis and exclusion of outliers with Residuals vs. Leverage plot and Cook’s Distance

	Lastly, let’s examine the selected final model for potential outliers using a residual vs. leverage plot as well as Cook’s distance. In the residuals leverage plot we can see the two outliers originally identified in the QQ-plot (cases numbers 290 and 293) along with an additional potential outlier in case 122 with a large leverage value. When we look at the Cook’s distance chart, it shows case 122 as having a significantly larger Cook’s distance than any of the other observations.



Next we check how the regression coefficients vary when excluding each of these outliers individually. While cases 290 and 293 show minimal variation in the coefficients, case 122 has a large impact on the FiberCalories coefficient, lowering it by almost 300 (or almost 3 retinol ng/ml per percentage point of fiber consumption to total caloric intake ratio.)







	Since case 122 caused the biggest variation in beta coefficients when excluding it from the model, we decide to use the model without it. When running summary statistics and anova for this new model we notice the R^2 value increase and the significance level of the FiberCalories variable go up, further validating this decision.



Final model:
Plasma Retinolng/ml = 674.1 + 3.317(Age) - 449.4(9*Fatg/Calories) - 1034.1(Fiberg/Calories) + ε
