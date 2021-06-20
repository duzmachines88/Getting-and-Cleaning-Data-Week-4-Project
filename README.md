# Getting-and-Cleaning-Data-Week-4-Project

#This file contains the data and cleans and presents the data in a cleaned format.

# the data was pulled from cloundfront.net.storing sensor data from smartphones.  The following code will pull the data and store it in a file in your current working directory

##if file exists command
if(!file.exists("./peerreview")){dir.create("./peerreview")}

##fileurl 
fileurl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

##downloadfile from URL and store in folder peerreview
download.file(fileurl,destfile="./peerreview/peerreview.zip", method = "curl")

##unzip folder command
unzip(zipfile="./peerreview/peerreview.zip", exdir="./peerreview")


##List all files in the UCI HAR DATASET
list.files("./peerreview/UCI HAR Dataset", recursive=TRUE)

#the code in run_analysis.R will execute code to pull the test and training data together into one clean file and will rename the columns into a more readable format

#the output of the file will be a text file stored as composite data


#files

codebook.md describes the variables and data manipulation performed on the data


run.analysis.R 
will execute code to pull the test and training data together into one clean file stored as a .txt file
  1. Define Path locations
  2. Store data as variables
  3. Rename columns with descriptive titles 
  4. Merge testing and training data into 1 table
  5. Pull out only columns containing mean or standard deviation
  6. Redefine column names into a more readable format
  7. Substitute activity number with descriptive name
  8. Export data into a final composite file named composite.txt
  

compositedata.txt is the output file for the clean data set
