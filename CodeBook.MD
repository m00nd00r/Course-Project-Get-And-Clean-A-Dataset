---
title: "CodeBook"
output: html_document
---

    The data in Samsung-Tidy-Data.txt file summarizes the measurements of the "Human Activity Recognition Using Smartphones Dataset." Please see "README.txt" accompanying that dataset's distribution for details regarding the collection of the data and see this repo's README for a description of how run_analysis works.
	
	The following data files were created by run_analysis.R:
	
	activity_labels		- a table of number keys associated with the activities:
										1 WALKING
										2 WALKING_UPSTAIRS
										3 WALKING_DOWNSTAIRS
										4 SITTING
										5 STANDING
										6 LAYING
							The numbers column was given the name ActKey and the label column given the name Activity.
							
	features			- the list of feature names for the data sets provided. This table also has number keys to identify the features.
							The keys column was given the name Key and the features column was given the name Feature.
							Additional manipulations were performed to make all the feature names syntactic R names so that they
							could be used as column names for the fully merged dataset. See the README file for how this was done.
	train_activities	- list of activities for each observation in the training set. This list was given column name ActKey
	train_data			- all of the acceleration and gyroscope data that was provided in the original dataset. This is a 
							data table with 561 columns representing the different variables and 7352 observations. Each column
							was given the corresponding name in the features table.
	subject_train		- this is a vector that identifies the subjects that correspond to each observation in the train dataset. 
							This vector column was given the heading Subject
	train				- this is the concatenated dataset that combines the train data, the subject identifiers for each data 
							observation, and the activity that each subject observation corresponds to.
	test_activities		- list of activities for each observation in the test set. This list was given column name ActKey
	test_data			- all of the acceleration and gyroscope data that was provided in the original dataset. This is a 
							data table with 561 columns representing the different variables and 7352 observations. Each column
							was given the corresponding name in the features table.
	subject_test		- this is a vector that identifies the subjects that correspond to each observation in the test dataset. 
							This vector column was given the heading Subject
	test				- this is the concatenated dataset that combines the test data, the subject identifiers for each data 
							observation, and the activity that each subject observation corresponds to.
	merged				- this is the data set of the combined train and test datasets. This dataset was then arranged first by
							Subject number, then by ActKey number.
	merged_subset		- this is a subset of merged containing only the features that are mean and standard deviation features. 
							I also remove all of the angle features as these don't actually represent mean or standard deviation
							measurements.
	merged_melt			- this is a the melted merged_subset that holds Subject, ActKey, and Activity as id labels, then stacks all of
							of the remaining mean and std features on top of each other in the Measurement column with their values
							in the Value column.
	merged_summary		- this is the tidy dataset with the mean calculated for each feature. This data set has dimensions of 
							180 x 81. The rows account for 30 subjects each being observed during 6 activities. There are 81 mean and standard
							deviation features each with their averages calculated alongside each Subject/Activity observation.
	summary_subset		- this is a truncated version of merged_summary for easier viewing of the larger tidy dataset.
	
    
	Samsung-Tidy-Data	- this the output file created from merged_summary that is the final required dataset for this assignment.
	
	Samsung-Tidy-Data-Summary		-this is a formatted version of summary_subset for easier viewing of the larger Samsung-Tidy-Data
								file.
