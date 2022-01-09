# Pet Insurance Customer Segmentation

[![Python 3.8](https://img.shields.io/badge/python-3.8-blue.svg)](https://www.python.org/downloads/release/python-380/)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-no-red.svg)](https://github.com/stevenrhart/predicting-claims/graphs/commit-activity)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

**[Executive Summary](#exec-summary)** | **[Datasets](#data)** | **[Wrangling](#wrangling)** | **[EDA](#eda)** | **[Results](#results)** | **[Future](#future)**

See the [presentation](#link)

See the [complete report](#link)


## EXECUTIVE SUMMARY <a id='overview'></a>

In 2020, the global pet insurance market is estimated to exceed 4 billion dollars [1](https://www.grandviewresearch.com/industry-analysis/pet-insurance-market). And in the US alone, the market value is anticipated to be close to half this total amount ($1.6 Billion USD) with sustained year-over-year growth of nearly 15% for the foreseeable future [2](https://www.ibisworld.com/industry-statistics/market-size/pet-insurance-united-states/). With so much potential revenue at stake, the need for proactive customer marketing is as critical as ever.

The idea behind pet insurance is simple and similar to the human health insurance market. When a pet insurance policy holder incurs veterinary expenses related to their enrolled pet, they can submit claims for reimbursement, and the insurance company reimburses eligible expenses.

The Marketing department at a leading pet insurance provider is seeking to better understand its customer base to prevent customer loss and drive additional company revenue. Data suggests that customers are most likely to cancel their policies around the 2-year point following enrollment. The goal of this project is to identify 3-4 customer segments (with some justification for each) that will enable the Marketing team to reduce customer shrinkage while improving outcomes related to targeted ads and/or direct-to-customer campaigns.

After evaluating a different clustering methods, the best performing model resulted in 4 distinct clusters in the data representing 4 distinct customer segments. Two predominate driving factors were observed in the clustering results - the first being the number of policy years with claims (either 0, 1, or 2) and the second based on which specific policy years the customer claims take place (in the event of only having claims in a single year).    


## DATASETS <a id ='data'></a>

The underlying source data for the project consists of two files - `PetData.csv` and `ClaimData.csv` obtained from a national pet insurance provider. The PetData file contains data for 50000 unique pets who enrolled for policies during the 2018 calendar year. The pet data includes 8 features which provide information about the type of pet (e.g., species, breed, age) and the cost of the policy (i.e., premium and deductible). The claim data includes 4 features detailing insurance claims recorded over a 3-year period between 2018 and 2020 providing the claim date and amount. The two datasets are linked by a common feature, PetId, which can be used to understand the claims totals for each individual pet. prediction.


## DATA WRANGLING <a id ='wrangling'></a>

Overall, the two datasets were relatively clean and the bulk of the data wrangling process consisted of data verification and determining how best to combine the pets data with the associated claims data. A few columns required some additional manipulation in preparation for exploratory data analysis. 
    
### Key Observations:
* **Pet Count** - Verified 50,000 unique pets (based on PetIds)
* **Species** - Data consists of two species of pets, cats and dogs (with dogs outnumbering cats 5 to 1)
* **Breed** - Observed 373 unique breeds in total (55 cat and 318 dog) 
* **Age** - Pet ages range between 0 (i.e., < 1 year) and 13 
* **Premium** - Premiums fall into a wide range with a few outlier values close to $1000 
* **Deductible** - Deductibles are fairly well distributed and appear to be stratified across a range of common values 
* **Median Claims** - For cats and dogs, the median value for total number and total amount of claims is 0 
* **Outlier Claims** - Both species have some significant outliers in both categories (number and amount of claims)


## EDA <a id ='eda'></a>

### Species differences related to claims

During exploratory data analysis, a number of observations stood out in relation to overall claims totals. In general, dog owners have more claims and higher total claims amounts than cat owners. And unsurprisingly, dogs also are much more likely to have claims in both of the first two policy years. 

#### Dogs tend to have higher claims totals on average

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures//Total-Claims-by-Species.png" />

#### Dogs also tend to have a higher number of claims

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures/Number-Claims-by-Species.png" />

#### Dogs are almost twice as likely to have claims in both policy years

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures/Claims-by-PolicyYear.png" />


### Overall feature correlation

Other than closely related features (e.g., Total number of claims and total amount of claims), no particularly interesting relationships were observed in terms of feature correlation.

#### Relatively weak correlation between features

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures/Feature-Correlation.png" />


### PCA
EDA was helpful in providing better context for the relationships in our dataset, but was inconclusive in terms of identifying clear customer groupings. PCA was utilized to better understand which features contribute the most to the variance in our data as a step toward identifying meaningful clusters of customers. 

#### PCA result illustrating that over 85% of the variance is explained in the first 3 principle components

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures/Explained-Variance-by-Component.png" />

#### Isolating the first 3 components, we observed that claims-related features contribute most to the variance

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures/Feature-Importance-by-Component.png" />


## CLUSTERING RESULTS <a id='results'></a>

In this project, we evaluated data for 50,000 pet insurance customers with a goal of building a model to identify distinct clusters of customers based on basic info about the pet (breed, age, species, etc.) and claims data (number of claims, amount of each claim, etc.) for the first policy year. We evaluated two different clustering models (K-Means and DBSCAN) and in the end, found the best performance using DBSCAN with some tuning of essential parameters. 

Using our final clustering model, we identified 4 distinct clusters in the data as described below.
* **Segment 1** - Customers with claims only in the second policy year
* **Segment 2** - Customers with no claims over the first two policy years
* **Segment 3** - Customers with claims in both of the first two policy years 
* **Segment 4** - Customers with claims only in the first policy year

#### Customer segmentation results

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures/DBSCAN-Customer-Segments.png" />

#### Customer segmentation analysis

<img src="https://github.com/stevenrhart/pet-insurance-customer-segmentation/blob/master/figures/DBSCAN-Count-of-YrsWithClaims-by-Segment.png" />


### Segmentation Options
    
Within this result, the Marketing team should have a few options for slicing and dicing customers to arrive at the best combination for a specific outreach goal. For example, if the goal is to demonstrate value for customers with no claims, or no recent claims, Marketing can target customers in Segments 1 and 2. In contrast, if the goal is to target customers making consistent claims or whose claims are on the rise, Marketing can focus on customers in segments 3 and 4.


## FUTURE RESEARCH <a id = 'future'></a>

The following are recommendations for potential next steps to refine or further expand upon the work done in this project:

* **Obtain additional customer data** - The dataset for this project relies heavily on data related to customer claims. While this is helpful in identifying customer segments based on claims data, it's possible additional data could lead to a different clustering outcome and present new and interesting opportunities for marketing to existing customers or reducing customer churn.
* **Engineer additional features** - Feature engineering in this project was largely focused on claims data. It's possible that additional feature engineering could be done to improve model performance. Suggestions include:
    * **Timing of Claims** As part of data wrangling, we rolled up our claims data into totals and averages per pet both overall and by claims year. But given our results, it stands to reason that the timing of when claims are submitted could factor into the resulting customer segments. 
    * **Additional Breed Data** It is widely known that different pet breeds have different characteristics, but our limited dataset did not include any data specific to each breed. By including additional breed specific data in our analysis, it may be possible to engineer meaningful features to improve our clustering model. One example might be engineering a feature that calculates the 'risk index' for a pet given their age, breed and species. 