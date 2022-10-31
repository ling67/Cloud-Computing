# PageRank Theory

<img width="300" alt="image" src="https://user-images.githubusercontent.com/93315926/199075533-ecaf0d4d-45d7-4133-b285-05b0efca2edb.png">

1. If The initial PageRank value for each webpage is 1.
PR(A) = 1
PR(B) = 1
PR(C) = 1
Page B has a link to pages C and A
Page C has a link to page A
Page D has links to all three pages

2. Then
A's PageRank is: PR(A) = (1-d) + d * (PR(B) / 2 + PR(C) / 1 + PR(D) / 3)
B's PageRank is: PR(B) = (1-d) + d * (PR(D) / 3)
C's PageRank is: PR(C) = (1-d) + d * (PR(B) / 2 + PR(D) / 3)
D's PageRank is: PR(D) = 1-d
Damping factor is 0.85

3. Then after 1st iteration
**Output**
* Page B would transfer half of its existing value, or 0.5, to page A and the other half, or 0.5, to page C.
* Page C would transfer all of its existing value, 1, to the only page it links to, A.
* Since D had three outbound links, it would transfer one third of its existing value, or approximately 0.33, to A.

**Input**
PR(A)
= (1-d) + d * (PR(B) / 2 + PR(C) / 1 + PR(D) / 3)
= (1-0.85) + 0.85 * (0.5 + 1 + 0.33)
= 1.71
PR(B)
= (1-d) + d * (PR(D) / 3)
= (1-0.85) + 0.85 * 0.33
= 0.43
PR(C)
= (1-d) + d * (PR(B) / 2 + PR(D) / 3)
= (1-0.85) + 0.85 * (0.5 + 0.33)
= 0.86
PR(D)
= 1-d
= 0.15

4. You can keep iterate
....

# Observation of PageRank

A web page does not have input will have:
* constant PageRank: 1-d
* the smallest PageRank
Input Web Pages' impact to the PageRank of a web page:
* The more Input Web Pages the better.
* The higher PageRank of an Input Web Page the better.

# PageRank + PySpark + GCP

## Set up PySpark on GCP

Steps:
1. Enable the Google Cloud Compute Engine API
2. Create, Configure and Launch a Google Cloud Dataproc cluster

<img width="918" alt="image" src="https://user-images.githubusercontent.com/93315926/199079606-3a427655-e31a-4ad8-bce1-83ceb9a117cd.png">

3. Connecting to the Master Node using Secure Shell (ssh) 

<img width="612" alt="image" src="https://user-images.githubusercontent.com/93315926/199079880-20504e81-56ec-4592-b767-7aefe4efabfd.png">

> Detail Steps See Week 4 Homework 2

## Do this question using PySpark

0. Questions
<img width="161" alt="image" src="https://user-images.githubusercontent.com/93315926/199082222-a50c1257-5be9-4097-8028-53973d9db1eb.png">

1. Manually	calculate the	first	2	iteration	of	the	PageRank

* Frist	iteration
A	=	1
B	=	(1/2)	=	0.5
C	=	1	+	(1/2)	=	1.5
PageRank	(A)	=	1	– 0.85	+	0.85	*	1	=	1
PageRank	(B)	=	1	– 0.85	+	0.85	*	1 =	0.575
PageRank	(C) =	1	– 0.85	+	0.85	*	1.5	=	1.425

* Second	iteration
A	=	1
B	=	(1/2)	=	0.5
C	=	0.575	+	(1/2)	=	1.075
PageRank	(A)	=	1	– 0.85	+	0.85	*	1.425 =	1.36125
PageRank (B)	=	1	– 0.85	+	0.85	*	0.5 =	0.575
PageRank	(C) =	1	– 0.85	+	0.85	*	1.075	=	1.06375

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
spark-submit pagerank.py hdfs:///mydata/pagerank_data.txt 1
```

> 1 is the iteration count

## Result

* First iteration
<img width="400" alt="image" src="https://user-images.githubusercontent.com/93315926/199089459-b1556744-399b-481c-9706-57639d5e5964.png">

The result is same as we calculate by hand.

* Second iteration
<img width="400" alt="image" src="https://user-images.githubusercontent.com/93315926/199089698-20f3bde0-9087-4cbc-aa13-d605ae3245c2.png">

The result is same as we calculate by hand.

* Ten times iteration
<img width="623" alt="image" src="https://user-images.githubusercontent.com/93315926/199090085-c8bb2283-0207-43bb-bad1-e183114a3d1d.png">

## Shutting down the Cluster 
 

# PageRank + Scala + GCP

## Set up Scala on GCP

> The detailed steps see Week 5 Homework 2

Steps:
1.Create a Cloud Storage bucket 
<img width="600" alt="image" src="https://user-images.githubusercontent.com/93315926/199094153-6fd7c07b-5c74-462d-a06b-69f64cf0fc14.png">

2.Create a Dataproc cluster 
<img width="600" alt="image" src="https://user-images.githubusercontent.com/93315926/199095204-53426a89-84bc-414c-a9af-ff0e7cf5d1eb.png">

3. Connecting to the Master Node using Secure Shell (ssh) 

<img width="600" alt="image" src="https://user-images.githubusercontent.com/93315926/199095683-474bd92d-671a-4922-8b7a-0d538ee92193.png">

## Do this question using Scala

1.Prepare data







