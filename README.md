Getting-And-Cleaning-Data
<<<<<<< HEAD

 I set this R script to work outside of the UCI Har Dataset directory but within the same directory
 as the UCI folder.
 These scripts will merge the files inside the UCI folder:
	activity_labels.txt
	subject_test.txt
	subject_train.txt
	X_test.txt#	X_train.txt
	y_test.txt
	y_train.txt

 Once merged they will output the required .txt file that is a tidy data table of all the mean and standard
 deviation values for the entire merged dataset. These mean and std values are organized first by Subject
 and then by Activity:
	1 WALKING
	2 WALKING_UPSTAIRS
	3 WALKING_DOWNSTAIRS
	4 SITTING
	5 STANDING
	6 LAYING
 The values for the mean and std for each of the variables is listed in the same row as each Subject/Activity pair.

 Because the course assignment requires that we output a .txt file for this tidy data set using write.table, it's
 difficult to verify that this is in fact a tidy data set without reading everything into R first. So I created
 Summary table showing only the variables for the first 3 variables in the full data set arranged accordign to 
 Subject and Activity. This file is called Samsung-Tidy-Date-Summary.txt.

 The rest of this ReadMe file shows the scripts that I wrote with explanations of how they work.


 ***************************************************************************
 ***************************************************************************

 Start by making sure the current system has all the necessary packages are loaded.

library(dplyr); library(data.table); library(reshape2)



 ***************************************************************************
 ***************************************************************************

 read "activity_labels.txt" into a data frame and change its column
 names to ActKey and Activity

activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")
names(activity_labels) <- c("ActKey", "Activity")

 read "features.txt" into a data frame and change its column names to Key and Feature

features <- read.table("./UCI HAR Dataset/features.txt")
names(features) <- c("Key", "Feature")

 since the Feature column of features will be used as column names for the final table they will need
 to be converted to characters with the - , ( and ) all removed.

features$Feature <- as.character(features$Feature)

 Need to modify the feature names so that they are syntactic R names.
 Start by turning features into a data.table.
 Then break the the data.table into two different data.tables to handle the angle variables
 separately as they have more parantheses in them.
 Start by removing the parentheses from the first dt, then replace the dashes and commas 
 with underscores.

features_dt<-tbl_dt(features)

features_dt1<-slice(features_dt,1:554) 
features_dt2<-slice(features_dt,555:561)

features_dt1$Feature<-gsub("\\(|\\)","",features_dt1$Feature)
features_dt1$Feature<-gsub("-|,","_",features_dt1$Feature)

features_dt2$Feature<-sub("\\(","_",features_dt2$Feature)
features_dt2$Feature<-sub(",","_",features_dt2$Feature)
features_dt2$Feature<-gsub("\\)","",features_dt2$Feature)

features<-rbind(features_dt1,features_dt2)



 ***************************************************************************
 ***************************************************************************

 Read the training activities file (y_train.txt) and name the column
 "ActKey"

train_activities <- read.table("./UCI HAR Dataset/train/y_train.txt",sep="")

names(train_activities) <- "ActKey"

 Read the training data (X_train.txt) and use the features$Feature vector to name the columns.
 Read the training subject data (subject_train.txt) and name the subject column "Subject"

train_data <- read.table("./UCI HAR Dataset/train/X_train.txt", sep = "",col.names=features$Feature)

names(train_data) <- c(features$Feature)

subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt", sep = "")

names(subject_train) <- "Subject"

 merge the three parts of the training data set (subject, activity, data) 
 into a new data frame named "train" using cbind

train <- cbind(subject_train, train_activities, train_data)



 ***************************************************************************
 ***************************************************************************

 Repeat previous steps for the test set.

test_activities <- read.table("./UCI HAR Dataset/test/y_test.txt", sep="")

names(test_activities) <- "ActKey"

test_data <- read.table("./UCI HAR Dataset/test/X_test.txt", sep = "",col.names=features$Feature)

names(test_data) <- c(features$Feature)

subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt", sep = "")

names(subject_test) <- "Subject"

test <- cbind(subject_test, test_activities, test_data)



 ***************************************************************************
 ***************************************************************************

 Merge the "train" and "test" data frames using dplyr function bind_rows

merged<-bind_rows(train,test)

 Add the activity labels by performing a left join (using the
 "left_join" function from the dplyr package)

merged <- left_join(activity_labels, merged, by = "ActKey")



 ***************************************************************************
 ***************************************************************************

 Sort the merged data frame by Subject then Activity using the "arrange" function
merged <- arrange(merged, Subject, ActKey)

 Subset the merged data frame to only the relevant columns ("Subject,"
 "Activity," and any of the remaining columns with "Mean," "mean," or
 "std" in their name
merged_subset <- merged[,grepl("ActKey|Subject|Activity|[Mm]ean|std",names(merged))]

 Subset merged_subset further by removing the "angle" vectors as none of them represent 
 means or standard deviations even though they have mean and std in their names.

merged_subset<-select(merged_subset,-(starts_with("angle")))



 ***************************************************************************
 ***************************************************************************

 Using the "melt" function from the reshape2 package, melt the merged
 data frame to create a long-form version of the data set

merged_melt <- melt(merged_subset, id.vars=c("Subject", "ActKey", "Activity"),
                    variable.name = "Measurement", value.name = "Value")

 Create a summary data frame with the average (mean) of each variable
 for each Subject/Activity combination using the dcast function from
 the "reshape2" package.

merged_summary <- dcast(merged_melt, Subject + Activity~Measurement,
                        mean, value.var = "Value")

 Create a subset of merged_summary for easy display in a text file.

summary_subset<-select(merged_summary,1:7)

 ***************************************************************************
 ***************************************************************************

 Write the merged_summary data frame to a text file in the working
 directory called "Samsung-Tidy-Data.txt" using the argument
 (row.names = FALSE) from the assignment
write.table(merged_summary, "Samsung-Tidy-Data.txt", row.names = FALSE)

 Write a formatted output version of the summary_subset dt for easy viewing of the full data set.

capture.output(print(summary_subset,print.gap=3),file="Samsung-Tidy-Data-Summary.txt")
=======
>>>>>>> 918271b3bdc8dbe0c05c686d19ad422930e05ea9


