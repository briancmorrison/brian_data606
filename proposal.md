# **Project Proposal**
### Background & Problem Statement

The advent of cashless methods for payment transactions, such as credit cards, has [IMPORTANCE FRAMING]

### Dataset Overview

The dataset can be accessed [here](https://www.kaggle.com/competitions/ieee-fraud-detection/data). The data was made available through a Kaggle competition held by the IEEE Computational Intelligence Society (IEEE-CIS), and contains real-world ecommerce transactions provided by Vesta - an organization actively targeting digital fraud. 

The dataset is 1.35 GB in size, comprised of four csv files - training and testing transactions data files, as well as training and testing identity data files. 

DESCRIBE DATA AND UNIT OF ANALYSIS

### Research Question

Will Stacking (Ensmeble) Classifiers or Deep Learning Models display the best performance in identifying and classifying instances of credit card fraud based on an array of relevant features? 

Can feature engineering be used to augment the performance of models?

### Proposed Models & Approach

PROPOSE STACKING CLASSIFIER VS DEEP LEARNING MODELS

PROPOSE METHODS OF FEATURE ENGINEERING

### Considerations

* This data was made public to facilitate a popular Kaggle competition, which received a large number of model submissions from over 6,000 teams. While this competition does provide one of the most rich fraud detection datasets available, the novelty of the proposed project may be affect as a result. It is unlikely entirely new approaches to this problem will be explored.
* In a use case such a fraud detection, there are inherent class imbalances that must be accounted for in data preprocessing. There are, naturally, magnitudes fewer instances of fraudulent credit card transactions relative to legitimate credit card transactions, and it is likely that models will not reach peak performance without some augmentation to the minority class. While methods like bootstrapping are reasonable accomodations for model training, it is important to note that the use of synthetic fraudulent transactions is in contrast with the proposed goal of the research - to identify *legitimate* instances of fraudulent transactions. 
