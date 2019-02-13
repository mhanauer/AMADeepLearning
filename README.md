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
training_like = training$Like
training_like = data.frame(id = 1:length(training_like), training_like = training_like)

```
Now we putting each response into its own text file

```{r}
library(devtools)
source_url("https://gist.github.com/benmarwick/9266072/raw/csv2txts.R")
csv2txt("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses/AutoContent", labels = 1)
```
We need the control file.  
What we want is the variable name, so we need to create a bunch of variables names
You need to create a data that has the path the text file so readme can read it.
```{r}
both_control = data.frame(t(replicate(dim(training_like)[1], rnorm(1, 0, 1))))

names(both_control) <- paste0( "P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses/AutoContent",1:ncol(both_control), ".txt")

library(reshape2)
both_control = melt(both_control)
both_control = both_control$variable
both_control = as.data.frame(both_control)
colnames(both_control) = c("filename")
filename = both_control$filename
filename = as.data.frame(filename)
dim(filename)

```
Now we need to introduce the codes.  I will just make them up for the sake of this example
```{r}
truth = c(rep(1, 68/4), rep(2, 68/4), rep(3, 68/4), rep(4, 68/4))
head(truth)
trainingset = c(rep(1, length(truth)[1]), rep(0, dim(testing)[1]-length(truth)))
length(trainingset)
dim(both)
```



