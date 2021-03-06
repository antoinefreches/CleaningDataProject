CleaningDataProject
===================

The run_analysis.R code works as follow:
After opening the run_analysis.R file in your R or RStudio, first please set the working 
directory as the directory where the raw data for this project is located.

This run_analysis.R file does the following:

##1. Merges the training and the test sets to create one data set.

### First we deal with the training set.
We start with the x_train.txt data, which we read in using the read.table function and which we store in the variabe called Xtrain.
Xtrain is a data frame of 7352 rows and 561 columns. It is comprised of numeric values between -1 and +1. Variables have no explicit
names. Only V1, V2 etc...

Then we deal with the y_train.txt data, which we read in using the read.table function and which we store in the variabe called Ytrain.
Ytrain is a data frame of 7352 rows and 1 column. It is comprised of integer values between 1 and 6. The variable has no explicit
name. Only V1.

Then we deal with the subject_train.txt data, which we read in using the read.table function and which we store in the variabe called 
Subtrain.
Subtrain is a data frame of 7352 rows and 1 column. It is comprised of 21 different integer values between 1 and 30. The variable has 
no explicit name. Only V1.

We now have 3 data sets in the variables Xtrain, Ytrain and Subtrain. We merge them together using the simple cbind function. 
Indeed we simply start by putting Ytrain to the right of Xtrain (since the number of rows is the same). The combined data frame is 
called firstMerge. We then put Subtrain to the right of the firstMerge data frame (same number of rows again), to create secondMerge. 
All our data of interest is hence stored in secondMerge. 

### Then we deal with the test set.
We start with the x_test.txt data, which we read in using the read.table function and which we store in the variabe called Xtest.
Xtest is a data frame of 2947 rows and 561 columns. It is comprised of numeric values between -1 and +1. Variables have no explicit
names. Only V1, V2 etc...

Then we deal with the y_test.txt data, which we read in using the read.table function and which we store in the variabe called Ytest.
Ytest is a data frame of 2947 rows and 1 column. It is comprised of integer values between 1 and 6. The variable has no explicit
name. Only V1.

Then we deal with the subject_test.txt data, which we read in using the read.table function and which we store in the variabe called 
Subtest.
Subtest is a data frame of 2947 rows and 1 column. It is comprised of 9 different integer values between 1 and 30. The variable has 
no explicit name. Only V1.

We now have 3 data sets in the variables Xtest, Ytest and Subtest. We merge them together using the simple cbind function. 
Indeed we simply start by putting Ytest to the right of Xtest (since the number of rows is the same). The combined data frame is 
called firstMerge2. We then put Subtest to the right of the firstMerge2 data frame (same number of rows again), to create secondMerge2. 
All our data of interest is hence stored in secondMerge2. 

### We merge the train set with the test set.
We simply use the rbind function to put the secondMerge2 below the seconMerge data frame. Indeed these 2 data frames have the same number 
of columns. The name of the resulting data frame is combDF. It contains all the data of interest. combDF has 10299 rows and 563 column, 
which is expected. 

##2. Extracts only the measurements on the mean and standard deviation for each measurement. 

We read-in the variable names which are located in the features.txt file. We again use the read.table function and load the data in a 
variable called myFeatures. 
This is a data frame with 561 rows and 2 columns. The second column is the one of interest, it gives the names of the 561 first columns 
of our combDF. The type of data is factor. 
We first extract and store in varNames the second column of myFeatures. We use the as.character function to convert the factors into 
character string. 
We then use the grepl function to transform this character vector into a vector a of logical values (TRUE/FALSE). The value is TRUE if the 
corresponding value in myFeatures contains either mean() or std(). 
We add 2 logical values (TRUE TRUE) at the end of our a vector. The vector is now called b.
We can then subset out of the columns of combDF only the columns which contain mean() or std(). To do that, we use our b vector to subset 
the columns. The resulting data frame is called combDF2. Instead of 563 columns, it has now only 81 columns. It has still the same number
of rows as combDF (10299).

##3. Uses descriptive activity names to name the activities in the data set

We again use the read.table function to read the activity_labels.txt file. The data is stored in tableActivity. This is a simple table of 
6 rows and 2 columns establishing a correspondance between our y column (the ones that contains integer values ranging from 1 to 6) and 
the activity names that these numbers represent. Those numbers are in the column 80 of combDF2. 
To replace the non descriptive numbers by their meaning, we start by taking only the second column of tableActivity, which we remame 
tableActivity2. tableActivity is simply a vector of 6 factors describing the activites ("walking" "standing" etc...). These are stored 
as factor variables. We convert that into characters using the as.character function, and put this in tableActivity3.
To actually perform the replacement, we use the sapply function on the column 80 of combDF2, replacing with the corresponding subset of 
tableActivity3. The resulting variable is called activityNames. It is a vector of 10299 characters with the explicit activity names in the 
same order of the orginal column 80 of combDF2.
Finally, we simply re-assign the 80th column to the vector activityNames.  

##4. Appropriately labels the data set with descriptive variable names. 

We now use the grep function on the same varNames vector as before, specifying value=TRUE. This allows us to not only extract only the 79 names
which contain std() or mean(), but also to explicitely store those names in a new variable called c. That variable contains the same names
as the previous variable a, but instead of a logical it is a character vector with the explicit names, not the logical that where pointing 
to the names of varNames.
We can perform some cleaning manipulations on c, like replacing the "-" or "()" by empty values. This is done using the gsub function and results 
in a character vector called e. We apply the tolower() function to e, which gives us the vector f. This is in order to have only non-capital
letters in our names. We could have used more explicit names like replacing std by "standarddeviation" but we disagree with this approach and prefer
to keep reasonably short names for our columnds. 
Finally, we concatenate "activity" and "individual" to our 79 names. These are going to be the 81 names of our 81 colmumns in combDF2. This gives 
us a vector g, which we simply assign to the names of combDF2. 

##5. From the data set in step 4, creates a second, independent tidy data set with the average of each 
variable for each activity and each subject.

We load the reshape2 library in order to use the melt and dcast functions. 
We create a new data frame called MeltDF, which the result of applying the melt function to combDF2, specifying that the 2 ids are "activity" and 
"individual", and that the variables are the columns corresponding to the names vector f (i.e. all the other columns). 
MeltDF is now a slim data frame, with 813621 rows and 4 columns.
We use the dcast function to create RecastDF, where all individuals and activities have been summarized with their mean values. 
The resulting data frame (RecastDF), had 180 rows and 81 columns. 


