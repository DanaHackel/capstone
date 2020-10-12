# Predicting Drug Prices
Dana Hackel, General Assembly DSI Capstone Project
## Problem Statement
The price of many drugs, both prescription (Rx) and over the counter (OTC) can often be very expensive.
Having a way to predict price can help pharmaceutical companies determine price for newly approve drugs and can help compnaies compare their current
price to similar drugs to determine if they are priced competitively. I will use the National Average Drug Acquisition cost data to build a regression model
(as of check-in 4, my best model is K Nearest Neighbors, but that may change) that predicts drug price values.

## Data Collection
My data was downloaded from the following sources:

[National Average Drug Acquisition Cost](https://healthdata.gov/dataset/nadac-national-average-drug-acquisition-cost)

        The National Average Drug Acquisition Cost information is collected from Medicaid.
[FDA Orange Book Data](https://www.fda.gov/drugs/drug-approvals-and-databases/orange-book-data-files)

        The Orange Book is a list of all drug products approved by the FDA.

## Data Cleaning and Exploration
Originally, I planned to work with just the NADAC data, however I quickly realized that the dataset did not provide the names of drugs, just their active ingredient.
I first tried to match their national drug code (NDC) numbers to a NDC database from the FDA. The NDC database would have provided
drug name and some other features such as phamacological type (i.e. analgesic/ pain reliever, antihistamine, selective serotonin reuptake inhibitor (SSRI), etc) however
some of the NDC's in the NADAC dataset were missing part of the code. So, I planned to use a library called Fuzzywuzzy to match the NDCs in the two datasets.
The Fuzzywuzzy library uses Levenshtein distance to determine strings which are similar based on a score (in my case, 85% similarity). While trying to match NDC numbers, I quickly learned that
this would not work as planned. If one number was slightly out of order, it would match drugs with slightly different NDCs that were not similar at all (i.e. an antacid was matched
with hand-sanitizer). In order to circumvent this problem, I decided to use the Orange Book data instead, which included the drug's active ingredient, strength, and dose route. I was able to
match this to a column containing the same information in the NADAC dataset.

Before combining the two datasets, I also cleaned the NADAC data. I removed duplicate drug entries, keeping the most recent 'As of' date. This dataset is updated frequently, as the price of drugs
can change frequently, but I want my model to predict price off of the most current information.

## Predictive Models
I ran the following models with my drug price data and determined ___ was the best model.
(Best model is currently KNN because it has the highest accuracy, least variance, and lowest RMSE score)

I decided to use Root Mean Squared Error (RMSE) to assess my error as that is the average error, in dollars, from each model.

| Model | Train Score | Test Score | Test RMSE ($) | Comments |
|-------|-------------|------------|---------------|----------|
|Linear Regression| 0.926903481896789 | -6.147243174148271e+18 | 33619121272.395153 | This had an embarassingly high variance and error (probably way too many features)|
|K Nearest Neighbors| 0.8915498524974116 | 0.8080358209686297 | 5.940954948304041 | This had a much better accuracy score and much less variance. The model is still, on average $5.94 off on price prediction, which is still relatively high since many of the drugs have an acquisition cost below $1.00|
|Random Forest| 0.87318737377666 | 0.7989923850483017 | 6.0792837755132805 | Slightly lower accuracy, and slightly higher variance/ error.  |
|Voting Classifier| Currently running with Adaboost, Gradient Descent, and Random Forest| - | - |
|Support Vector Machine| Next on the list | - | - |

## Drug Prices Over Time

This part is still a work in progress. I have one graph made on Tableau and am working on the second (for brand name drugs, which requires saving a second csv to filter out the generics - this should be up tonight but I am completing the readme first)
I previously made two timeseries graphs during my original EDA, but they have changed slightly as the most mentioned drugs has changed once I combined my dataframes.
Old images can be seen in the eda_nadac notebook.

## Creating an Interactive Visual Dashboard on Tableau

Please view the current dashboard [here](https://public.tableau.com/shared/BMZC4F56D?:display_count=y&:origin=viz_share_link).
This is also still a work in progress.

## Future Directions/ To-Do before the End:
Things to continue after check in 4 (this will be changed for the final submission):
<ol>
<li> Run Grid-Search with best performing model (right now thats KNN) to try to lower variance and/or increase accuracy score
<li> Complete visuals for Tableau
<li> More timeseries analysis

- Look for seasonality and patterns
- Assess stationarity in some drugs?
- Predict future prices of drugs?

<li> Add Brand name info to timeseries (requires a separate csv of just brandname drugs)
</ol>

General Next Steps: (for now)
<ol>
<li> Continue to add/remove parameters to improve accuracy score and lower variation
<li> Find more data to add!
</ol>

## Sources
 [Github of a similar project](https://github.com/alofgran/Drug-Price-Prediction)
