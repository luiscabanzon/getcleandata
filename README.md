---
title: "Getting and Cleaning Data 2"
author: "Luis Cabanzon"
date: "24/11/2014"
output: html_document
---
# GETTING AND CLEANING DATA
# PEER ASSESTMENT
===============================================================
Kindly find below the code used to transform the original dataset, extracted from
[https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) :

```{r eval =FALSE}
## 1.a : READING DATA
features = read.table("/home/honulele/R_env/GetData2/data/features.txt", header = FALSE)
labels = read.table("/home/honulele/R_env/GetData2/data/activity_labels.txt", header = FALSE)

x_test <- read.table("/home/honulele/R_env/GetData2/data/test/X_test.txt", header = FALSE)
y_test <- read.table("/home/honulele/R_env/GetData2/data/test/y_test.txt", header = FALSE)
subject_test <- read.table("/home/honulele/R_env/GetData2/data/test/subject_test.txt", header = FALSE)

x_train <- read.table("/home/honulele/R_env/GetData2/data/train/X_train.txt", header = FALSE)
y_train <- read.table("/home/honulele/R_env/GetData2/data/train/y_train.txt", header = FALSE)
subject_train <- read.table("/home/honulele/R_env/GetData2/data/train/subject_train.txt", header = FALSE)

# 1.b : NAMING COLUMNS
colnames(labels) <- c("activity_id", "activity_name")

colnames(x_test) <- features[,2]
colnames(y_test) <- "activity_id"
colnames(subject_test) <- c("subject_id")

colnames(x_train) <- features[,2]
colnames(y_train) <- "activity_id"
colnames(subject_train) <- c("subject_id")

# 1.c : MERGIN DATASETS
test <- cbind(y_test, subject_test, x_test)
train <- cbind(y_train, subject_train, x_train)

data <- rbind(test,train)

# 2 : EXTRACTING COLUMNS WITH MEANS AND STANDARD DESVIATIONS
mean_std_cols <- grep("-(mean|std)\\(\\)", features[,2])
mean_and_std <- data[,c(1,2,(mean_std_cols+2))]
colnames(mean_and_std)[3:ncol(mean_and_std)] <- as.character(features[mean_std_cols,2])

# 3 : ADDING DESCRIPTIVE ACTIVITY NAMES
data$activity_id <- labels[data$activity_id,2]

# 4 : LABELING DATA WITH DESCRIPTIVE VARIABLE NAMES
# Done on step 1.b

# 5 : CREATING NEW DATA SET WITH AVERAGE VALUR PER ACTIVITY AND SUBJECT
library(plyr)
average_per_activity_subject <- ddply(data, .(activity_id, subject_id),
                                      function(x) colMeans(x[,2:68]))

write.table(average_per_activity_subject, "average_per_activity_subject.txt", row.name = FALSE)

```
