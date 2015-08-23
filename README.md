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

PART 2 Extracts only the measurements on the mean and standard deviation for each measurement

So instead of using the 561 variables, you filter the features (easy with the top left R studio panel but difficult to program then) to notice how many variables deal really with mean (53 out of 561) and std (33 out of 561). So you should have less columns for the features, ie 88 instead of 561.

To select the columns which respect this criteria, one can use the dplyr package with the select command and transform dataframes into tables first

library(dplyr)
tidy <- tbl_df(total)

Since the features dataframe was organised with levels, I simplified the characters (mean and std) subsetting by using the following commands ( see forum thread https://class.coursera.org/getdata-031/forum/thread?thread_id=254 one of the reply of the TA):

matched <- grep("mean", features$V2)
matched2 <- grep("std", features$V2)
tokeep <- c(matched, matched2)
tk <- features[tokeep, ]
# One get a list named tk with V1 the variable number and V2 the measurement descriptions. Means are first and then are std. I did not sort them.

cols_mean_std <- tbl_df(tk)

One then subset the tidy table only with the columns that are in tk.


PART 4	Appropriately labels the data set with descriptive variable names.

So from the subsetting done in part 2, we have less variables than the 561 initial ones.

The variables are as follow :

Code	V2	Name
V1	1	tBodyAcc-mean()-X
V2	2	tBodyAcc-mean()-Y
V3	3	tBodyAcc-mean()-Z
V41	41	tGravityAcc-mean()-X
V42	42	tGravityAcc-mean()-Y
V43	43	tGravityAcc-mean()-Z
V81	81	tBodyAccJerk-mean()-X
V82	82	tBodyAccJerk-mean()-Y
V83	83	tBodyAccJerk-mean()-Z
V121	121	tBodyGyro-mean()-X
V122	122	tBodyGyro-mean()-Y
V123	123	tBodyGyro-mean()-Z
V161	161	tBodyGyroJerk-mean()-X
V162	162	tBodyGyroJerk-mean()-Y
V163	163	tBodyGyroJerk-mean()-Z
V201	201	tBodyAccMag-mean()
V214	214	tGravityAccMag-mean()
V227	227	tBodyAccJerkMag-mean()
V240	240	tBodyGyroMag-mean()
V253	253	tBodyGyroJerkMag-mean()
V266	266	fBodyAcc-mean()-X
V267	267	fBodyAcc-mean()-Y
V268	268	fBodyAcc-mean()-Z
V294	294	fBodyAcc-meanFreq()-X
V295	295	fBodyAcc-meanFreq()-Y
V296	296	fBodyAcc-meanFreq()-Z
V345	345	fBodyAccJerk-mean()-X
V346	346	fBodyAccJerk-mean()-Y
V347	347	fBodyAccJerk-mean()-Z
V373	373	fBodyAccJerk-meanFreq()-X
V374	374	fBodyAccJerk-meanFreq()-Y
V375	375	fBodyAccJerk-meanFreq()-Z
V424	424	fBodyGyro-mean()-X
V425	425	fBodyGyro-mean()-Y
V426	426	fBodyGyro-mean()-Z
V452	452	fBodyGyro-meanFreq()-X
V453	453	fBodyGyro-meanFreq()-Y
V454	454	fBodyGyro-meanFreq()-Z
V503	503	fBodyAccMag-mean()
V513	513	fBodyAccMag-meanFreq()
V516	516	fBodyBodyAccJerkMag-mean()
V526	526	fBodyBodyAccJerkMag-meanFreq()
V529	529	fBodyBodyGyroMag-mean()
V539	539	fBodyBodyGyroMag-meanFreq()
V542	542	fBodyBodyGyroJerkMag-mean()
V552	552	fBodyBodyGyroJerkMag-meanFreq()
V4	4	tBodyAcc-std()-X
V5	5	tBodyAcc-std()-Y
V6	6	tBodyAcc-std()-Z
V44	44	tGravityAcc-std()-X
V45	45	tGravityAcc-std()-Y
V46	46	tGravityAcc-std()-Z
V84	84	tBodyAccJerk-std()-X
V85	85	tBodyAccJerk-std()-Y
V86	86	tBodyAccJerk-std()-Z
V124	124	tBodyGyro-std()-X
V125	125	tBodyGyro-std()-Y
V126	126	tBodyGyro-std()-Z
V164	164	tBodyGyroJerk-std()-X
V165	165	tBodyGyroJerk-std()-Y
V166	166	tBodyGyroJerk-std()-Z
V202	202	tBodyAccMag-std()
V215	215	tGravityAccMag-std()
V228	228	tBodyAccJerkMag-std()
V241	241	tBodyGyroMag-std()
V254	254	tBodyGyroJerkMag-std()
V269	269	fBodyAcc-std()-X
V270	270	fBodyAcc-std()-Y
V271	271	fBodyAcc-std()-Z
V348	348	fBodyAccJerk-std()-X
V349	349	fBodyAccJerk-std()-Y
V350	350	fBodyAccJerk-std()-Z
V427	427	fBodyGyro-std()-X
V428	428	fBodyGyro-std()-Y
V429	429	fBodyGyro-std()-Z
V504	504	fBodyAccMag-std()
V517	517	fBodyBodyAccJerkMag-std()
V530	530	fBodyBodyGyroMag-std()
V543	543	fBodyBodyGyroJerkMag-std()

At this stage a txt file as been created and submitted to Coursera.

PART 5

Need to do the matrix with the mean using the dplyr grouping functions (like a "Tableau croisé dynamique" - sorry don't know the name in English - in Excel
