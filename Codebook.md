Our resulting data frame called RecastDF has 180 rows and 81 columns. 
The rows are separated in 6 activities (2nd column, called "activity", taking the values "WALKING", "WALKING_UPSTAIRS", "WALKING_DOWNSTAIRS", 
"SITTING", "STANDING", "LAYING") for 30 individuals (1st column, integer values from 1 to 30). Indeed 6*30=180.
This is because we are monitoring 30 individuals each performing 6 activities. We measure 79 variables on each of these 180 activities. 
These 79 measurements on our 180 experiences consist in average values. This is the totality of the data set. 
To understand the meaning of the 79 column names (variables), one can refer to the initial code book, keeping in mind that we used only lower
case and deleted the "-" and "()".

The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals taccxyz and tgyroxyz. These time domain signals 
(prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth 
filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals 
(tbodyaccxyz and tgravityaccxyz) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tbodyaccjerkxyz and tbodygyrojerkxyz). Also 
the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tbodyaccmag, tgravityaccmag, tbodyaccjerkmag, tbodygyromag, 
tbodygyrojerkmag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fbodyaccxyz, fbodyaccjerkxyz, fbodygyroxyz, fbodyaccjerkmag, 
fbodygyromag, fbodygyrojerkmag. (Note the 'f' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern:  
'xyz' is used to denote 3-axial signals in the X, Y and Z directions.

tbodyaccxyz
tgravityaccxyz
tbodyaccjerkxyz
tbodygyroxyz
tbodygyrojerkxyz
tbodyaccmag
tgravityaccmag
tbodyaccjerkmag
tbodygyromag
tbodygyrojerkmag
fbodyaccxyz
fbodyaccjerkxyz
fbodygyroxyz
fbodyaccmag
fbodyaccjerkmag
fbodygyromag
fbodygyrojerkmag

The set of variables that were estimated from these signals are: 

mean: Mean value
std: Standard deviation

Since the original data was normalized between - 1 and + 1, the average resulting from step 5 (in the README file) are also comprised between -1  and +1.