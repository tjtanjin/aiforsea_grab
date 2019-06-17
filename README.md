# Grab: AI For SEA
This repository contains work pertaining to the Grab AI For SEA competition organized by Grab.

## Flow Summary
A beginner to machine learning, I approached the task with the following simple steps:
- Data Exploration
- Data Cleaning
- Data Processing
- Model Training
- Model Predicting

## Data Exploration
For data exploration, the two main files to analyze were the labels file and features file. A quick look at the labels file revealed that there were 18 duplicate IDs with mismatching labels. Considering the possibility that the features might have been hard to classify even manually (by humans), I decided to remove the duplicated IDs from both the labels file and the features file.

The features data was split up somewhat equally into 10 csv files. I had the initial thought that they were purposefully done to help us split train and testing data. However, with each file containing 20000 IDs, I decided to combine them into one large csv file before sorting them to look for whole patterns. The Accuracy column and Speed column stood out in particular with what I considered bad data.

## Data Cleaning
For data cleaning, I was working on the Accuracy column and Speed column which I had identified from the initial data exploration. After testing a couple of percentile values, I chose to keep Accuracy values up to the 99th percentile, which sits nicely at a value of 50. With a plan in mind to drop the inaccurate columns, I was also afraid that certain IDs might be left with far too little rows to be of use in training. As such, I included a condition where if an ID is unable to keep at least 80% of its current rows, the ID will be dropped entirely.

A similar approach was done for removing bad data from the Speed column with the difference being that instead of using percentile, I had negative speed considered to be bad data.

## Data Processing
With majority of the bad data out of the way, I moved on to processing and preparing my data to feed into my training model. For the 3 acceleration and 3 gyro columns, I calculated their magnitudes into separate columns before dropping the original columns, reducing the columns from 6 to 2. The Bearing column was dropped as well as I felt that gyro would do a good job representing the car rotations and angles during the drive. 

Unfortunately, due to time constraints and mostly limited experience on my side, I am unable to explore more creative ways of generating meaningful features for training the model. The final 5 columns I settled on as training features were Accuracy, Speed, second(time), acceleration and gyro. I then obtained the final training values for each ID by pivoting the table to take the mean value of Accuracy and max values of the remaining 4 columns.

## Model Training
For the training of the model, I tried the K Nearest Neighbors (KNN) and XgBoost algorithm. For the KNN algorithm, there was little parameters to adjust apart from finding the best n_neighbors value. On the other hand, there were many parameters that could be adjusted for XgBoost. In particular, due to having an unbalanced dataset of 3:1, there was a need to change parameters like scale_pos_weight. 

Despite decent accuracies that gave me the impression of good model performance from both algorithms, the f1_score revealed otherwise. The model was unable to identify quite a significant portion of dangerous driving, especially for the KNN algorithm. I believe the bad model performance stems from my poor data processing techniques and would certainly love to re-explore my approach in future as well as test out other algorithms.

## Model Prediction
For this final part, I simply tested the model by loading it and passing in a random set of test data to ensure that predictions are generated. Once I verified that I had prediction results, I know the model is functioning!

## Usage
For the purpose of evaluating my model, the test data will need to be put through 2 of the above 5 steps. An important assumption made here is that the test data contains the exact same columns as in the provided features csv file. To obtain prediction results for the test data, first pass the test data file through the data processing step.

Following which, pass the processed file into the model prediction file and the results should be appended as a new 'labels' column and saved to a final_results csv file.

## Conclusion
To sum up my experience, this has been a very interesting competition for me. While I have used csv file for machine learning before, this was the first time I received a dataset with multiple rows of features across time for each ID. While I was unable to produce anything impressive for this competition, it had still been a very enjoyable learning experience. In future when I have more free time, I would certainly wish to explore other ways of processing the data as well as try out other algorithms!
