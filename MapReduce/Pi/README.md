
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

## Setup passphraseless ssh
Now check that you can ssh to the localhost without a passphrase:
```
  $ ssh localhost
```
If you cannot ssh to localhost without a passphrase, execute the following commands:
```
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```

## Prepare input data
```
  $ mkdir PiCalculation
  $ cd PiCalculation
  $ vi GenerateRandomNumbers.java
  $ javac GenerateRandomNumbers.java
  $ java -cp . GenerateRandomNumbers
```

Input data will store in PiCalculationInput

## Make the HDFS directories required to execute MapReduce jobs(Copy input data to HDFS)
```
  $ bin/hdfs dfs -mkdir /user
  $ bin/hdfs dfs -mkdir /user/lchen
  $ bin/hdfs dfs -mkdir /user/lchen/picalculate
  $ bin/hdfs dfs -mkdir /user/lchen/picalculate/input
  $ bin/hdfs dfs -put ../PiCalculation/PiCalculationInput /user/lchen/picalculate/input
```

## Prepare code

* Build PiCalculation java file
```
  $ cd hadoop
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
  $ bin/hadoop jar wc.jar PiCalculation /user/lchen/picalculate/input /user/lchen/picalculate/output3
```

* Output
```
  $ bin/hdfs dfs -ls /user/lchen/picalculate/output3
  $ bin/hdfs dfs -cat /user/lchen/picalculate/output3/part-r-00000 
```

* Stop
```
  $ sbin/stop-dfs.sh
```

## Test Result

<img width="332" alt="image" src="">

## Detail Design Presentation
[Registration_System_Ling_Chen](https://docs.google.com/presentation/d/1fKrEHRzQL_7SV7ZmFwpuIFqvs5zGvqN0Bel_uh_A2tg/edit#slide=id.g25f6af9dd6_0_0)


