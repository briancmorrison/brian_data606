# **Project Proposal**
### Background & Problem Statement

The advent of cashless methods for payment transactions, such as credit cards, has incomparably augmented the way in which consumers interact with product and service providers. In fact, market research has indicated that as many as 80% of consumers used some form of digital payment - defined as including "...browser-based and in-app online purchases, in-store checkout using a mobile phone and/or QR code, and person-to-person payments..."  in 2020, with nearly 60% of those consumers reporting using two or more forms of digital payment<sup>1</sup>. Cash and check transactions have been becoming increasingly rare relative to more convenient and secure payment methods, such as credit and debit cards - which accounted for over 50% of payment usage in 2019<sup>2</sup> and are almost ubiquitously linked to online transactional systems. While this shift has introduced a number of positive benefits to consumers, another area it has significantly affected is fraud; instances of attempted digital payment fraud have increased dramatically in the past few years, both in quantity and magnitude<sup>3,4</sup>. Given the compounding growth of digital transaction methods, and the resulting surge in fraudulent digital payment attempts, developing robust methods for identifying and classifying instances of fraud is a similarly compounding topic of interest to digital system providers. 

Problem Statement: There are more ways to commit payment fraud today than there ever have been, and this statement will again be true in 10 years. Building and deploying a plethora of machine-learning based fraud detection systems, each with different approaches to classifying potential instances of fraud, is likely the best way to combat the wide range of ways individuals can commit fraud. In this project, I propose methods for constructing robust fraud detection models, with a specific emphasis on comparing the performance of Ensemble and Deep Learning based classification models. 

### Dataset Overview

The proposed dataset for this project is a large log of e-commerce transactions with a range of associated details. The dataset can be accessed [here](https://www.kaggle.com/competitions/ieee-fraud-detection/data). The data was made available through a Kaggle competition held by the IEEE Computational Intelligence Society (IEEE-CIS), and contains real-world ecommerce transactions provided by Vesta, an organization actively targeting digital fraud. 

The dataset is 1.35 GB in size, comprised of four csv files - two training and two testing files, separated by the type of features they contain. Two of the files contain transactional data, basic information about the transaction such as product and payment details, and two of the files contain identity data, information about the purchaser such as device and personal information. The transactional data contains 392 unique features, while the identity data contains 40 unique features. The target variable, feature 'isFraud', is a binary variable representing whether or not the transactional was fraudulent. The transactions can be correlated between datasets by an identifying feature, 'TransactionID'. The training dataset contains over 590,000 individual transactions, while the testing dataset contains just over 500,000 individual transactions.

### Research Question

Will Stacking (Ensmeble) Classifiers or Deep Learning Models display the best performance in identifying and classifying instances of credit card fraud based on an array of relevant features? 

Can feature engineering be used to augment the performance of models?

### Proposed Models & Approach

While model proposals may shift based on initial performance analyses, I am principally interested in investigating the performance differences between an Ensemble, or Stacking, Classifier and a Deep Learning Model in this project. My proposed approaches to building these models are: 

* **Stacking Classifier** - Use [Lazy Predict](https://lazypredict.readthedocs.io/en/latest/) to fit multiple simple classification models to the dataset, selecting the top five highest performing models to aggregate into a final stacked model. Then, experiment with both Logistic Regression and XGBoost as final classifiers within the model to assign decisive predictions about classes. 
*  **Deep Learning Model** - Use Keras to build a simple neural network capable of binary classification, with a flattening input layer and no more than five subsequent dense layers. Potential to incorporate transfer learning by introducing a pretrained network to the dataset and adjusting its final output. 

While the proposed models are relatively straightforward, I would also like to experiment with performing dimensionality reduction on the breadth of included features to enhance model performance. Specifically, given the diminished weight of the target class - or significantly lower presence of fraudulent transactions relative to legitimate transactions - I would like to experiment with approaches to minority class augmentation. Additionally, leveraging other approaches to dimensionality reduction, such as principal components analysis, is something I would like to include as a method for tuning out noise and boosting the training and testing performance of the model (reducing overfit potential). 

### Considerations

* This data was made public to facilitate a popular Kaggle competition, which received a large number of model submissions from over 6,000 teams. While this competition does provide one of the most rich fraud detection datasets available, the novelty of the proposed project may be affected as a result. It is unlikely entirely new approaches to this problem will be explored.
* In a use case such a fraud detection, there are inherent class imbalances that must be accounted for in data preprocessing. There are, naturally, magnitudes fewer instances of fraudulent credit card transactions relative to legitimate credit card transactions, and it is likely that models will not reach peak performance without some augmentation to the minority class. While methods like bootstrapping are reasonable accomodations for model training, it is important to note that the use of synthetic fraudulent transactions is in contrast with the proposed goal of the research - to identify *legitimate* instances of fraudulent transactions. 

### References

1: https://www.mckinsey.com/industries/financial-services/our-insights/banking-matters/us-digital-payments-achieving-the-next-phase-of-consumer-engagement#:~:text=Although%20penetration%20of%20digital%20payments,to%20reach%20the%20remaining%20group.

2: https://www.frbsf.org/cash/publications/fed-notes/2019/june/2019-findings-from-the-diary-of-consumer-payment-choice/#:~:text=Consumers%20used%20cash%20in%2026,percentage%20point%20increase%20from%202017

3: https://resources.sift.com/ebook/digital-trust-safety-index-fraud-economy/

4: https://shiftprocessing.com/credit-card-fraud-statistics/
