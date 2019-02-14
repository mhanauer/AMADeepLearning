---
---
title: "BAHCS-10 Prelim Results"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Library packages
```{r}
library(lavaan)
library(psych)
library(semTools)
library(dplyr)
library(ltm)
library(prettyR)
library(semTools)
library(GPArotation)
library(lavaan)
library(psych)
library(semTools)
library(dplyr)
library(ltm)
library(lordif)
library(Amelia)
library(plyr)
library(paran)
library(caret)
library(prettyR)
library(lme4)
#library(lmerTest)
library(MuMIn)
library(HLMdiag)
library(MASS)
library(descr)
library(brms)
library(future)
library(caret)

install_github("iqss-research/VA-package")
#install_github("iqss-research/ReadMeV1")
library(ReadMe)

```
Get the data and get rid of missing values

Need to grab demographics for each of them first

Don't get rid of NAs, because those might have valuable information
```{r}
head(adult)
adult = data.frame(Like = adult$X5..Like.Best, Help = adult$X6..How.Help, Improve = adult$X7..Like.Least.Improve)

sum(is.na(adult))
dim(adult)

youth = data.frame(Like = youth$X5..Like.Best, Help = youth$X6..How.Help, Improve = youth$X7..Life.Least)

both = rbind(adult, youth)
dim(both)
both$id = 1:dim(both)[1]
```
Ok now take a random sample of 15%
Need to give the filenames for all the data
```{r}
set.seed(12345)
inTrain = createDataPartition(y = both$id, p = .20, list = FALSE)
training = both[inTrain,]
testing = both[-inTrain,]
head(testing)
dim(training)
dim(testing)
both_like = both$Like
both_like = data.frame(id = 1:length(both_like), both_like = both_like)
write.csv(both_like, "both_like.csv", row.names = FALSE)
```
Now we putting each response into its own text file

```{r}
library(devtools)
source_url("https://gist.github.com/benmarwick/9266072/raw/csv2txts.R")
csv2txt("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses/AutoContent", labels = 1)
```
############### 
Like Analysis
###############
We need the control file.  
What we want is the variable name, so we need to create a bunch of variables names
You need to create a data that has the path the text file so readme can read it.
```{r}
like_filename = data.frame(t(replicate(length(both_like$id), rnorm(1, 0, 1))))

names(like_filename) <- paste0( "P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses/AutoContent",1:ncol(like_filename), ".txt")

library(reshape2)
like_filename = melt(like_filename)
like_filename = like_filename$variable
like_filename = as.data.frame(like_filename)
colnames(like_filename) = c("filename")
filename = like_filename$filename
filename = as.data.frame(filename)
dim(filename)

```
Now we need to introduce the codes.  I will just make them up for the sake of this example.

Now we have random codes for the training set.  We need to rbind these codes the the testing data set, to create a data with training what needs to be established.

So for all the data points in the training that do not have a code, I will put "", so the package knows that is needs to provide a value for this one.
```{r}
trainingset_truth = c(rep(1, dim(training)[1]/4), rep(2, dim(training)[1]/4), rep(3, dim(training)[1]/4), rep(4, dim(training)[1]/4))
trainingset_truth = data.frame(trainingset = trainingset_truth)

trainingset_estimate = rep("", dim(filename)[1]-length(trainingset_truth))
trainingset_estimate = data.frame(trainingset = trainingset_estimate)

trainingset = rbind(trainingset_truth, trainingset_estimate)

### Truth is the indicator telling the package what is the true code and what is not (needs to be estimated)
truth = c(rep(1, length(trainingset_truth)[1]), rep(0, dim(filename)[1]-length(trainingset_truth)))

### Now put together control file
control = cbind(filename, trainingset, truth)

write.table(control, "control.txt", row.names = FALSE, sep = ",")

```



