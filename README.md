# **Machine Learning for Digital Fraud Detection**
### Comparing the Performance of Stacking Classifier & Deep Learning Models in Identifying Instances of Digital Transaction Fraud



Brian Morrison

DATA606 - Capstone Project in Data Science

Professor Jay Wang

The University of Maryland, Baltimore County

## **Contents**
* [Introduction & Background](#introduction--background)

* [Dataset Overview](#dataset-overview)

* [Source Data Splitting](#source-data-splitting)

* [EDA & Dataset Preparation](#eda--dataset-preparation)

* [Ensemble Classification Model](#ensemble-classification-model)

* [Deep Learning Model](#deep-learning-model)

* [Conclusions & Final Thoughts](#conclusions--final-thoughts)

* [References](#project-references)

## **Introduction & Background**

The advent of cashless methods for payment transactions, such as credit cards, digital wallets, and buy now pay later (BNPY) services has incomparably augmented the way in which consumers interact with product and service providers. In fact, market research has indicated that as many as 80% of consumers used some form of digital payment - defined as including "...browser-based and in-app online purchases, in-store checkout using a mobile phone and/or QR code, and person-to-person payments..." in 2020, with nearly 60% of those consumers reporting using two or more forms of digital payment<sup>1</sup>. Cash and check transactions have been becoming increasingly rare relative to more convenient and secure payment methods, such as credit and debit cards - which accounted for over 50% of payment usage in 2019<sup>2</sup> and are almost ubiquitously linked to online transactional systems. While this shift has introduced a number of positive benefits to consumers, another area it has significantly affected is fraud; instances of attempted digital payment fraud have increased dramatically in the past few years, both in quantity and magnitude<sup>3,4</sup>. Given the compounding growth of digital transaction methods, and the resulting surge in fraudulent digital payment attempts, developing robust methods for identifying and classifying instances of fraud is a similarly compounding topic of interest to digital system providers.

Problem Statement: There are more ways to commit payment fraud today than there ever have been, and this statement will again be true in 10 years. Building and deploying a plethora of machine-learning based fraud detection systems, each with different approaches to classifying potential instances of fraud, is likely the best way to combat the wide range of ways individuals can commit fraud. In this project, I propose methods for constructing robust fraud detection models, with a specific emphasis on comparing the performance of Ensemble and Deep Learning based classification models.

## **Dataset Overview**

The dataset for this project is a large log of e-commerce transactions with a range of associated details. The dataset can be accessed [here](https://www.kaggle.com/competitions/ieee-fraud-detection/data). The data was made available through a Kaggle competition held by the IEEE Computational Intelligence Society (IEEE-CIS), and contains real-world ecommerce transactions provided by Vesta, an organization actively targeting digital fraud. 

The dataset is 1.35 GB in size, comprised of four csv files - two training and two testing files, separated by the type of features they contain. Two of the files contain transactional data, basic information about the transaction such as product and payment details, and two of the files contain identity data, information about the purchaser such as device and personal information. The transactional data contains 392 unique numerical and categorical features, while the identity data contains 40 unique numerical and categorical features. The transactions can be correlated between datasets by an identifying feature, 'TransactionID'. 

For this project, only the training dataset will be used, as it contains the target variable that models will be attempting to predict - 'isFraud', a binary variable indicating whether or not the transaction was fraudulent. The testing dataset's class labels were withheld by competition hosts, as test data predictions generated by models were submitted for consideration in project scoring. The training dataset contains over 590,000 individual transactions, with the proportion of fraudulent to legitimate transactions standing at nearly 1 to 30; roughly 3.5% of the dataset.

## **Source Data Splitting**

The first step in beginning this project is to migrate the dataset from its source location, Kaggle, to the project's GitHub repository. While GitHub provides a simple interface to manually upload data, there is a maximum file size limit of 50 MB. The maximum file size limit for data uploaded to a repository through the command line, however, is 100 MB - a much more reasonable limit considering our transactional dataset's size of over 600 MB.

In this notebook, we take steps to split the transactional dataset into 7 files below 100 MB to upload to the project's repository. By using pandas to import the data into a dataframe, we can easily split the dataset into multiple smaller dataframes ready for export. Importantly, these dataframe were split row-wise to support later concatenation through pandas .concat method. The identification dataset was itself below 100 MB, so it did not need to be split before being uploaded to the repository. 

Finally, a brief overview of the Git commands leveraged in uploading the data files to the project repository is provided for context. While many of these commands can be executed through a masked interface in the Visual Studio Code IDE, consideration of the Git commands popularly used for version control allows for some contextualization of the approach to data migration. An overview of the Git commands discussed is included below.

* `$git --version` - *Checking Git version*
* `$git clone https://github.com/briancmorrison/brian_data606.git` - *Cloning the project repository to make local edits*
* `$cd brian_data606` - *Navigating to cloned directory*
* `$git add ['filenames']` - *Adding files to local repository*
* `$git commit -m "Uploaded Source Data"` - *Commiting changes locally, with '-m' specifying the commit message*
* `$git push origin master` - *Pushing changes to GitHub repository, reconciling versions*

After confirming that the dataset additions were made to the GitHub repository, we are ready to move on to dataset exploration, cleaning, and preparation.

## **EDA & Dataset Preparation**

After handling necessary preparation steps for the source data files, the next step in the project is to explore, clean, and prepare data for introduction to machine learning models. Throughout the EDA & Dataset Preparation notebook, changes are made to align data parameters with those expected by many machine learning models - more specifically, changes are largely oriented towards gradient or distance-based models that may be sensitive to differences in the magnitude of variances in features. For example, a distance-based classifier may interpret differences in transaction amounts as more meaningful than differences in number of transactions made by cards due to large discrepancies in purchase amounts, despite the assumption in training that all features should be considered equally.

Thus, the desired outcome from this notebook is a dataset that contains only integer columns with no null values, and whose column ranges are scaled to be approximately (0, 1). 

#### Null Handling & Feature Selection

After some basic data exploration, the first, and arguably most significant, decision to be made in this notebook is how to handle the high proportion of null values present in the dataset. Figure 1 below shows the proportion of null values in each column of the source dataset. The figure clearly displays an extremely high quantity of null values across a majority of features in the dataset, with only a small number of columns containing no null values. 

**Figure 1** - Proportion of null values in each of the combined dataset's 434 columns.

![image](https://user-images.githubusercontent.com/80338181/183307425-a5f42ec1-d7c7-434e-84fa-e007cd2faead.png#gh-light-mode-only)
![image](https://user-images.githubusercontent.com/80338181/183307470-2335ae61-f8c8-4b69-8568-0d1a4da2753d.png#gh-dark-mode-only)

There are a multitude of approaches to handling null values in a dataset, including value imputation, feature combination, dimensionality reduction, or simple removal.

#### Feature Encoding & Scaling

Following the removal of null values, the next step in preparing our dataset for introduction to machine learning models is encoding categorical variables. 

**Figure 2** - Feature distributions prior to scaling.

![image](https://user-images.githubusercontent.com/80338181/183308390-fad2419a-08a9-4292-b6d3-ed37beb908c2.png#gh-light-mode-only)
![image](https://user-images.githubusercontent.com/80338181/183308507-ec7cca50-2eaf-453f-94bd-ce68fa1bb897.png#gh-dark-mode-only)

**Figure 3** - Feature distributions following scaling. 

![image](https://user-images.githubusercontent.com/80338181/183308415-3b1d8cd2-13f4-40b6-beac-5a9384416bc9.png#gh-light-mode-only)
![image](https://user-images.githubusercontent.com/80338181/183308542-eb619dd6-7306-4392-be83-837be452ebd9.png#gh-dark-mode-only)

#### Minority Class Augmentation
##### SMOTE

##### Random Oversampling

## **Ensemble Classification Model**

#### Lazy Predict

#### Model Creation

#### Model Evaluation

## **Deep Learning Model**

#### Consideration - Transfer Learning

#### Model Creation

#### Model Evaluation

## **Conclusions & Final Thoughts**

#### Performance & Comparison

#### Future Directions for Similar Projects

* *Feature Engineering* - 
* *Hyperparameter Tuning* - 
* *Transfer Learning* - 

## **Project References**

1) https://www.mckinsey.com/industries/financial-services/our-insights/banking-matters/us-digital-payments-achieving-the-next-phase-of-consumer-engagement#:~:text=Although%20penetration%20of%20digital%20payments,to%20reach%20the%20remaining%20group.
2) https://www.frbsf.org/cash/publications/fed-notes/2019/june/2019-findings-from-the-diary-of-consumer-payment-choice/#:~:text=Consumers%20used%20cash%20in%2026,percentage%20point%20increase%20from%202017
3) https://resources.sift.com/ebook/digital-trust-safety-index-fraud-economy/
4) https://shiftprocessing.com/credit-card-fraud-statistics/
