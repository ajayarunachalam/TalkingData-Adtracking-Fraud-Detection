### TalkingData AdTracking Fraud Detection Challenge ###

# AIM:- 
-------
Predict whether a user will download an app after clicking a mobile app advertisement / Simply can you detect fraudulent click traffic for mobile app ads?  

# About Public Dataset:-
------------------------

This dataset provides historical information of the user app activities originating from clicking advertisement. The dataset is quite interesting and unique because of highly imbalance distribution of classes & rare data events.

Note:-
The dataset is downloaded from the link provided in the reference section below.

# Attributes description #
--------------------------

# ip: ip address of click.
# app: app id for marketing.
# device: device type id of user mobile phone (e.g., iphone 6 plus, iphone 7, samsung, etc.)
# os: os version id of user mobile phone
# channel: channel id of mobile ad publisher
# click_time: timestamp of click (UTC)
# attributed_time: if user download the app for after clicking an ad, this is the time of the app download
# is_attributed: the target/outcome that is to be predicted, indicating the app was downloaded

# Note that ip, app, device, os, and channel are encoded.

# ML Model:-
------------

Let us explore this dataset with R. Specifically, I am interested in evaluation of the LightGBM model. LightGBM is a distributed high performance gradient boosting algorithm based on decision trees. It is available for both Python & R. 


# Prerequisite for installing lightGBM on Windows system in R :-
---------------------------------------------------------------

# To summarize what worked for me on Windows 10/R-Studio

1) Install Rtools - 64 bit https://cran.r-project.org/bin/windows/Rtools/
2) Install/Update to R 64 bit version >= 3.4.0 
3) Install CMAKE https://cmake.org/download/ (64-bit)
4) MinGW / Visual Studio / gcc (depending on your OS and your needs) with make in PATH environment variable
5) devtools::install_github("Microsoft/LightGBM", subdir = "R-package")


Installing LightGBM:-
---------------------
1) In R console type:-

install.packages("devtools")
devtools::install_github("Laurae2/lgbdl", force = TRUE)
devtools::install_github("Microsoft/LightGBM", subdir = "R-package")

or alternate method

Click 'clone or download' on https://github.com/Microsoft/LightGBM and save as .zip
Extract the .zip contents ('LightGBM-master') to C:\LightGBM-master
Run cmd as administrator and cd to C:\LightGBM-master\R-package
Run 'R CMD INSTALL --build . --no-multiarch' from the same cmd terminal.

2) Add the following (or their equivalents on your system) to your PATH environment variable:- (Start -> Control Panel -> System -> Advanced System Settings)

C:\Users\AppData\Local\Programs\Git\mingw64\bin;
C:\RBuildTools\3.4\bin; 
C:\RBuildTools\3.4\mingw_64\bin;
C:\Program Files\CMake\bin\cmake;
C:\Users\Documents\R\R-3.4.4\bin

WorkFlow:-
----------
For evaluating the performance of lightGBM model, i have used the public available dataset from kaggle. 
The full data set has 184 million observations. I have carried out the data pre-processing, feature engineering, data analysis, modelling, model evaluation and making predictions steps. I have tested the model on 18 million records. The dataset is loaded with pacman approach. Then the data is preprocessed & new features are derived with chaining & piping.
Chaining and piping in layman's term is just the easy of data manipulation that can be achieved with chunk. One can chain multiple functions together by taking the output of one function and inserting it into the next. In short, "chaining" means that you pass an intermediate result onto the next. We chain and pipe the training data for the following.
  
# 1. Read the 184M observations in training data
# 2. drop/deselect attributed_time
# 3. Extract weekday and hour as a new features from click_time
# 4. drop/deselect click_time
# 5. Add count features with various grouping
# 6. Drop IP address column
# 7. Store the resulting data to object named **train** which is assigned in the beginning with **<-** operator. 

# - **%>%** is called pipe operator and works exactly like '|'operator in unix. 
# Pipes take the output from one function and feed it to the first argument of the next function. 
# - add_count() function in dplyr is useful for groupwise filtering. 
# It adds a column "n" to a table based on the number of items within each existing group as shown in the code chunk above. 

The same procedure is then carried out for test data. And, further the data analysis is done. Since IP addresses seems to be dynamic or possibly fake so i have omitted this attribute from model training directly. Here, we dropped IP address, attributed time and click time columns after utilizing frequency counts from IP addresses and extracting weekday and hour from click time done as a part of feature engineering. The new derived features signify the count of ip address with respect to weekday-hour, hour-channel, hour-os, hour-app, hour-device. Then we prepare the list of unique values by each feature in train & test data. The next task is visualizing the most frequent values by Top Apps, Top OS, Top Channels, Top Devices, Top Hours, Top Weekdays etc. Similarly, we visualizing the same for the cases when the app is downloaded. Then, next we check the distribution of the classes. To evaluate the model performance before making any prediction, we create validation set (on 5% of training data) on which we can measure the model performance and can make educated guess on generalizing it on unseen (test) data.
From total observations, we are taking first 95% rows as training and last 5% as validation data. Then we prepare the data for modeling. We build the model & get feature importance by gain, cover and frequency. Then the prediction is done for unseen(test) data. 


Conclusion:-
------------
We see that the lightGBM package is more robust and aims to improve the modeling time. 


Reference:-
----------- 
https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/
https://github.com/Microsoft/LightGBM/

Reference Paper:-
----------------
Guolin Ke, Qi Meng, Thomas Finley, Taifeng Wang, Wei Chen, Weidong Ma, Qiwei Ye, and Tie-Yan Liu. 
"LightGBM: A Highly Efficient Gradient Boosting Decision Tree". In Advances in Neural Information Processing Systems (NIPS), pp. 3149-3157. 2017.


Acknowledgment
--------------
[1] https://github.com/Microsoft/LightGBM/issues/




