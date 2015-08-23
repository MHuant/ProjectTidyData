# ProjectTidyData
Project assignement for the get and cleaning data coursera class

Thanks to the persons who will read this file. I tried my best.
I used the code book template for this file out the DSS community site, at  https://gist.github.com/JorisSchut/dbc1fc0402f28cad9b41 
so thanks to Joris Schut

The submit date is Sunday August 23 2015
	
Project Description from the class :

One should download stats data about measurements from captors on wearable computing about people doing 6 activities at the following site https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip and then create one R script called run_analysis.R that does the following :

1.	Merges the training and the test sets to create one data set.

2.	Extracts only the measurements on the mean and standard deviation for each measurement. 

3.	Uses descriptive activity names to name the activities in the data set

4.	Appropriately labels the data set with descriptive variable names. 

5.	From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.


So in details 

PART 1 : Merges the training and the test sets to create one data set.


Even though in the forum some people did, I did not automate the download part.
Then I used both a text editor, Excel first and R then to read the data and put it into dataframes to start.

For instance when you tried to look at the subject file into a text editor or Excel, it looked like Asian characters (sorry I do not read those languages even though I would like to - good class in coursera too but no time yet). They read correctly only into R.

Then putting the data into dataframes you could understand in which order they fit by noticing the number of observations and variables. I started with the test data (then to reproduce with the train data)
On the utmost left will go the subject_test data frame with 2947 rows, then you can put the Y (which represents the activity - you see there are 6 possibilities like the 6 activities, then put the x test, which contains the bulk of the data. I used cbind to do this.
Finally the features will serve as column names (in later part)

You do the same thing for the train dataset.
Finally you merge the 2 dataframes by using rbind since they have the same structure.

The code for part 1 is :

# PART 1 MERGE THE DATA SETS
# Set the working directory where the files have been downloaded
# First part reading the txt files and putting them into dataframes
# First part - subpart A, taking the test data
activity_labels <- read.table("activity_labels.txt", header = FALSE)
features <- read.table("features.txt", header = FALSE)
subject_test <- read.table("test/subject_test.txt", header = FALSE)
X_test <- read.table("test/X_test.txt", header = FALSE)
Y_test <- read.table("test/Y_test.txt", header = FALSE)
# Once you've loaded the first part of data you understand in which order they fit by noticing the number of observations and variables
# On the utmost left will go the subject_test data frame with 2947 rows, then you can put the Y (which represents the activity - you see there...)
# are 6 possibilities like the 6 activities, then put the x test, which contains the bulk of the data.

# Binding test columns
dftest <- cbind(subject_test, Y_test)
colnames (dftest) <- c("Subject", "Code_activity")
dftest1 <- cbind(dftest, X_test)
dim(dftest1)
# So the first file is ready. The test dataset has 2947 rows and 563 colums.
# Then it could be good to add a new column named Source if later one wants to separate the test and train after the merge.

dftest1$Source <- "Test"

# One then reproduce the reading part for the train data
subject_train <- read.table("train/subject_train.txt", header = FALSE)
X_train <- read.table("train/X_train.txt", header = FALSE)
Y_train <- read.table("train/Y_train.txt", header = FALSE)

# Binding train columns
dftrain <- cbind(subject_train, Y_train)
colnames (dftrain) <- c("Subject", "Code_activity")
dftrain1 <- cbind(dftrain, X_train)
dim(dftrain1)

# So the 2cd file is ready. The train dataset has 7352 rows and 563 colums.
# Then it could be good to add a new column named Source if later one wants to separate the test and train after the merge.

dftrain1$Source <- "Train"

# One can now merge the 2 dataframes dftest1 and dftrain1

total <- rbind(dftest1, dftrain1)

At this stage one can remove all in between files and keep only the dataframes total (result of merge) as well as features (measurements) and activity label to have something clean at the end.

PART 2

The instructions than says "Extracts only the measurements on the mean and standard deviation for each measurement", so instead of using the 561 variables, you filter the features (easy with the top left R studio panel but difficult to program then) to notice how many variables deal really with mean (53 out of 561) and std (33 out of 561). So you should have less columns for the features, ie 88 instead of 561.

To select the columns which respect this criteria, one can use the dplyr package with the select command OR since the features dataframe was organised with levels, I simplified the characters (mean and std) subsetting by using the following commands ( see forum thread https://class.coursera.org/getdata-031/forum/thread?thread_id=254 one of the reply of the TA):

matched <- grep("mean", features$V2)
matched2 <- grep("std", features$V2)
tokeep <- c(matched, matched2)
tk <- features[tokeep, ]
# One get a list named tk with V1 the variable number and V2 the measurement descriptions. Means are first and then are std. I did not sort them.
library(dplyr)

cols_mean_std <- tbl_df(tk)


Short description of the project
Study design and data processing
Collection of the raw data

Description of how the data was collected.
Notes on the original (raw) data

Some additional notes (if avaialble, otherwise you can leave this section out).
Creating the tidy datafile
Guide to create the tidy data file

Description on how to create the tidy data file (1. download the data, ...)/
Cleaning of the data

Short, high-level description of what the cleaning script does. link to the readme document that describes the code in greater detail
Description of the variables in the tiny_data.txt file

General description of the file including:

    Dimensions of the dataset
    Summary of the data
    Variables present in the dataset

(you can easily use Rcode for this, just load the dataset and provide the information directly form the tidy data file)
Variable 1 (repeat this section for all variables in the dataset)

Short description of what the variable describes.

Some information on the variable including:

    Class of the variable
    Unique values/levels of the variable
    Unit of measurement (if no unit of measurement list this as well)
    In case names follow some schema, describe how entries were constructed (for example time-body-gyroscope-z has 4 levels of descriptors. Describe these 4 levels).

(you can easily use Rcode for this, just load the dataset and provide the information directly form the tidy data file)
Notes on variable 1:

If available, some additional notes on the variable not covered elsewehere. If no notes are present leave this section out.
Sources

