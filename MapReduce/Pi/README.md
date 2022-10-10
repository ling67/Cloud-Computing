
# Description

* Register system is used by almost application. It is normally the first step to use the application.
* We use Python Web Server implement a Register system. 
* We also want provide this system to remote access.

# Related Theory
* Simple HTTP Server With CGI Scripts Enabled
* Sending Emails With Python
* Let anyone in the world to access your Web Server set up at home
* Database Access with Python

# Implement

## Requirment

* GCP Environment
<img width="600" alt="image" src="https://user-images.githubusercontent.com/93315926/194799644-6b303972-e90e-4fc4-821b-0b26e2df9a6d.png">

* hadoop environment
<img width="600" alt="image" src="https://user-images.githubusercontent.com/93315926/194799724-14031ad3-43db-4f36-a668-faf70a279365.png">

* java environment

## Prepare input data
```
  $ mkdir PiCalculation
  $ cd PiCalculation
  $ vi GenerateRandomNumbers.java
  $ javac GenerateRandomNumbers.java
  $ java -cp . GenerateRandomNumbers
```

Input data will store in PiCalculationInput

## Setup passphraseless ssh
Now check that you can ssh to the localhost without a passphrase:
```
  $ cd hadoop-3.3.4/
  $ ssh localhost
```
If you cannot ssh to localhost without a passphrase, execute the following commands:
```
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```

## Make the HDFS directories required to execute MapReduce jobs(Copy input data to HDFS)
```
  $ cd ..
  $ cd hadoop-3.3.4/
  $ bin/hdfs namenode -format
  $ sbin/start-dfs.sh
  $ wget http://localhost:9870/
  $ bin/hdfs dfs -mkdir /user
  $ bin/hdfs dfs -mkdir /user/lchen
  $ bin/hdfs dfs -mkdir /user/lchen/picalculate
  $ bin/hdfs dfs -mkdir /user/lchen/picalculate/input
  $ bin/hdfs dfs -put ../PiCalculation/PiCalculationInput /user/lchen/picalculate/input
```
> If you can not copy input into hadoop dictionary, please restart the virtual machine.

## Prepare code

* Build PiCalculation java file
```
  $ cd /hadoop-3.3.4
  $ vi PiCalculation.java      
```

* Compile PiCalculation.java and create a jar
```
  $ bin/hadoop com.sun.tools.javac.Main PiCalculation.java
  $ jar cf wc.jar PiCalculation*class  
```

## Run

* Execute
```
  $ bin/hadoop jar wc.jar PiCalculation /user/lchen/picalculate/input /user/lchen/picalculate/output5
```

* Output
```
  $ bin/hdfs dfs -ls /user/lchen/picalculate/output5
  $ bin/hdfs dfs -cat /user/lchen/picalculate/output5/part-r-00000 
```

* Stop
```
  $ sbin/stop-dfs.sh
```

## Test Result

<img width="700" alt="image" src="https://user-images.githubusercontent.com/93315926/194802159-668eb99d-39c7-4feb-b0ee-1921e827bf41.png">

## Detail Design Presentation
[Registration_System_Ling_Chen](https://docs.google.com/presentation/d/1SfhsLRsk71G_YDLw-eBnDyk6nwd0qdZ1rLrxOv_LQGE/edit#slide=id.g25f6af9dd6_0_0)


