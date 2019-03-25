---
---
title: "BAHCS-10 Prelim Results"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Packages
GPU could get you more computing power
```{r}
library(tidyverse)
library(keras)
library(caret)
```

Get data ready for keras later on
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
Scramble the responses and then send for potential coding
```{r}
### 
set.seed(123)
both_like = sample(both$Like, size = 329, replace = FALSE)
head(both_like)
write.csv(both_like, "both_like.csv")

```


Ok try using the cleaning webiste: https://shirinsplayground.netlify.com/2019/01/text_classification_keras_data_prep/

Each data point is the freq of that word in the data document

Ok so it give you the index for each word and each word get's it own index
```{r}
both_like = data.frame(id = both$id, Like = both$Like)
dim(both_like)
both_like$truth = c(rep(1,dim(both_like)[1]/7), rep(1,dim(both_like)[1]/7), rep(1,dim(both_like)[1]/7), rep(1,dim(both_like)[1]/7), rep(0,dim(both_like)[1]/7), rep(0,dim(both_like)[1]/7), rep(0,dim(both_like)[1]/7))

both_like_text = as.character(both_like$Like)



max_words <- 10000
tokenizer <- text_tokenizer(num_words = max_features)
both_like_token = fit_text_tokenizer(tokenizer,both_like_text)
head(both_like_text)

head(both_like_token$index_word)
head(both_like_token$num_words)
head(both_like_token$word_index)


both_like_seq = texts_to_sequences(tokenizer, both_like_text)
head(both_like_seq)
summary(both_like_seq)
```
Just straight copy and see if you can use Keras
Padding adds zeros to fill in.  
You place the words at the end.  Then you place zeros in front to fill up space for the max length of each response
```{r}
library(gdata)
x_all = pad_sequences(both_like_seq)
dim(x_all)
y_all <- matrix(both_like$truth)
length(y_train)
all_dat = cbindX(x_all, y_all)
head(all_dat)

```
Ok now randomly assign them
```{r}
dim(all_dat)
all_dat = as.matrix(all_dat)
head(all_dat)

inTrain = createDataPartition(y = all_dat[,37], p = .5, list = FALSE)
training = all_dat[inTrain,]
training_x = training[,-c(37)]
training_y = training[,c(37)]

testing = all_dat[-inTrain,] 
testing_x = testing[,-c(37)]
testing_y = testing[,c(37)]
```

What is the difference between their data or your data???  The outcome is binary, so maybe just try that first????

Ok so you need to have the number of dims to be larger than the largest number assignment to a word.  Remember words are assigned random numbers.  The output dim doesn't matter you will have to guess at this.  The input_length must be the number of columns in the testing data set so everything besides the predicted values.


Put in model parameters

What do I need to learn next?
Output_dim and how that affect accuracy
How to compare models
layer_dense
CNN and different kinds of neural networks
Save the model

CNN = image classification so deal with this later
```{r}
dim(all_dat)

model <- keras_model_sequential() %>%
layer_embedding(input_dim = 1000, output_dim = 4,
input_length = 36) %>%
layer_flatten() %>%
layer_dense(units = 1, activation = "sigmoid")

model
20*8

model %>% compile(
optimizer = "rmsprop",
loss = "binary_crossentropy",
metrics = c("acc")
)
model


## LSTM
model_ltsm <- keras_model_sequential() %>%
layer_embedding(input_dim = 1000, output_dim = 4,
input_length = 36) %>%
layer_lstm(units = 4) %>%  
layer_dense(units = 1, activation = "sigmoid")

model_ltsm

model_ltsm %>% compile(
optimizer = "rmsprop",
loss = "binary_crossentropy",
metrics = c("acc")
)
model_ltsm

```
Now run the models
```{r}
history <- model %>% fit(
  training_x, training_y,
  validation_data = list(testing_x, testing_y)
)
summary(history)
history$metrics
plot(history)
```
Now run ltsm model
```{r}
history_ltsm <- model_ltsm %>% fit(
  training_x, training_y,
  validation_data = list(testing_x, testing_y)
)
summary(history_ltsm)
history_ltsm$metrics
plot(history_ltsm)
```



