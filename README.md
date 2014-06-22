Getting and Cleaning Data Course Project
========================================================
**Course**: Getting and Cleaning Data

**Author**: Jody P. Abney

###Project Description
The purpose of this project is to demonstrate the ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. 

### Submission Requirements
1. A tidy data set as described below
2. A link to a Github repository with the script for performing the analysis
3. A code book that describes the variables, the data, and any transformations or work performed to clean up the data called CodeBook.md.
4. Include a README.md in the repo with the scripts. This repo explains how all of the scripts work and how they are connected. 

#### Course Project Requirements:
Create one R script called run_analysis.R that does the following. 
1. Merges the training and the test sets to create one dataset.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the dataset
4. Appropriately labels the dataset with descriptive variable names. 
5. Creates a second, independent tidy dataset with the average of each variable for each activity and each subject. 

#### Basic Code Walkthrough for run_analysis() Function
1. Load library/package "stringr" to use the str_detect function
2. Read the source data files (activity labels, features, test, training)
3. Merge the test and training data into a single dataset
4. Check the row counts and columns match from the sources to the merged data
5. Define the readings columns to be used in the tidy dataset (e.g. those that are "means" or "standard deviations" of their data readings)
6. Set up tidy_dataset based on the required columns including the mapping of the activity text values based on the training_label data column values
7. Write out the "tidy" dataset (tidy_data) to the "./data/tidy_data.txt" file
8. Create an empty tidy_data2 dataset based on the tidy_data dataset to get the column structure
9. Get the unique subject values
10. Loop through each subject based on the unique activity values to calculate the averages of each readings column
11. Append the averages by student by activity onto the tidy_data2 dataset
12. Remap the activity text based on the training_label data
13. Relabel the now averaged reading columns by prepending their names with "Mean_"
14. Write the averaged tidy dataset (tidy_data2) to the "./data/tidy_data2.txt" file

#### Assumptions:
* R Pagkage Dependencies:
    * stringr v0.6.2 or later
* Working directory contains:
    * subdirectory called "data" which contains the unzipped files/folders from the project dataset
    	* activity_labels.txt
    	* features_info.txt
    	* features.txt
    	* README.txt
    	* /test (folder)
    	* /test/Inertial Signals (folder that contains raw data readings, not listed here)
    	* /test/subject_test.txt
    	* /test/X_test.txt
    	* /test/Y_test.txt
    * R script named "run_analysis.R" which reads in the data txt files and outputs the tidy dataset to be submitted
* No pre-cleaning/manipulation of the data is allowed

=======================================================

####Basic Code Walkthrough
```r
# Begin Function Definition #
run_analysis <- function() {
```

**Walkthrough Step 1.** Load library/package "stringr" to use the str_detect function

```r
# Prerequisite library
    library("stringr")
    # We will use the str_detect function
```
    
**Walkthrough Step 2.** Read the source data files (activity labels, features, test, training)

```r
   # Read the activity labels data
    act_labels <- read.table("./data/activity_labels.txt", 
                             col.names = c("index", "actvity_labels"))
    
    # Read the features data
    features <- read.table("./data/features.txt",
                           col.names = c("index", "feature"))
    
    # Test Data
    # Read the Test Subject data (subject number)
    sub_test <- read.table("./data/test/subject_test.txt",
                           col.names = c("subject"))
    
    # Read the X Test data (readings)
    x_test <- read.table("./data/test/X_test.txt",
                         col.names = features[,2])
    
    # Read the Y Test data (activity type)
    y_test <- read.table("./data/test/Y_test.txt",
                         col.names = c("training_label"))
    
```

**Walkthrough Step 3.** Merge the test and training data into a single dataset
```r
    # Create the Test Data table by combining columns from the individual data files
    # read above
    test_data <- as.data.frame(c(sub_test, x_test, y_test))
    
    # Get the row count of data loaded into train_data (will be used later to check)
    # the merged data success
    rows_test_data <- nrow(test_data)
    message("test_data rows loaded: ", rows_test_data)
    
    # Training Data
    # Read the Training Subject data (subject number)
    sub_train <- read.table("./data/train/subject_train.txt",
                            col.names = c("subject"))
    
    # Read the X Train data (readings)
    x_train <- read.table("./data/train/X_train.txt",
                          col.names = features[,2])
    
    # Read the Y Train data (activity type)
    y_train <- read.table("./data/train/Y_train.txt",
                          col.names = c("training_label"))
    
    # Create the Train Data table by combining columns from the individual data files
    # read above
    train_data <- as.data.frame(c(sub_train, x_train, y_train))
    
    # Get the row count of data loaded into train_data (will be used later to check)
    # the merged data success
    rows_train_data <- nrow(train_data)
    message("train_data rows loaded: ", rows_train_data)
    
    # Merge the Test and Train data
    merged_data <- merge(test_data, train_data, all=TRUE)
```

**Walkthrough Step 4.** Check the row counts and columns match from the sources to the merged data

