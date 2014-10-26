
*********************************************************************************************************************
1.Merges the training and the test sets to create one data set.
*********************************************************************************************************************

First we deal with the training set. We read-in the x_train data which we store in "Xtrain".
We have a look and get familiar with the data.
We then do the same with the y_train data Which we store in Ytrain, and the subject_train data which goes
into SubTrain. 

We then perform 2 first "merging operations". We start by merging the Xtrain data with the the YTrain 
(this creates firstMerge) and then we add the Subtrain to create secondMerge.

We repeat the same process for the test data. 
We create a variable Xtest that stores the data coming from x_test.txt.
We create a variable Ytest that stores the data coming from y_test.txt.
We create a variable Subtest that stores the data coming from subject_test.txt.

We now perform the 2 same first "merging operations". 
# I.e. simply putting together the x_test data with the the y_test and the sub_test data

firstMerge2=cbind(Xtest,Ytest)
dim(firstMerge2)
secondMerge2=cbind(firstMerge2,Subtest)
dim(secondMerge2)

# Now let's simply append the test data frame below the train data frame 

combDF=rbind(secondMerge,secondMerge2)
dim(combDF)

# *********************************************************************************************************************
# 2.Extracts only the measurements on the mean and standard deviation for each measurement.
# *********************************************************************************************************************

# We read-in the variable names

myFeatures=read.table("./features.txt")
dim(myFeatures)
myFeatures[1:10,]

varNames=as.character(myFeatures[,2])  #  We transform those names as characters instead of factors

a=grepl("std()|mean()",varNames) # this is a logical vector telling which name contains either std() or mean()
sum(a)
b=c(a,c(TRUE,TRUE)) # we want to keep the last 2 columns as well (Activity and Individual)
sum(b) # This is going to be our number of columns after subsetting
length(b) 
dim(combDF) # we check that the dimensions match

# Now we do the actual subsetting operation
combDF2=combDF[,b] # We only keep the colums whose names contain either std() or mean()
dim(combDF2)

# *********************************************************************************************************************
# 3.Uses descriptive activity names to name the activities in the data set
# *********************************************************************************************************************

tableActivity=read.table("./activity_labels.txt")
tableActivity
tableActivity2=tableActivity[,2]
tableActivity2
class(tableActivity2)
tableActivity3=as.character(tableActivity2)
tableActivity3
class(tableActivity3)
length(tableActivity3)

# We now replace the integers from 1 to 6 which used to describe activity names by their more explicit names

activityNames=sapply(combDF2[,80],function (x) tableActivity3[x])
head(activityNames)
tail(activityNames)
combDF2[1:10,79:81]
combDF2[,80] = activityNames
combDF2[1:10,79:81]

# *********************************************************************************************************************
# 4.Appropriately labels the data set with descriptive variable names
# *********************************************************************************************************************

names(combDF2)
c=grep("std()|mean()",varNames,value=TRUE)
head(c,50)
length(c)
d=c(c,c("Activity","Individual"))
names(combDF2)=d
combDF2[1:10,77:81]

# *********************************************************************************************************************
# 5.From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
# *********************************************************************************************************************

library(reshape2)
MeltDF=melt(combDF2,id=c("Activity","Individual"),measure.vars=c)
head(MeltDF)
RecastDF=dcast(MeltDF,Individual+Activity~variable,mean)
RecastDF[,1:5]
class(RecastDF)
dim(RecastDF)