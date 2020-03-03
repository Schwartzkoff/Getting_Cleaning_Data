The data in this data set represents the average values of each “Activity” by “Subject” of variables that contains mean and standard deviation calculation of this other: [data set](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) .

These are the steps to get this data:

Prerequisites

```
require("dplyr")

```

First the original data set was downloaded: 

```
temp_file <- tempfile()
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(url, temp_file)
unzip(temp_file)
rm(temp_file)

```

The complete data were in different files in the "test" and "train" folders. The names of variables and activities that represent each observation for both data sets were in 'features.txt' and 'activity_labels.txt' respectively.
The activity labels and  variable names were extracted and then from the object containing variable names were selected all variable names with "mean" or "std". The result was saved in other object for use to select only the measurements on the mean and standard deviation from the whole dataset.

```
# Uses descriptive activity names to name the activities in the data set
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")[,2]

# Appropriately labels the data set with descriptive variable names. 
features <- read.table("./UCI HAR Dataset/features.txt")[,2]

# Extracts only the measurements on the mean and standard deviation for each measurement. 
mean_std_colunms <- grep("mean|std", features)

```

Fisrt the test data was read from "X_test.txt", "y_test.txt" and "subject_test.txt". The variable names were changed and all data from the 3 files was merged:
```
X_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
names(X_test) <- features
X_test <- X_test[, mean_std_colunms]

y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
y_test[,1] <- activity_labels[y_test[,1]]
names(y_test) <- "Activity"

subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")
names(subject_test) <- "Subject"

test_data <- cbind(subject_test, y_test, X_test)

```

The same was done for the data in train folder:
```
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
```

Then both data were merged:
```
## Merges the training and the test sets to create one data set.
data <- rbind(test_data, train_data)
```

Finally, all data is grouped by Activity and Subject and summarized to the average of each variable.
```
tidy <- data %>% group_by(Activity, Subject) %>% summarize_all(mean)
```


These are the variables contained in this data set:

**Activity** WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING

**Subject** person performed activities

**tBodyAcc-mean()-X** time body acceleration mean direction X

**tBodyAcc-mean()-Y** time body acceleration mean direction Y

**tBodyAcc-mean()-Z** time body acceleration mean direction Z

**tBodyAcc-std()-X** time body acceleration standard deviation direction X

**tBodyAcc-std()-Y** time body acceleration standard deviation direction Y

**tBodyAcc-std()-Z** time body acceleration standard deviation direction Z

**tGravityAcc-mean()-X** time gravity acceleration mean direction X

**tGravityAcc-mean()-Y** time gravity acceleration mean direction Y

**tGravityAcc-mean()-Z** time gravity acceleration mean direction Z

**tGravityAcc-std()-X** time gravity acceleration standard deviation direction X

**tGravityAcc-std()-Y** time gravity acceleration standard deviation direction Y

**tGravityAcc-std()-Z** time gravity acceleration standard deviation direction Z

**tBodyAccJerk-mean()-X** time body acceleration Jerk mean direction X

**tBodyAccJerk-mean()-Y** time body acceleration Jerk mean direction Y

**tBodyAccJerk-mean()-Z** time body acceleration Jerk mean direction Z

**tBodyAccJerk-std()-X** time body acceleration Jerk standard deviation direction X

**tBodyAccJerk-std()-Y** time body acceleration Jerk standard deviation direction Y

**tBodyAccJerk-std()-Z** time body  acceleration Jerk standard deviation direction Z

**tBodyGyro-mean()-X** time body Gyro mean direction X

**tBodyGyro-mean()-Y** time body  Gyro mean direction Y

**tBodyGyro-mean()-Z** time body mean direction Z

**tBodyGyro-std()-X** time body Gyro standard deviation direction X

**tBodyGyro-std()-Y** time body Gyro standard deviation direction Y

**tBodyGyro-std()-Z** time body Gyro standard deviation direction Z

**tBodyGyroJerk-mean()- X** time body Gyro Jerk mean direction X

**tBodyGyroJerk-mean()-Y** time body Gyro Jerk mean direction Y

**tBodyGyroJerk-mean()-Z** time body Gyro jerk mean direction Z

**tBodyGyroJerk-std()-X** time body  Gyro Jerk standard deviation direction X

**tBodyGyroJerk-std()-Y** time body Gyro Jerk standard deviation direction Y

**tBodyGyroJerk-std()-Z** time body Gyro Jerk standard deviation direction Z

**tBodyAccMag-mean()** time body acceleration Magnitude mean

**tBodyAccMag-std()** time body acceleration Magnitude standard deviation

**tGravityAccMag-mean()** time  gravity acceleration Magnitude mean

**tGravityAccMag-std()** time gravity acceleration Magnitude standard deviation