```
   # Get the row count of data merged into merged_data (will be used later to check)
    # the merged data success
    rows_merged_data <- nrow(merged_data)
    message("merged_data rows loaded: ", rows_merged_data)
    
    message("Checking success of merged data...")
    
    # Compare the row counts of test_data + train_data to merged_data
    if((rows_test_data + rows_train_data) != rows_merged_data) {
        message("Data merge failed: row count mismatch!!!")
        message("test_data rows: ", rows_test_data, 
                " + train_dat rows: ", rows_train_data, 
                " != merged_data_rows: ", rows_merged_data)
    } else {
        message("Data merge passed: row count matches")
        message("test_data rows: ", rows_test_data, 
                " + train_dat rows: ", rows_train_data, 
                " = merged_data_rows: ", rows_merged_data)
    }
    
    if (!(setequal(names(test_data), names(merged_data))) &
            setequal(names(train_data), names(merged_data))) {
        message("Data merge failed: column names mismatch!!")
    } else {
        message("Data merge passed: column names match")
    }
```

**Walkthrough Step 5.** Define the readings columns to be used in the tidy dataset (e.g. those that are "means" or "standard deviations" of their data readings)
    
```r
    # Get the column names from the merged_data set
    columns <- names(merged_data)
    
    # Use the str_detect function from the stringr package to get the column names
    # of interest
    subjects <- str_detect(columns, "subject")
    means <- str_detect(columns, "mean")
    stds <- str_detect(columns, "std")
    trainings <- str_detect(columns, "training_label")
    
    # Return the required columns of interest from the merged_data set
    required_columns <- subjects | means | stds | trainings
    
    # Return the required data subset from the merged_data set
    required_data <- merged_data[,required_columns]
```

**Walkthrough Step 6.** Set up tidy_dataset based on the required columns including the mapping of the activity text values based on the training_label data column values
    
```r
    # Set up Activity based on match from training_label to act_labels data set
    activity <- matrix("temp", nrow = nrow(required_data), ncol = 1) # define activity data set
    for(i in 1:nrow(required_data)) {
        activity[i,1] <- as.character(act_labels[required_data$training_label[i],2])
    }
    # Bind Activity data set to required_data data set
    tidy_data <- cbind(required_data, activity)
```

**Walkthrough Step 7.** Write out the "tidy" dataset (tidy_data) to the "./data/tidy_data.txt" file

```   
    # Write out the "tidy data set" without the row.names
    write.table(tidy_data, 
                file = "./data/tidy_data.txt", 
                row.names = FALSE)
    message("tidy_data.txt file written")
```

**Walkthrough Step 8.** Create an empty tidy_data2 dataset based on the tidy_data dataset to get the column structure

```
    # Now create a 2nd Tidy Data Set per Project Requirement #5
    # Calculate average of each variable for each activity and each subject
    variables <- c(2:80) # Define the columns for the variables
    
    # Set the structure of the tidy_data set by subsetting the required data for
    # a value that ensures we get only the columns
    tidy_data2 <- subset(tidy_data, tidy_data$subject > 99)
```

**Walkthrough Step 9.** Get the unique subject values

```
    # Subset by each subject
    unique_subjects <- sort(unique(required_data$subject)) # get unique subjects
```

**Walkthrough Step 10.** Loop through each subject based on the unique activity values to calculate the averages of each readings column

```
    for(sub in unique_subjects) { # loop through each of the unique subjects
        temp_sub_data <- subset(required_data, subject == sub) # get subject data
        
        # subset by activity type
        temp_act_data <- NULL # reset the temp_act_data set just to be safe
        activities <- sort(unique(as.numeric(temp_sub_data$training_label))) # get the 
        # unique
        # activities
        for(act in activities) { # loop through each unique activity to calculate the averages
            temp_act_data <- subset(required_data, 
                                    training_label == act, 
                                    select = variables) # get activity data
            
            # calculate the averages across the variables
            means <- lapply(temp_act_data, mean)
            
            # add the means with the complete data to a temp data set
            temp <- c(temp_sub_data$subject[sub],
                      means,
                      act,
                      NA   # Just plug in "NA" for now and re-map the 
                      # activity text at end of looping
            )
            # Set up the column names for temp to match tidy_data2 column names
            names(temp) <- names(tidy_data2)
```

**Walkthrough Step 11.** Append the averages by student by activity onto the tidy_data2 dataset

```
            # Append the temp data to the tidy_data2 data set
            tidy_data2 <- rbind(tidy_data2, temp)
            
        }
        
    }
```

**Walkthrough Step 12.** Remap the activity text based on the training_label data

```
    # Map the proper activity based on the training_label value
    tidy_data2$activity <- act_labels[as.numeric(tidy_data2$training_label),2]
```

**Walkthrough Step 13.** Relabel the now averaged reading columns by prepending their names with "Mean_"

```
    # Prepend "Mean_" to the data column names to make them more meaningful
    column_names <- names(tidy_data2) # get the column names from tidy_data2
    temp_names <- str_c("Mean_", 
                        column_names[2:80]) # prepend "Mean_" to readings columns
    names(tidy_data2) <- c("subject", 
                           temp_names, 
                           "training_label", 
                           "activity") # assign the column names
```

**Walkthrough Step 14.** Write the averaged tidy dataset (tidy_data2) to the "./data/tidy_data2.txt" file

```
    # Write the tidy_data2 data set to file without row.names
    write.table(tidy_data2, 
                file = "./data/tidy_data2.txt", 
                row.names = FALSE)
    message("tidy_data2.txt file written")
```

```
# End Function Definition #
}
```

