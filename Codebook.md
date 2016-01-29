---
title: "CodeBook"
author: "Xiao"
date: "January 29, 2016"
output: html_document
---
1.Download data and load packages.
```{r, eval = FALSE}
# Download data and set work directory.
url<-("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip")
download.file(url,"data.zip")
unzip("data.zip")
setwd("./UCI HAR Dataset")
library(reshape)
```
2.Load data into R.
```{r, eval = FALSE}
# Load data and name dataset
activitylabels <-  read.table("activity_labels.txt")
features <-  read.table("features.txt")    
subjecttrain <-  read.table("train/subject_train.txt")
subjecttest <-  read.table("test/subject_test.txt")
xtrain <- read.table("train/X_train.txt")
xtest <-  read.table("test/X_test.txt")
ytrain <-  read.table("train/y_train.txt")
ytest <-  read.table("test/y_test.txt")
```
3.Merge data in R.
```{r, eval = FALSE}
# Merge training and test data
totalsubject <- rbind(subjecttest, subjecttrain)
totalx <- rbind(xtest, xtrain)
totaly <- rbind(ytest, ytrain)
names(totalsubject) <- c("subject")
names(totalx) <- features$V2
names(totaly) <- c("Activity")
totaldata <- cbind(totalsubject,totaly,totalx)
```
4.Extract data in R.
```{r, eval = FALSE}
# Extract Mean and Sd.
meanx <- grep("mean()",colnames(totaldata))
sdx <- grep("std()",colnames(totaldata))
meansdx <- totaldata[,c(1,2,meanx,sdx)]
```
5. Getting tidy data and output.
```{r, eval = FALSE}
# Using melt getting tidy data
meansdxmelt <- melt(meansdx,id =c("subject","Activity"))
tidydata <- dcast(meansdxmelt, subject + Activity ~ variable, mean)

# Output tidy data
write.table(tidydata,file = "tidydata.txt",row.names = FALSE,sep=",")
```
