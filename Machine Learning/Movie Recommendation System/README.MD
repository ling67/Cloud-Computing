# Description


# Run the ipynb file on Colab 

## Step 1: Study PySpark Collaborative Filtering with ALS

Reference:
[PySpark Collaborative Filtering with ALS](https://towardsdatascience.com/build-recommendation-system-with-pyspark-using-alternating-least-squares-als-matrix-factorisation-ebe1ad2e7679)

## Step 2: Study Colab
* Google is quite aggressive in AI research. Over many years, Google developed AI framework called TensorFlow and a development tool called Colaboratory. Today TensorFlow is open-sourced and since 2017, Google made Colaboratory free for public use. Colaboratory is now known as Google Colab or simply Colab.

* Another attractive feature that Google offers to the developers is the use of GPU. Colab supports GPU and it is totally free. The reasons for making it free for public could be to make its software a standard in the academics for teaching machine learning and data science. It may also have a long term perspective of building a customer base for Google Cloud APIs which are sold per-use basis.

* Irrespective of the reasons, the introduction of Colab has eased the learning and development of machine learning applications.

Reference:[Google Colab Tutorials](https://www.tutorialspoint.com/google_colab/what_is_google_colab.htm)

## Step 3: Experiment Pyspark code (ipynb) of PySpark Collaborative Filtering with ALS

1. First upload the data to the running space, then install pyspark in Google Colab

<img width="700" alt="image" src="https://user-images.githubusercontent.com/93315926/201575436-1e4559f4-517e-423b-9d47-e9629aa0a71b.png">

2. Result

<img width="400" alt="image" src="https://user-images.githubusercontent.com/93315926/201595020-e4472760-69bb-45a2-a3a3-f7f68fe23a47.png">

3. Save the modified ipynb file as py format

<img width="700" alt="image" src="https://user-images.githubusercontent.com/93315926/201593607-6bce388d-9ddb-4baa-a3d0-48bc78db6906.png">

4. Save the modified ipynb file as HTML format which can be used on Step 5 of this project

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/201593706-b01415cf-cbe1-433d-8343-8f2fd36b654d.png">

# Run the py file saved on GCP

## 1.Set up PySpark on GCP

Steps:
1. Enable the Google Cloud Compute Engine API
2. Create, Configure and Launch a Google Cloud Dataproc cluster

<img width="800" alt="image" src="https://user-images.githubusercontent.com/93315926/203155230-58a0dec9-2fcf-4b13-b362-bcbff8cf150e.png">

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/203155584-2eb65963-42af-45ef-b412-01b28901dcf9.png">

3. Connecting to the Master Node using Secure Shell (ssh) 

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/203155868-77a2ba38-54d9-4e03-854a-d52ad995b4b5.png">


> Detail Steps See Week 4 Homework 2

## 2. Prepare Data in HDFS 

* movies.csv
* rating.csv
* tags.csv
* recommendation_engine_movielens.py

1.Upload movies.csv and ratings.csv files at GCP

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/203156392-caf40188-b798-4423-8679-654e2d410801.png">

2. create a directory (folder) to store the data: 

```
hdfs dfs -mkdir hdfs:///mydata 
hdfs dfs -put movies.csv hdfs:///mydata
hdfs dfs -put ratings.csv hdfs:///mydata
```

* To verify that the file is indeed located in the mydata folder, run the following command:  
```
hdfs dfs -ls hdfs:///mydata 
```
<img width="800" alt="image" src="https://user-images.githubusercontent.com/93315926/203157136-facc897d-57ad-44a7-84e0-56b202eb1b6e.png">

## 3.Prepare the program

The code is on the above py file. Upload the file to GCP.
```
vi recommendation_engine_movielens.py
```
<img width="800" alt="image" src="https://user-images.githubusercontent.com/93315926/203157622-6e75f911-2518-466a-88da-bc3ba31aa756.png">

## 4.Running the program with Pyspark 

```
spark-submit recommendation_engine_movielens.py
```

## 5.Result

<img width="800" alt="image" src="https://user-images.githubusercontent.com/93315926/203168655-2aff6cf7-50e5-4b53-b29f-fc90dabb6f24.png">

# Shutting down the Cluster 
 
 <img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/199174391-dc21b7f3-20ea-434f-a494-98d8507fae5b.png">

# Detail Design Presentation 

[Movie Recommendation System](https://docs.google.com/presentation/d/1lQb6fVTNrRmfF3GYxRXxFfJCima171QhtwF7ISW0zXg/edit#slide=id.p)

