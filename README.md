

![cover_photo](./6_README_files/cover_photo.png)





## 1. Problem Identification

#### 1.1. Background: Lending Club

*[LendingClub](https://www.lendingclub.com/) is an American peer-to-peer lending company, headquartered in San Francisco, California. It was the first peer-to-peer lender to register its offerings as securities with the Securities and Exchange Commission (SEC), and to offer loan trading on a secondary market. LendingClub is the world's largest peer-to-peer lending platform. The company claims that $15.98 billion in loans had been originated through its platform up to December 31, 2015.

LendingClub enables borrowers to create unsecured personal loans between $1,000 and $40,000. The standard loan period is three years. Investors can search and browse the loan listings on LendingClub website and select loans that they want to invest in based on the information supplied about the borrower, amount of loan, loan grade, and loan purpose. Investors make money from interest. LendingClub makes money by charging borrowers an origination fee and investors a service fee.[Company Wiki Page](https://en.wikipedia.org/wiki/LendingClub)*

#### 1.2 Problem Statement

In this capstone project, our goal is to create a loan interest rate generator based on the characteristics on each loan. 


## 2. Data Wrangling

#### 2.1. Data Collection

The dataset contains loan data for all loans issued through the 2007-2015, including the current loan status (Current, Late, Fully Paid, etc.) and latest payment information. The file containing loan data through the "present" contains complete loan data for all loans issued through the previous completed calendar quarter. Additional features include credit scores, number of finance inquiries, address including zip codes, and state, and collections among others. The file is a matrix of about 890 thousand observations and 75 variables. The datast is acquired through the Kaggle API by clicking on the links below:


> * [Kaggle Dataset](https://www.kaggle.com/wendykan/lending-club-loan-data)

#### 2.2. Data Definition

we investigate the below features with the help of info(), describe(), and panda profiling. 

    1.	Column Name
    2.	Data Type (numeric, categorical, timestamp, etc)
    3.	Description of Column
    4.	Count or percent per unique values or codes (including NA)
    5.	The range of values or codes


## 3. Data Cleaning

* **Problem 1:** Handling missing data. **Solution:** use fillna() to imputate the missing value with its mean, median or mode. In some cases, just replace the missing data with zeros.

* **Problem 2:** Removing duplicates. **Solution:** use the built in Pandas DataFrame function drop_duplicates(). 


## 4. Exploratory Data Analysis




## 5. Pre-processing and Training Data Development

•	Create dummy or indicator features for categorical variables
•	Standardize the magnitude of numeric features: minmax or standard scaler 
•	Split into testing and training datasets: 



## 6. Modeling

#### 6.1. Method

There are two main types of regression models:

1. **Simple Regression:** Use for linear data.

2. **Ensemble Methods :** Random Forest Regression. If the data is nonlinear, Ensemble Method generates better predictions.

![](./6_README_files/matrix_example.png)


**WINNER:Random Forest Regression** 


I choose Random Forest Regressor to accomodate the nonlinear nature of the dataset.





## 5. Algorithms & Machine Learning

[ML Notebook](https://colab.research.google.com/drive/1kAlvwwJnGcdCAJD8oFokT3gtJF2UnyZP)

I chose to work with the Python [surprise library scikit](http://surpriselib.com/) for training my recommendation system. I tested all four different filtered datasets on the 11 different algorithms provided, and every time the Single Value Decomposition++ (SVD++) algorithm performed the best. It should be noted that this algorithm, although the most accurate is also the most computationally expensive, and that should be taken into account if this were to go into production.

![](./6_README_files/algo.png)

>***NOTE:** I choose RMSE as the accuracy metric over mean absolute error(MAE) because the errors are squared before they are averaged which gives the RMSE a higher weight to large errors. Thus, the RMSE is useful when large errors are undesirable. The smaller the RMSE, the more accurate the prediction because the RMSE takes the square root of the residual errors of the line of best fit.*

**WINNER: SVD++ Algorithm**

This algorithm is an improved version of the SVD algorithm that Simon Funk popularized in the million dollar Netflix competition that also takes into account implicit ratings (*yj*). Using stochastic gradient descent (SGD), parameters are learned using the regularized squared error objective.

![](./6_README_files/forumla.png)

## 6. Which Dataset to choose?

[More details about this process...](https://colab.research.google.com/drive/1kAlvwwJnGcdCAJD8oFokT3gtJF2UnyZP)

After choosing the SVD++ algorithm, I tested the accuracy of all four different filtered datasets. The dataset which filtered out any route names occurring less than 6 times performed the most accurate predictions. Thus, it was chosen to be the dataset I trained on.

>* All of the dataframes displayed discrepancies with the 1 star ratings(This is to be expected due to the inherent skewed positive ratings). Also, the one star ratings are not imperative to this project's goal. It is more important that the 1 star ratings are different enough to be filtered out of the top ten routes recommended to users. 
>* Notice the 3-star rating has a fat bulge at the top of the "violin" which indicates its predicting 3-star ratings for some of the true 3-star routes. This was not as prominent in the other dataframes
>* The 1-star rating also has a fatter tail than the other datasets displayed


![](./6_README_files/accuracy.png)

## 7. Coldstart Threshold
[More details about this process...](https://colab.research.google.com/drive/1kAlvwwJnGcdCAJD8oFokT3gtJF2UnyZP)

**Coldstart Threshold**: There is a problem when only using collaborative based filtering: *what to recommend to new users with very little or no prior data?* Remember, we already set our cold start threshold for the routes by choosing the dataset that filtered out any route occurring less than 6 times. Now, let investigate where to put the threshold for users.

![](./6_README_files/20user_thresh.png)

*It is my hypothesis that the initial filtering of the routes is what affected the RMSE of the users* 

>* Increasing the user threshold to 5 would increase the RMSE by .005 & would lose approximately 40% of the data.
>* Increasing the user threshold to 13 would increase the RMSE by .0075 & would lose approximately 60% of the data
>* If there were a larger increase in the RMSE (>= .01) I would trade my users' data for this improvement. However, these improvements are too minuscule to give up 40%-60% of my data to train on. Instead, I voted to keep some of these outliers to help the model train, and will focus on fine tuning my parameters using gridsearch to improve the RMSE


## 7. Predictions

[Final Predictions Notebook](https://colab.research.google.com/drive/1vLkoW_4SYessy_igmJxlVz_jEPlgJ06v)

In the final predictions notebook, the user can enter their user_id number and receive a list of top ten routes recommended to them:

![](./6_README_files/predictions.png)

## 8. Future Improvements

* In the future, I would love to spend more time creating a filtering system, wherein a climber could filter out the type, difficulty of climb, & country before receiving their top ten recommendation

* This recommendation system could also be improved by connecting to the 8a.nu website so that the user could input their actual online ID instead of just their user_id number 

* Due to RAM constraints on google colab, I had to train a 65% sample of the original 6x dataset. Without resource limitations, I would love to train on the full dataset. Preliminary tests showed that the bigger the training size, the lower the RMSE. One test showed an increase in sample size could increase the RMSE by .03 (in contrast to the .005 improvement I received when increasing the coldstart threshold)

## 9. Credits

Thanks to Nicolas Hug for his superb surprise library scikit, Colin Brochard for his stellar advice from his Mountain Project recommendation system, and DJ Sarkar for being an amazing Springboard mentor.
