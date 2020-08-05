# Code Book

## The following actions are applied to the original data:
* Clear the current variables `rm(list=ls())`

* Download the zip file: [https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) to `data.zip`

* Unzip data file: `data.zip` to `UCI HAR Dataset`

* Read the data and load to the workspace
`activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))`
`features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))`
`subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")`
`subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")`
`x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)`
`x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)`
`y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")`
`y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")`


* Merge  all train and test data into single dataset: `data_merged`
`x <- rbind(x_train, x_test)`
`y <- rbind(y_train, y_test)`
`subject <- rbind(subject_train, subject_test)`
`data_merged <- cbind(subject, y, x)`


* Extracts only the measurements on the mean and standard deviation for each measurement
`data_clean <- data_merged %>% select(subject, code, contains("mean"), contains("std"))`




* Use descriptive activity names to name the activities in the data set
`data_clean$code <- activities[data_clean$code, 2]`


* Appropriately labels the data set with descriptive variable names.
`names(data_clean)[2] = "activity"`
`names(data_clean)<-gsub("Acc", "Accelerometer", names(data_clean))`
`names(data_clean)<-gsub("Gyro", "Gyroscope", names(data_clean))`
`names(data_clean)<-gsub("BodyBody", "Body", names(data_clean))`
`names(data_clean)<-gsub("Mag", "Magnitude", names(data_clean))`
`names(data_clean)<-gsub("^t", "Time", names(data_clean))`
`names(data_clean)<-gsub("^f", "Frequency", names(data_clean))`
`names(data_clean)<-gsub("tBody", "TimeBody", names(data_clean))`
`names(data_clean)<-gsub("-mean()", "Mean", names(data_clean), ignore.case = TRUE)`
`names(data_clean)<-gsub("-std()", "STD", names(data_clean), ignore.case = TRUE)`
`names(data_clean)<-gsub("-freq()", "Frequency", names(data_clean), ignore.case = TRUE)`
`names(data_clean)<-gsub("angle", "Angle", names(data_clean))`
`names(data_clean)<-gsub("gravity", "Gravity", names(data_clean))`

* From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
`data_clean2 <- data_clean %>%`
`  group_by(subject, activity) %>% summarise_all(list(mean))`
`write.table(data_clean2, "data_clean2.txt", row.name=FALSE)`

