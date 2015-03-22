# GettingAndCleaningDataCourseProject
This repo is created as a course project for Getting and cleaning data course.

You should create one R script called run_analysis.R that does the following. 
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for 
   each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. From the data set in step 4, creates a second, independent tidy data set 
   with the average of each variable for each activity and each subject.

# Step 1
Download the Zip Data file from below link in your working directory with name Dataset.zip

# Step 2
Unzip the file in your working directory

# Step 3
Set working directory to the location where the UCI HAR Dataset was unzipped 

# Step 4
Reading the Subject files
Reading the Activity files
Reading the Fearures files
Reading Feature Names and Activity Lables

# Step 5
Merging the training and the test data sets to create one data set

# Step 6
Assiging names to Colunms of Data Frames. This will help in extracting the mean and std of combined data

# Step 7
Merging further data after naimng the colunms

# Step 8
Extracting the Feature names where mean and std key word is present

# Step 9
Subseting the data where required clounms names are coming along with Subject and activity colunms

# Step 10
Assigning names to acitivites from activity label file

# Step 11
Labeling the dataset with appropriate descriptions

# Step 12
Creating the tidy data  data set with the average of each variable for each activity and each subject.

# Step 13
Writing the Tidy Data in text file in the working directory
