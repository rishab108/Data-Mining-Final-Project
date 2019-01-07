# Yelp Hybrid Recommender System

The goal of this project is to create a hyrbid recommendation system using the Yelp [dataset](https://www.yelp.com/dataset) to recommend businesses to users.

# Data 

* The data is a subset of Yelp Dataset Challenge.
* We chose data from 2 cities i.e. Pittsburgh and Las Vegas which can be found [here](https://drive.google.com/drive/folders/1Q6zdfU_k1DJ3I963AlT6KQTGgZNLATR-?usp=sharing)


# 1. Collaborative Filtering

Collaborative-Filtering (CF) Based Recommendation techniques have been quite successful in the past giving promising results in this domain. The main idea behind these set of algorithms is that the users with similar interests may like similar items. These algorithms unlike Content-Based approaches don’t require the content(features) of the items we want to recommend to users but instead only requires ratings various users have given to various items. Based on just this information, it is able to predict rating a user may give to some item it has not rated. Moreover, it helps in recommending new items to users which he may like.

### Prerequisites

* Scala 2.11 
* Spark 2.3.1 (Mllib, Core, SQL)
* Java 1.8

### Instructions

To run the collaborative filtering models follow the steps below, 
1. save the `Collaborative_Models.jar` in the bin folder in spark. 
2. open terminal and go to ~/spark/bin/
3. run the following command,

```
spark-submit --class <class_name> Collaborative_Models.jar <train_file_path> <test_file_path> <city> <run_mode>
```

* <class_name> : ItemBased_CF / UserBased_CF / als
* <train_path> : entire path to the training data
* <test_path> : entire path to the testing data
* <run_mode> : 0 (will generate only test predictions) / 1 (will generate all the predictions test, train and all)
 

# 2. Content Based

Content based recommendation systems try to find similarity between users and items by building their corresponding vectors. The most important aspect of such models is choosing features. We follow three different approaches to build user and item features. These features are transformed into vectors and then different similarity measures can be used to find compute the similarity between them. Unlike collaborative recommendation systems, in content-based models we used text in the review to form user and item vectors.

### Prerequisites

* Numpy
* Pandas
* Scikit-learn

* Install `findspark` using the following command
```python
pip install findspark
```

* Install Gensim for doc2vec
```python
pip install gensim
```

* Install nltk for pre-processing
```python
pip install nltk
```

### Instructions

* In order to run the code for any content based model give the path for `train_file` and `test_file` under `Load Train and Test Data`
* Change the parameters for `TfidfVectorizer` under `TF-IDF Vector Creation` or `Doc2Vec` under `Doc2Vec Model Creation`. (optional)
* Hypertune the parameters for regression under `Training Regressor on similarity values` if the data is changed. (optional)
* Give the path and name of file to save test results under `Save predictions file`.

# 3. Hybrid Recommender System

The final model combines all the predictions generated from the above-mentioned models by their weighted sum. The CF model uses the ratings, while the content-based model uses reviews or categories to capture the similarity between users and businesses to predict the unknown rating. The combination of all the models can grasp different aspects of the relationship between users and businesses to give a more accurate prediction.

### Instructions
* To generate the predictions give the `test_ground_truth` and test_predictions(test prediction files generated using each one of the content based and collaborative based models) as program arguments to the `HybridModelRMSE.scala` in the following order (order is important as the weights of the models are hardcoded).

1. city_test.txt
2. <city>_ItemBased_test_predictions.txt
3. <city>_UserBased_test_predictions.txt
4. <city>_ALS_test_predictions.txt
5. <city>_SGD_test_predictions.txt
6. <city>_TextBased_test_predictions.txt
7. <city>_ReviewBased_test_predictions.txt
8. <city>_CategoryBased_test_predictions.txt


This will generate a resultant RMSE of the combined models along with a text file `Hybrid_Model_predictions.txt` which contains predictions of rating corresponding to each user business pair in test file.

* to generate recommendations corresponding to a user give 4 program arguments to  `HybridModelRecommendation.scala` in the following order. 

1. Hybrid_Model_predictions.txt
2. ReviewBased_Sorted_similarities.txt
3. UserBased_Sorted_similarities.txt
4. TextBased_Sorted_similarities.txt

This will generate a text file of all the recommendations for userset in the test file. First item of each row is the user_id and the following items in that row are the business ids of the recommendations.  
