# Starbucks Capstone Challenge
Data Scientist Nanodegree of Udacity

### Table of Contents

1. [Installation](#installation)
2. [File Descriptions](#files)
3. [Results](#results)
4. [Acknowledgements](#licensing)

## Installation <a name="installation"></a>

	1) Python 3.x
	2) numpy
	3) pandas
	4) matplotlib

## Project Motivation<a name="motivation"></a>

It is the Starbuck's Capstone Challenge of the Data Scientist Nanodegree in Udacity. We get the dataset from the program that creates the data simulates how people make purchasing decisions and how those decisions are influenced by promotional offers. We want to make a recommendation engine that recommends Starbucks which offer should be sent to a particular customer.

We are interested to answer the following two questions:
1. Which offer should be sent to a particular customer to let the customer buy more?
2. Which demographic groups respond best to which offer type?


## File Descriptions

The notebook available here showcases work related to the above questions.  

This data set is a simplified version of the real Starbucks app because the underlying simulator only has one product whereas Starbucks actually sells dozens of products.

The data is contained in three files:
- `portfolio.json` - containing offer ids and meta data about each offer (duration, type, etc.)
- `profile.json` - demographic data for each customer
- `transcript.json` - records for transactions, offers received, offers viewed, and offers completed

Here is the schema and explanation of each variable in the files:

`portfolio.json`
- id (string) - offer id
- offer_type (string) - the type of offer ie BOGO, discount, informational
- difficulty (int) - the minimum required to spend to complete an offer
- reward (int) - the reward is given for completing an offer
- duration (int) - time for the offer to be open, in days
- channels (list of strings)

`profile.json`
- age (int) - age of the customer
- became_member_on (int) - the date when customer created an app account
- gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
- id (str) - customer id
- income (float) - customer's income

`transcript.json`
- event (str) - record description (ie transaction, offer received, offer viewed, etc.)
- person (str) - customer id
- time (int) - time in hours since the start of the test. The data begins at time t=0
- value - (dict of strings) - either an offer id or transaction amount depending on the record


## Results

Algorithim:

We use FunkSVD to split the matrix into user matrix, latent feature matrix and offer matrix. FunkSVD is used because there are missing values inside the matrix and normal SVD just don’t work. To test our prediction, we split the records into the training set and the testing set. We want to use the previous records (earlier time) as the training set to build our model, and then use the same model to test the later records. It will be a good test to test whether our model predicts customer response correctly

Evaluation:

After we decompose the user-offer matrix to user matrix and offer matrix, we test it with our testing set. For the testing, we found an interesting phenomenon, the model built with 15 latent features get the worst performance of MSE 0.3303. The one that built with 10 latent features makes the best result in the prediction of MSE 0.009175, while the one that built with 5 latent features had similar performance before of MSE 0.02391. This interesting result may due to overfitting. Also, It may be due to the splitting of the records since the offer sent frequently and across a few days. We will encounter some cases that the user received the offer and viewed the offer in the training set, but completed the offer in the testing set. In this case, we didn’t count the user completed the offer. From the above, we can’t really be sure how many features to use, because simply having a better way to predict the value of the matrix doesn’t exactly give us an indication of if we are able to make good recommendations


Conclusion:

Based on the transcript records, we build an user-item-matrix that represents how users responded to the offers they received. We then split the records into the training set and the test set and trained our SVD algorithm to predict how a user responses to a particular offer. We achieved the lowest mean square error around 0.003823 with 15 latent features with the training set and around 0.009175 with 10 latent features with the testing set. After that, we created a recommendation engine that recommends Starbucks which offer should be sent to a particular user.

In the later section, we found out which demographic groups respond best to which offer type. Female respond much better than men, in both BOGO and discount. Men react slightly better to discount than BOGO. We also found that it is better to promote the offer via social media. Among the ten offers, sending buy 10 dollars get 2 dollars off within 10 days offer via email, web, mobile and social makes Starbucks gain more. It is the best offer so far!


## Acknowledgements

Must give credit to Udacity and Stakbucks for the data.