**tBodyAccJerkMag-mean()** time body acceleration Jerk Magnitude mean

**tBodyAccJerkMag-std()** time body acceleration Jerk Magnitude standard deviation

**tBodyGyroMag-mean()** time body Gyro Magnitude mean

**tBodyGyroMag-std()** time body Gyro Magnitude standard deviation

**tBodyGyroJerkMag-mean()** time body Gyro Jerk Magnitude mean

**tBodyGyroJerkMag-std()** time body Gyro Jerk Magnitude standard deviation

**fBodyAcc-mean()-X** Fast Fourier Transform body acceleration mean direction X

**fBodyAcc-mean()-Y** Fast Fourier Transform body acceleration mean direction Y

**fBodyAcc-mean()-Z** Fast Fourier Transform body acceleration mean direction Z

**fBodyAcc-std()-X** Fast Fourier Transform body acceleration standard deviation X

**fBodyAcc-std()-Y** Fast Fourier Transform body acceleration standard deviation direction Y

**fBodyAcc-std()-Z** Fast Fourier Transform body acceleration standard deviation direction Z

**fBodyAcc-meanFreq()-X** Fast Fourier Transform body acceleration mean Frequency direction X

**fBodyAcc-meanFreq()-Y** Fast Fourier Transform body acceleration mean Frequency direction Y

**fBodyAcc-meanFreq()-Z** Fast Fourier Transform body acceleration mean Frequency direction Z

**fBodyAccJerk-mean()-X** Fast Fourier Transform body acceleration Jerk mean direction X

**fBodyAccJerk-mean()-Y** Fast Fourier Transform body acceleration Jerk mean direction Y

**fBodyAccJerk-mean()-Z** Fast Fourier Transform body acceleration Jerk mean direction Z

**fBodyAccJerk-std()-X** Fast Fourier Transform body acceleration Jerk standard deviation direction X

**fBodyAccJerk-std()-Y** Fast Fourier Transform body acceleration Jerk standard deviation direction Y

**fBodyAccJerk-std()-Z** Fast Fourier Transform body acceleration standard deviation direction Z

**fBodyAccJerk-meanFreq()-X** Fast Fourier Transform body acceleration mean Frequency direction X

**fBodyAccJerk-meanFreq()-Y** Fast Fourier Transform body acceleration mean Frequency direction Y

**fBodyAccJerk-meanFreq()-Z** Fast Fourier Transform body acceleration mean Frequency direction Z

**fBodyGyro-mean()-X** Fast Fourier Transform body Gyro mean direction X

**fBodyGyro-mean()-Y** Fast Fourier Transform body Gyro mean direction Y

**fBodyGyro-mean()-Z** Fast Fourier Transform body Gyro mean direction Z

**fBodyGyro-std()-X** Fast Fourier Transform body Gyro standard deviation direction X

**fBodyGyro-std()-Y** Fast Fourier Transform body Gyro standard deviation direction Y

**fBodyGyro-std()-Z** Fast Fourier Transform body Gyro standard deviation direction Z

**fBodyGyro-meanFreq()-X** Fast Fourier Transform body Gyro mean Frequency direction X

**fBodyGyro-meanFreq()-Y** Fast Fourier Transform body Gyro mean Frequency direction Y

**fBodyGyro-meanFreq()-Z** Fast Fourier Transform body mean Frequency direction Z

**fBodyAccMag-mean()** Fast Fourier Transform body acceleration Magnitude mean

**fBodyAccMag-std()** Fast Fourier Transform body acceleration Magnitude standard deviation

**fBodyAccMag-meanFreq()** Fast Fourier Transform body acceleration Magnitude mean Frequency 

**fBodyBodyAccJerkMag-mean()** Fast Fourier Transform body body acceleration Jerk Magnitude mean

**fBodyBodyAccJerkMag-std()** Fast Fourier Transform body body acceleration Jerck Magnitude standard deviation 

**fBodyBodyAccJerkMag-meanFreq()** Fast Fourier Transform body body acceleration Jerk Magnitude mean Frequency

**fBodyBodyGyroMag-mean()** Fast Fourier Transform body body Gyro Magnitude mean

**fBodyBodyGyroMag-std()** Fast Fourier Transform body body Gyro Magnitude standard deviation

**fBodyBodyGyroMag-meanFreq()** Fast Fourier Transform body body Gyro Magnitude mean Frequency

**fBodyBodyGyroJerkMag-mean()** Fast Fourier Transform body body Gyro Jerk Magnitude mean

**fBodyBodyGyroJerkMag-std()** Fast Fourier Transform body body Gyro Jerk Magnitude standard deviation

**fBodyBodyGyroJerkMag-meanFreq()** Fast Fourier Transform body body Gyro Jerk Magnitude mean Frequency

