# ProjectTidyData
Project assignement for the get and cleaning data coursera class

Thanks to the persons who will read this file. I tried my best
I used the code book template for this file out the DSS community site, at  https://gist.github.com/JorisSchut/dbc1fc0402f28cad9b41 
so thanks to Joris Schut

The submit date is Sunday August 23 2015
	
Project Description from the class :

One should download stats data about wearable computing about people doing 6 activities at the following site https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip and then create one R script called run_analysis.R that does the following :

1.	Merges the training and the test sets to create one data set.

2.	Extracts only the measurements on the mean and standard deviation for each measurement. 

3.	Uses descriptive activity names to name the activities in the data set

4.	Appropriately labels the data set with descriptive variable names. 

5.	From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Even though in the forum some people did, I did not automate the download part.
Then I used both a text editor, Excel first and R then to read the data and put it into dataframes to start.

For instance when you tried to look at the subject file into a text editor or Excel, it looked like Asian characters (sorry I do not read those languages even though I would like to - good class in coursera too but no time yet). They read correctly only into R.

Then putting the data into dataframes you could understand in which order they fit by noticing the number of observations and variables. I started with the test data (then to reproduce with the train data)
On the utmost left will go the subject_test data frame with 2947 rows, then you can put the Y (which represents the activity - you see there are 6 possibilities like the 6 activities, then put the x test, which contains the bulk of the data. I used cbind to do this.
Finally the features will serve as column names

But if you read properly the instructions it said "Extracts only the measurements on the mean and standard deviation for each measurement", so instead of using the 561 variables, you filter the features (easy with R studio) to notice how many variables deal really with mean (53 out of 561) and std (33 out of 561). So you should have less columns for the features, ie 88 instead of 561.


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

