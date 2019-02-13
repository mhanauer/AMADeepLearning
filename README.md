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
write.csv(both, "both.csv", row.names = FALSE)
both$id = 1:dim(both)[1]
```
Ok now take a random sample of 15%
```{r}
set.seed(12345)
inTrain = createDataPartition(y = both$id, p = .20, list = FALSE)
training = both[inTrain,]
testing = both[-inTrain,]
head(testing)
dim(training)
dim(testing)
```



