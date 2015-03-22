# GettingAndCleaningDataCourseProject
This repo is created as a course project for Getting and cleaning data course.

###############################################################################

### Course : Getting and Cleaning Data
### Assignment: Course Project

## You should create one R script called run_analysis.R that does the following. 
##  1. Merges the training and the test sets to create one data set.
##  2. Extracts only the measurements on the mean and standard deviation for 
##     each measurement. 
##  3. Uses descriptive activity names to name the activities in the data set
##  4. Appropriately labels the data set with descriptive variable names. 
##  5. From the data set in step 4, creates a second, independent tidy data set 
##     with the average of each variable for each activity and each subject.

###############################################################################

### Download the Zip Data file from below link in your working
### directory with name Dataset.zip

fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl,destfile="Dataset.zip")

### Unzip the file in your working directory

unzip(zipfile="Dataset.zip")

### Set working directory to the location where the UCI HAR Dataset was unzipped
setwd(paste(getwd(),"UCI HAR Dataset", sep="/"))

### Reading the Subject files

dataSubjectTrain <- read.table(file.path(getwd(), "train", "subject_train.txt"),header = FALSE)
dataSubjectTest  <- read.table(file.path(getwd(), "test" , "subject_test.txt"),header = FALSE)

### Reading the Activity files

yDataActivityTest  <- read.table(file.path(getwd(), "test" , "Y_test.txt" ),header = FALSE)
yDataActivityTrain <- read.table(file.path(getwd(), "train", "Y_train.txt"),header = FALSE)


### Reading the Fearures files

xDataFeaturesTest  <- read.table(file.path(getwd(), "test" , "X_test.txt" ),header = FALSE)
xDataFeaturesTrain <- read.table(file.path(getwd(), "train", "X_train.txt"),header = FALSE)


### Reading Feature Names and Activity Lables

FeaturesNames <- read.table(file.path(getwd(), "features.txt"),head=FALSE)
activityLabels <- read.table(file.path(getwd(), "activity_labels.txt"),header = FALSE)

### Merging the training and the test data sets to create one data set

mDataSubject <- rbind(dataSubjectTrain, dataSubjectTest)
mDataActivity<- rbind(yDataActivityTrain, yDataActivityTest)
mDataFeatures<- rbind(xDataFeaturesTrain, xDataFeaturesTest)

### Assiging names to Colunms of Data Frames. This will help in extracting the
### mean and std of combined data

names(mDataSubject)<-c("subject")
names(mDataActivity)<- c("activity")
names(mDataFeatures)<- FeaturesNames$V2

### Merging further data after naimng the colunms

mDataCombine <- cbind(mDataSubject, mDataActivity)
mData <- cbind(mDataFeatures, mDataCombine)

### Extracting the Feature names where mean and std key word is present

MeanStdFeatureNames <- FeaturesNames$V2[grepl("mean|std", FeaturesNames$V2)]

#### Subseting the data where required clounms names are coming along with
### Subject and activity colunms

requiredNames<-c(as.character(MeanStdFeatureNames), "subject", "activity" )
mData2 <-subset(mData,select=requiredNames)

### Assigning names to acitivites from activity label file

activity.ID = 1
for (ActivityLabel in activityLabels$V2) {
  mData2$activity <- gsub(activity.ID, ActivityLabel, mData2$activity)
  activity.ID <- activity.ID + 1
}

###Labeling the dataset with appropriate descriptions

names(mData2)<-gsub("^t", "time", names(mData2))
names(mData2)<-gsub("^f", "frequency", names(mData2))
names(mData2)<-gsub("Acc", "Accelerometer", names(mData2))
names(mData2)<-gsub("Gyro", "Gyroscope", names(mData2))
names(mData2)<-gsub("Mag", "Magnitude", names(mData2))
names(mData2)<-gsub("BodyBody", "Body", names(mData2))

### Creating the tidy data  data set with the average of each variable 
### for each activity and each subject.

TData<-aggregate(. ~subject + activity, mData2, mean)
TData<-TData[order(TData$subject,TData$activity),]


### Writing the Tidy Data in text file

write.table(TData, file = "tidydata.txt",row.name=FALSE)
