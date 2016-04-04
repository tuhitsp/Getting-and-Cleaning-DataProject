Getting-and-Cleaning-DataProject
Course Project

Introduction
The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
Here are the data for the project:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
You should create one R script called run_analysis.R that does the following:

Merges the training and the test sets to create one data set.
Extracts only the measurements on the mean and standard deviation for each measurement.
Uses descriptive activity names to name the activities in the data set
Appropriately labels the data set with descriptive variable names.
From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
##Step 1-Download data

fileurl = 'https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip'

##Step 2- Read and convert data


data.train.x <- read.table('./UCI HAR Dataset/train/X_train.txt')//has X_train.txt
data.train.activity <- read.csv('./UCI HAR Dataset/train/y_train.txt', header = FALSE, sep = ' ') //has Y_train.txt
data.train.subject <- read.csv('./UCI HAR Dataset/train/subject_train.txt',header = FALSE, sep = ' ')//has tain subject

data.train <-  data.frame(data.train.subject, data.train.activity, data.train.x) //combining data
names(data.train) <- c(c('subject', 'activity'), features) //renaming columns
##same is done for test data


##Step 3-Combine X_train, y_train, subject_train, X_test, y_test and subject_test into a new data frame data.test, and rename the colname of new data frame.

##Step 4-Merges the training and the test sets to create one data set
data.all <- rbind(data.train, data.test)
##Step 5-Merge the train and test data into data.allwith the rbind command.

##Step 6-Extracts only the measurements on the mean and standard deviation for each measurement?
col.select <- grep('mean|std', features)
data.sub <- data.all[,c(1,2,col.select + 2)]

Select all the columns that represent mean or standard deviation of the measurements with grep order.

##Step 7-Uses descriptive activity names to name the activities in the data set

Replacing numeric labels of activity in column 2 of the data frame (from 1 to 6) by descriptive strings which come from the file activity_labels.txt.

##Appropriately labels the data set with descriptive variable names
###name.new <- names(data.sub)
###name.new <- gsub("[(][)]", "", name.new)
###name.new <- gsub("^t", "TimeDomain_", name.new)
###name.new <- gsub("^f", "FrequencyDomain_", name.new)
###name.new <- gsub("Acc", "Accelerometer", name.new)
###name.new <- gsub("Gyro", "Gyroscope", name.new)
###name.new <- gsub("Mag", "Magnitude", name.new)
###name.new <- gsub("-mean-", "_Mean_", name.new)
###name.new <- gsub("-std-", "_StandardDeviation_", name.new)
###name.new <- gsub("-", "_", name.new)
###names(data.sub) <- name.new
Rename the colname of data.sub with gsub order.

##Step 8-Creates a second, independent tidy data set with the average of each variable for each activity and each subject.
data.tidy <- aggregate(data.sub[,3:81], by = list(activity = data.sub$activity, subject = data.sub$subject),FUN = mean)
write.table(x = data.tidy, file = "data_tidy.txt", row.names = FALSE)
##Output tidy data as data.tidy
