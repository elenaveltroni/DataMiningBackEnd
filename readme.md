# INSTALLATION
The following commands assume the working directory is the root of the project.
## Docker installation
0. needed docker installed on machine
1. ```docker-compose build```
2. run each command as ./python (to not run the python installed on machine but the python inside docker)
## On machine installation
0. needed python 3 installed on machine
1. ```pip install -r requirements.txt```
2. ```python download.py```
## Import to MongoDB
Using docker:  
``` docker-compose run flask python app/excel_to_mongo/import_all.py ```  

Without docker:  
It's important check if MongoDB it's up and set the env variable DB to the link to the MongoDB service (probabily: localhost).  
The MongoDB service, in this development phase, must have the following credential:  
1. user: root
2. password: password
So, run:
``` python app/excel_to_mongo/import_all.py ```  

# PROJECT
## Overview
The project born with the collaboration with the Faculty of Economy.
The main goal of the project was to implement an application in which the
user can perform clustering and classification analysis on company acquisition
articles.
In particular, we pre-processed the data provided by the colleagues in the Fac-
ulty of Economics, performing a data cleanup in order to prepare data to send
to the classifiers and to the clustering algorithm.
The result we obtain can be useful in trading and others analysis performed by
Faculty of Economy.
The users can perform the analysis with a simple web app described below.

## Dataset
### Data provided
The dataset is composed by some selected sentences in documents that talk
about the company acquisitions.
The starting point are articles from the economic sector dealing with company
acquisitions. The article is not taken in its totality, but it is divided into sentences, the decision to select a sentence is made by the subjective feeling which
the sentence give to the user, only for these sentences a class is assigned.
When a sentence is selected the user classified it with a label indicating if the
sentence is Negative, Neutral or Positive according to the user feeling.
This job is done by the colleagues in the Faculty of Economy that provide us
an excel file containing the labeled sentences.
In this file we had to perform some cleaning tasks, such as delete sentences
written in Italian (the almost totality of the sentences was in English) and
make the file interpretable for a Python script.
The first version of the dataset provided to us was highly imbalanced, this
caused, in the first version of the classification algorithms, poor classification
performances.
So with the objective to improve the classification performances the dataset was
balanced.

### Data from twitter
The dataset of this application also contains a part of data retrieved from Twitter.
Twitter makes available to external users offcial APIs to search for tweets within
the platform, restricting the search for tweets by specifying some parameters,
but they show a big limit that is the possibility to retrieve only tweets published in the previous 7 days. To get around this limitation, the GetOldTweets3
library for Python has been used, a library that allows to search by username
or keyword, and allows us to specify the period of the tweets wanted.


## Preprocessing
There not exists algorithm that works with sentences directly so we have to get
numbers from sentences that represent the sentences to fit the algorithms.
This text elaboration was performed with two different methods and then compared to choose the best one:
1. Bag of Words
2. Bidirectional Encoder Representations from Transformers
From the analysis made the BERT algorithm works better in this case and therefore the classification was made using this algorithm.

## Classification
We perform classiffcation using 3 algorithms: K-NN, SVM, Logistic Regression.
These algorithms was trained using the 768 features returned by the Sentence Transformers (using the model bert-base-nli-mean-tokens) and the class presented in the dataset.

## Clustering
Clustering was performed using an agglomerative and hierarchical clustering with different connection metrics and affinity measurements based on the features returned by BERT.
In this application the main linkage inter-cluster metrics were chosen: single linkage, full linkage and pair group average, and 3 different similarity measurements to measure the distance between clusters: Euclidean, Manhattan and cosine.
Clustering results are shown with dendograms.
