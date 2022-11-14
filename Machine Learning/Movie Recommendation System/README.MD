# Description


# Run the ipynb file on Colab 

## Step 1: Study PySpark Collaborative Filtering with ALS

## Step 2: Study Colab

## Step 3: Experiment Pyspark code (ipynb) of PySpark Collaborative Filtering with ALS

1. First upload the data to the running space, then install pyspark in Google Colab

<img width="1000" alt="image" src="https://user-images.githubusercontent.com/93315926/201575436-1e4559f4-517e-423b-9d47-e9629aa0a71b.png">

2. Save the modified ipynb file as py format

3. Save the modified ipynb file as HTML format which can be used on Step 5 of this project


# Run the py file saved on GCP

## 1 Set up PySpark on GCP

Steps:
1. Enable the Google Cloud Compute Engine API
2. Create, Configure and Launch a Google Cloud Dataproc cluster

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/199079606-3a427655-e31a-4ad8-bce1-83ceb9a117cd.png">

3. Connecting to the Master Node using Secure Shell (ssh) 

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/199079880-20504e81-56ec-4592-b767-7aefe4efabfd.png">

> Detail Steps See Week 4 Homework 2

2. Prepare Data in HDFS 

* Manual input data
```
vi pagerank_data.txt
```

Data
```
A B
A C
B C
C A
```

* create a directory (folder) to store the data: 
```
hdfs dfs -mkdir hdfs:///mydata 
```

```
hdfs dfs -put pagerank_data.txt hdfs:///mydata
```

* To verify that the file is indeed located in the mydata folder, run the following command:  
```
hdfs dfs -ls hdfs:///mydata 
```

3. Prepare the program

The code is on the above py file.
```
vi pagerank.py
```

4. Running the program with Pyspark 

```
spark-submit pagerank.py
```

5. Result


# Shutting down the Cluster 
 
 <img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/199174391-dc21b7f3-20ea-434f-a494-98d8507fae5b.png">

# Detail Design Presentation 

[PageRank using Spark](https://docs.google.com/presentation/d/1lProy-vwvsE6qNtg2iTOGM1fvHHBbh1QKhV9KT7d2xU/edit#slide=id.g25f6af9dd6_0_0)
