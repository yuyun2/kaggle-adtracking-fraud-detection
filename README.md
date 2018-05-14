# Adtracking Fraud Detection
This project includes an end to end solution of the kaggle competition, TalkingData AdTracking Fraud Detection Challenge. The goal is to predict if a click will lead to download given a set of attributes.


## Getting Started
- Go to your projects and create python3 as our virtual environment.

```
pip install virtualenv
virtualenv --python=path/to/your/python3_file python3_env
source python3_env/bin/activate
```
- If you are using conda:  
```
conda create -n py35 python=3.5 anaconda
source
```
- Install all of required dependencies. (Make sure to update requirements.txt timely.)  
```
pip install -r requirements.txt
```

## Summary 

### FEATURE ENGINEERING
Two main categories of features were computed on both train and test, time delta and aggregaton features. Time delta features are features related to time, for example the timestamp an IP address last seen given a time window of 24 hours. And aggregation features are the frequency of a feature or a set of features.

The intuition behind these two main categories of features is that they serve as indications of fraudulent behavior. Fraudsters here are defined as clicks done by bot or human repeatedly to scam for money while has no intention to download the mobile app. For example, given a particular IP address, if the time between clicks is very short, then it might indicates this clicks done by this IP address possibly could be fraudulent. Using common sense, a user who is interested in this app will take some time to explore. It is doubtful for this user to click on the same ad or a different ad within seconds. 

### MODELING
For modeling only LightGBM is used.

### Two main challenge: 
 
- size of data 
- given features were too generic

To overcome these two obstacles, I implemented the following methodologies:

- Hashing unique combination of features 
- applying map reduce to data aggregation

When calculating time delta features such as time last seen and first seen given a time window for unique combinations of features, hashing the combination then,  going through the data once is enough to generate these features. We first construct a dictionary-like object, store all hashed unique combinations and its timestamp. Then, we update the target timestamp for each unique combination as the function goes through the entire dataset. This method avoids using aggregation function such as group by or analytical functions which saved a lot of memory and time. Implementation of this hashing trick is possible because the original dataset is ordered with the respect to time. 

When generating aggregation features such as the count of IP address, device, channel or any combination of these features, we applied the concept map reduce. For example, we first segregate the entire dataset into batches, calculate the count of a unique IP address on each batch, then aggregate all batches to calculate the count of IP address again. This method avoids using aggregation function on the entire dataset which was much more time efficient. 









