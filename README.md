---
title: "Project Course Getting and Cleaning Data"
author: "Silverio Neves"
date: "4 de Setembro de 2018"
output: html_document
---

Data set home page : http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Data set : https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

##-------- Global files ------

read activity labels table (var activitylabels)

> activitylabels <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/activity_labels.txt", quote="\"", comment.char="")

6 obs of 2 variables

rename variables of the activilylables table

> names(activitylabels) <- c("activityid","activity")

read features table (var features)

> features <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/features.txt", quote="\"", comment.char="")

561 obs of 2 variables

rename variable of features table

> names(features) <- c("featureid","feature")

## ------ TEST values -------

read features values (var x_test)

> x_test <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/test/X_test.txt", quote="\"", comment.char="")

2497 obs of 561 variables

rename variable with the values of the column feature (number 2) of features table

> names(x_test) <- features[,2]

read activity observations (var y_test)

> y_test <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/test/y_test.txt", quote="\"", comment.char="")

2947 obs of 1 variable

for cycle to replace value of y_test by the description (activity variable in activitylabels)

> for(n in 1:2947) y_test[n,1] <- as.character(activitylabels[y_test[n,1],2])

rename variable 

> names(y_test) <- c("activity")

read usersid observations

> subject_test <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/test/subject_test.txt", quote="\"", comment.char="")

rename variable

> names(subject_test) <- c("userid")

put 3 tables in one x_test + y_test + subject_test -> datasettest

> datasettest <- cbind(subject_test, y_test, x_test)

## ------ TRAIN values -------

read features values (var x_test)

> x_train <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/train/X_train.txt", quote="\"", comment.char="")

7352 obs of 561 variables

rename variable with the values of the column feature (number 2) of features table

> names(x_train) <- features[,2]

read activity observations (var y_train)

> y_train <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/train/y_train.txt", quote="\"", comment.char="")

7352 obs of 1 variable

for cycle to replace value of y_test by the description (activity variable in activitylabels)

> for(n in 1:7352) y_train[n,1] <- as.character(activitylabels[y_train[n,1],2])

rename variable 

> names(y_train) <- c("activity")

read usersid observations

> subject_train <- read.table("~/Particular/DataScience_Specialization/Course_3/Final_Project/UCI HAR Dataset/train/subject_train.txt", quote="\"", comment.char="")

rename variable

> names(subject_train) <- c("userid")

put 3 tables in one x_test + y_test + subject_test -> datasettest

> datasettrain <- cbind(subject_train, y_train, x_train)


## ----- Final Dataset -------

merge rows of the two datasets (datasettest, datasettrain) into a new database datasettemp 

> datasettemp <- rbind(datasettrain, datasettest)

10299 (2947 + 7352) obs of 563 variables

subseting columns with mean and std and the first two (userid and activity)

collect columns with mean

> vmean <- grepl("mean", names(datasettemp))

collect colums with std

> vstd <- grepl("std", names(datasettemp))

create the final vector

> vfinal <- vmean | vstd

userid column

> vfinal[1] <- TRUE

activity column

>vfinal[2] <- TRUE

> datasetfinal <- datasettemp[,vfinal]

10229 obs of 81 variables

## ----- Second Final Dataset ----

Groupby activity and userid(subject)

> groupbas <- datasetfinal %>% group_by(activity, userid)

summarises the average of each variable for each activity and each subject of datasetfinal

second datasetfinal secdatasetfinal

> secdatasetfinal <- summarise_all(groupbas, mean)

180 obs of 81 variables
