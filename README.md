# Getting_Cleaning_Data
Getting and Cleaning Data Course Project

After running script run_analysis.R you will get the data set described in CodeBook.md file.


This is the contain of run_analysis.R:

```
require("dplyr")

temp_file <- tempfile()
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(url, temp_file)
unzip(temp_file)
rm(temp_file)

# Uses descriptive activity names to name the activities in the data set
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")[,2]

# Appropriately labels the data set with descriptive variable names. 
features <- read.table("./UCI HAR Dataset/features.txt")[,2]

# Extracts only the measurements on the mean and standard deviation for each measurement. 
mean_std_colunms <- grep("mean|std", features)

# Read test data, rename and select only the measurements on the mean and standard deviation columns.
# Merge measurements data (X_test), activities data (y_test) and subject data (subject_test)
X_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
names(X_test) <- features
X_test <- X_test[, mean_std_colunms]

y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
y_test[,1] <- activity_labels[y_test[,1]]
names(y_test) <- "Activity"

subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")
names(subject_test) <- "Subject"

test_data <- cbind(subject_test, y_test, X_test)

# Read train data, rename and select only the measurements on the mean and standard deviation columns.
# Merge measurements data (X_train), activities data (y_train) and subject data (subject_train)
X_train <- read.table("./UCI HAR Dataset/train/X_train.txt")
names(X_train) = features
X_train <- X_train[, mean_std_colunms]

y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")
y_train[,1] <- activity_labels[y_train[,1]]
names(y_train) = "Activity"

subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")
names(subject_train) = "Subject"

train_data <- cbind(subject_train, y_train, X_train)

## Merges the training and the test sets to create one data set.
data <- rbind(test_data, train_data)

# creates a independent tidy data set with the average of each variable for each activity and each subject.
tidy <- data %>% group_by(Activity, Subject) %>% summarize_all(mean)
```

