# TargetAutocontent

---
title: "Target Autocontent"
output:
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Library packages
```{r}
library(psych)
library(prettyR)

```
Load data
We only want to get rid of data that was blank.  If they stated something was N/A, then we want to know that, because I think we can make the assumption that there is nothing related to that category.  For example, N/A for improve, means that they are not aware of anyways to improve.
```{r}
setwd("P:/Evaluation/TN Lives Count_Writing/4_Target1_EnhancedCrisisFollow-up/3_Data & Data Analyses")
TargetAdults = read.csv("Target1EnhancedPostAdult.csv", header = TRUE, na.strings = c(""))
TargetYouth = read.csv("TargetYouth_clean.csv", header = TRUE, na.strings = c(""))
```
Just grab the variables that you want
Adult = 1, Youth = 0
```{r}
TargetAdult_Auto = data.frame(Like_Best = TargetAdults$X5..Like.Best, How_Help = TargetAdults$X6..How.Help, Improve = TargetAdults$X7..Like.Least.Improve, Indicator = rep(1,dim(TargetAdults)[1]))

dim(TargetYouth)
names(TargetYouth)

TargetYouth_auto = data.frame(Like_Best = TargetYouth$X5..Like.Best, How_Help = TargetYouth$X6..How.Help, Improve = TargetYouth$X7..Life.Least, Indicator = rep(0,dim(TargetYouth)[1]))

TargetAdultYouth_Auto = rbind(TargetAdult_Auto, TargetYouth_auto)
dim(TargetAdultYouth_Auto)

TargetAdultYouth_Auto = na.omit(TargetAdultYouth_Auto)
dim(TargetAdultYouth_Auto)

write.csv(TargetAdultYouth_Auto, "TargetAdultYouth_Auto.csv", row.names = FALSE)

```
So we want to sample about 20% of the data, but need to have an indicator of that sample
Could do a stratified random sample stratified on youth or adult makeing sure we get 10% of youth and 10% of adults

This looks good.
```{r}
library(caret)
set.seed(12345)
inTrain = createDataPartition(y = TargetAdultYouth_Auto$Indicator, p = .20, list = FALSE)
training = TargetAdultYouth_Auto[inTrain,]
testing = TargetAdultYouth_Auto[-inTrain,]
head(testing)
dim(training)
dim(testing)

mean(testing$Indicator)
mean(training$Indicator)

write.csv(training, "training.csv", row.names = FALSE)

```






