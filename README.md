# H1N1 Vaccine Status Predictor and Recommendations to Increase Vaccine Acceptance
**Created by Lucas Fishbein**

![GetVaccine-Blog](https://user-images.githubusercontent.com/117129342/231223874-66fd57bc-14cc-49ba-9ade-5afdc9bd018b.jpg)
[Image source: Living.AAHS.org](https://living.aahs.org/coronavirus-covid-19/covid-19-vaccine-clinics-for-july/)

# Getting Started with this Repository

The Data for this project is stored in the "data" folder of the repository and is ready to run.

# Overview and Business Problem

As it has become very evident with the COVID-19 pandemic, in the globalized world we live in, diseases can quickly spread across the earth like wildfire. In many cases the best defense we have against diseases of this nature are vaccines. Looking at history, the effectiveness of vaccines can be seen via the near or complete eradication of incredibly destructive diseases such as pollio and smallpox. One lesson learned from the recent the Covid 19 Pandemic is that trust in vaccines varies highly in populations, with many people being resistent to taking a vaccine.

The goal of this project is to examine a dataset of behavioral and demographic features of individuals recorded after the 2009 H1N1 flu virus, often referred to as "swine flu", in order to determine what factors may have contributed to individuals accepting or rejecting a vaccine and from there, create action steps that will allow us as a population to increase vaccine rates for future diseases that may occur. 

The stakeholders for this project will be the US National Institutes of Health (NIH) who lead the charge on COVID-19 vaccine information dissemination. The hope of this project is to first allow them to identify those who may be resistent to receiving vaccines by creating a predictive model for vaccination status and secondly bring to light intervention steps that may help increase vaccine rates across the population.

# Data Analysis Results

## Recommendations to Increase Vaccine Rates

#### 1. Have Doctors recommend Vaccinations upon visits more frequently 
#### 2. Diseminate information on the effectiveness of the Vaccine and limit false anti-vaccine rhetoric 
#### 3. Be sure that people fully understand the risks of the present disease as well as the seasonal flu
#### 4. Help the public understand the disease better through science communicators 

## Model's Stats for Predicting Vaccine Status


       precision    recall  f1-score   support

    Not Vaccinated  0.87      0.94      0.90      5260
    Vaccinated      0.68      0.47      0.56      1417

    accuracy                            0.84      6677
    macro avg       0.77      0.71      0.73      6677
    weighted avg    0.83      0.84      0.83      6677
    
### Targetable Vaccine Hesitant Groups

Based on the information extracted from the model the following groups are likely to be vaccine hesitant.

* People of Multi-racial or non-white, black or hispanic descent 
* Unemployed People
* People with "Some College"
    
# Database Understanding

The present dataset consists of survey information from 2012 on 26707 individuals broken into two csv files, one with 35 features of the individuals and the second with labels for the vaccination status for H1N1 of these subjects. These files both contain a unique respondent id number so the two files can be corroborated.

The definitions of the feature columns can be accessed via the H1N1_preds_data_dicrionary in the github repository.

The data is provided courtesy of the United States National Center for Health Statistics from [The National 2009 H1N1 Flu Survey. Hyattsville, MD: Centers for Disease Control and Prevention, 2012.](https://www.cdc.gov/nchs/nis/data_files_h1n1.htm)  



# Data Cleaning and Preprocessing


The cleaned dataset did not remove any of the orginal subjects from the study but three feature columns were completely removed as they had NaNs for about 50% of the data.

In order to use Categorical Variables in the classification modeling, they were converted into numerical codings via one-hot encoding.

The data was then broken into train and test groups at a 75/25 split using train_test_split. This data was then scaled using the StandardScaler as not all of the features had similar scales. 

The remaining missing data points were imputed using the SimpleImputer employing the "most_frequent" strategy.

As can be seen in the graph below, the training data's classes were very unbalanced to begin with at a ratio of about 4:1, SMOTE was employed in order to balance the training set prior to modeling.

<img width="854" alt="Screenshot 2023-05-18 at 10 27 14 AM" src="https://github.com/LucasFishbein/H1N1_Vaccine_Status_Predictor/assets/117129342/f1f279d9-ffb8-4959-afee-712234e9c79d">


# Data Analysis and Modeling

As this is a classification problem, 3 different types of ensemble classification models were trained and compared, these types being RandomForestClassifier, KNearestNeighbors and XGBoost. Each model was built into a baseline Pipeline that included the preprocessing steps above, Scaling, Imputing and SMOTE as well as the classifier and then fit to the original (before preprocessing) training data. 

Each Pipeline's hyperparameters were then tuned using a GridSearch Method applied to the classifier in order to optimize the model for the present data. The results of the gridsearch were then hardcoded into a final model for each classfier.

A fourth Pipeline was also built out using XGBoost and the same framework as before but lacking the class balancer, SMOTE, as XGBoost has native features to deal with unbalanced data.

All four of these Pipelines were finalized and then compared via their confusion matrices and classification reports. The best performing model within the context of the current business problem was then choosen and analyized.

The final step of the data analysis was to inspect the feature importance scores of the model by examining the coefficients for each feature within the model. From here the top features were extracted and used to create action steps that the NIH could hopefully use to help increase vaccination rates.

# Final Model for Predicting H1N1 Vaccine Status

Of the four pipelines built out, the XGBoost model that included SMOTE was the best performing by a tiny margin. The confusiuon matrix as well as the classification report for this model can be examined below:

0 = Not Vaccinated
1 = Vaccinated

<img width="381" alt="Screenshot 2023-04-13 at 12 35 00 PM" src="https://user-images.githubusercontent.com/117129342/231826334-88b5c718-3e88-43b1-9bd7-687895a863ea.png">

                    precision recall  f1-score   support

    Not Vaccinated  0.87      0.94      0.90      5260
    Vaccinated      0.68      0.47      0.56      1417

    accuracy                            0.84      6677
    macro avg       0.77      0.71      0.73      6677
    weighted avg    0.83      0.84      0.83      6677
    
## Interpreting Final Model

The final model has an overall accuracy of 84% and performs significantly better at predicting subjects that are not vaccinated than vaccinated indivduals, this is evident via the precision, recall and f1-scores. It can also be seen via the confusiuon matrix that there are over twice as many false negatives as false positives.

Both of these features of our final model make it less than optimal but still perfectly workable as the main focus of this project is to identify those who did not get vaccinated as those who have been vaccinated already have complied with the overall goal of increasing vaccination rates. The final model does a very good job of identifying those who have not been vaccinated with an f1 score of 0.90. These results give confidence that those who may be resistent to vaccinations will be identified using this predictive classification model.

## Feature Importance in Final Model

A breakdown of the top 20 most important features in the model are displayed below. The Coef number is equal to how big of an impact that factor has on the probability of an individual accepting a vaccine. 
    
<img width="633" alt="Screenshot 2023-04-13 at 12 36 28 PM" src="https://user-images.githubusercontent.com/117129342/231826595-abed63a7-2342-4600-9b6c-d0643372c89d.png">

From this graph it can be extrapolated that the top most important features of someone that received the H1N1 vaccine are:

>  * A doctor's recommendation to take the vaccine (0.18)
>  * One's Opinion on the effectiveness of the vaccine (0.12)
>  * One's opinion on the risk of H1N1 (0.06)
>  * One's opinion on the risk of seasonal flu (0.05)
>  * One's Knowledge of the H1N1 diesease (0.05)

### Targetable Vaccine Hesitant Groups

Based on the information extracted from the model the following groups are likely to be vaccine hesitant.

* People of Multi-racial or non-white, black or hispanic descent 
* Unemployed People
* People with "Some College"


# Recommendations and Conclusions

Based on the information gathered from the finalized classification prediction model, the following recommendations will be given to the NIH in an attempt to give them action steps that will increase the rates of vacination for the Corona Virus as well as future diseases.

### 1. Have doctors recommend Vaccinations upon visits
> The factor the had the greatest overall correlation with a positive vaccine status was a recommendation from a Doctor, in order to best utalize this information it is important that known and trusted doctors, such as one's PCP or pediatrician are recommending vaccines at every reasonable opportunity that presents itself. 

### 2. Diseminate information on the effectiveness of the Vaccine and removal of false anti-vaccine rhetoric
> The seconnd most impactful factor was the subjects opinion of the effectiveness of the vaccine, to combat this it is important that this information is packaged in an approachable way and given to the public as increasing this knowledge may increase vaccine rates.

### 3. Be sure that people fully understand the risks of the present flu disease as well as the seasonal flu
> The next two strongest factors were found to be the subject's opinion on the risks of the H1N1 disease as well as the seasonal flu, this is a delicate subject as the risks should not be communicated in a way that will cause a panic, but it is important that the true risks are well known by the population so that they have a better idea of the cost-benefit of recieving a vaccine.

### 4. Help the public understand the nature of the disease better
> A correlation exists that shows that as people better understood the H1N1 disease they were more likely to get vaccinated for the disease. Much like the other recommendations it is important this this information is packaged in an appropriate non-technical manner so that the average person can understand more about the present disease.

Based on the information generated by the predictive classification model that has been built out, it seem that information is the key to helping people accepting vaccinations. Whether it is information about the risk of the disease or the effectiveness of the vaccines, it seem that the more people understand the disease the more responsive people are to taking vaccines.

It is the hope of the project team that these conclusions can be utalized to help inform the NIH on where they should be putting their efforts regarding diseases similar to H1N1 in the future, in order to maximize vaccine rates accross the population and ultimatly avoid any unnecessary harm that may be caused by a disease of this nature.

# For More Information
See the full analysis in the [Jupyter Notebook](https://github.com/LayFish21/H1N1_Vaccine_Status_Predictor/blob/main/H1N1_Vaccine_Prediction_Model.ipynb) 

For additional info, contact Lucas Fishbein at FishbeinLucas@gmail.com

## Repository Structure

```
├── .gitignore
├── CONTRIBUTING.md
├── H1N1_Vaccine_Status_Predictor.ipynb
├── data
├── LICENSE.md
└── README.md
```

