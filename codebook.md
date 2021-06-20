title "Coursera week 4 course project notebook"

1. Estabilsh training data and test data paths stored in 
  xtrainpath, ytrainpath, subtrainpath
  xtestpath, ytestpath, subtestpath
2. Etablish paths for activities list and features list store in
  activitiespath, featurespath
3. Store relevent files in data tables stored as
  xtrain, ytrain, subtrain, xtest, ytest, subtest, activities, features
4.Rename the columns
  xtrain and xtest column lables were stored in the featrues table in the 
  second column. Performed a colname substitution using the second colum from 
  the features table
  ytrain and ytest included the activity number reference so I added a lable as
  "activity"
  subtest and subtrain are references to the subjec id number and were labeled as "subject"
5. Data was then merged with cbind to add the subject number and activity number to the dataset
6. Training and test data were combined with rbind to add the two seperate tables together
7. The data was then paired down to only columns with mean or std in the column name 
8.  The remaining columns were then cleaned up with substitutions to create a 
  more consistent labeling schema
9.  The activity number was substituted for the activity name from the activity.txt file
10.  The file was then grouped_by Subject and Activity and summarized
11. Output data stored in a clean .txt file with the composite data


##pull in tables for training data
xtrainpath<-"./peerreview/UCI HAR Dataset/train/X_train.txt"
ytrainpath<-"./peerreview/UCI HAR Dataset/train/y_train.txt"
subtrainpath<-"./peerreview/UCI HAR Dataset/train/subject_train.txt"
xtrain<-read.table(xtrainpath, header=FALSE)
ytrain<-read.table(ytrainpath, header=FALSE)
subtrain<-read.table(subtrainpath, header=FALSE)

##pull in tables for test
xtestpath<-"./peerreview/UCI HAR Dataset/test/X_test.txt"
ytestpath<-"./peerreview/UCI HAR Dataset/test/y_test.txt"
subtestpath<-"./peerreview/UCI HAR Dataset/test/subject_test.txt"
xtest<-read.table(xtestpath, header=FALSE)
ytest<-read.table(ytestpath,header=FALSE)
subtest<-read.table(subtestpath, header=FALSE)

#pull in activity lables table
activitiespath<-"./peerreview/UCI HAR Dataset/activity_labels.txt"
activities<-read.table(activitiespath, header=FALSE)

#pull in features list
featurespath<- "./peerreview/UCI HAR Dataset/features.txt"
features<-read.table(featurespath, header=FALSE )

#assigncolumn names 
colnames(xtrain)<- features[,2]
colnames(ytrain)<-"activity"
colnames(xtest)<-features[,2]
colnames(ytest)<-"activity"
colnames(subtest)<-"subject"
colnames(subtrain)<-"subject"
colnames(activities)<-c("activity","type")

#merge data set
testdata<-cbind(xtest, ytest, subtest)
traindata<-cbind(xtrain, ytrain, subtrain)
combinedata<-rbind(testdata,traindata)

#selects out columns with avg and std in title as well as the subject and activity number
avg_std<-combinedata%>%select(subject,activity,contains("mean"),contains("std"))

#rename the titles in the data set to be descriptive and clean
names(avg_std)<-gsub("Acc","Accelerometer",names(avg_std))
names(avg_std)<-gsub("^t","Time",names(avg_std))
names(avg_std)<-gsub("Mag","Magnitude",names(avg_std))
names(avg_std)<-gsub("^f","Frequency",names(avg_std))
names(avg_std)<-gsub("Gyro","Gyroscope",names(avg_std))
names(avg_std)<-gsub("tBody","TimeBody",names(avg_std))
names(avg_std)<-gsub("angle","Angle",names(avg_std))
names(avg_std)<-gsub("-std()","STD",names(avg_std))
names(avg_std)<-gsub("-mean()","Mean",names(avg_std))
names(avg_std)<-gsub("BodyBody","Body",names(avg_std))
names(avg_std)<-gsub("activity","Activity",names(avg_std))
names(avg_std)<-gsub("subject","Subject",names(avg_std))
names(avg_std)<-gsub("gravity","Gravity",names(avg_std))

#recode activity numbers with description of activity
avg_std$Activity<-activities[avg_std$Activity,2]

#final dataset from avg_std written in a new text filetable
compositedata<-avg_std %>%
  group_by(Subject,Activity) %>%
  summarise_all(funs(mean))
write.table(compositedata,"compositedata.txt", row.name=FALSE)