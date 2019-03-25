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
Ok try an example from the book to see if we can pattern match with your data set
```{r}
max_features <- 100
maxlen <- 20

imdb <- dataset_imdb(num_words = max_features)
c(c(x_train, y_train), c(x_test, y_test)) %<-% imdb

head(imdb$train$x)
head(imdb$train$y)
head(imdb$train)
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


#mnist <- dataset_mnist()

mnist$test$y

mnist$train$x

mnist_y=  to_categorical(mnist$test$y)
head(mnist_y)

```
Try example and mess around with outputs
Max words is deleting the words that are not frequent words are not kept
Max length is the max of review.  Everything after 20 words in a review is cut.
```{r}
max_features <- 100
maxlen <- 20

imdb <- dataset_imdb(num_words = max_features)
c(c(x_train, y_train), c(x_test, y_test)) %<-% imdb

head(imdb$train$y)

x_train <- pad_sequences(x_train, maxlen = maxlen)
x_test <- pad_sequences(x_test, maxlen = maxlen)

head(x_train)
dim(x_train)
dim(y_train)
length(y_train)

model <- keras_model_sequential() %>%
layer_embedding(input_dim = 100, output_dim = 8,
input_length = maxlen) %>%
layer_flatten() %>%
layer_dense(units = 1, activation = "sigmoid")

model


model %>% compile(
optimizer = "rmsprop",
loss = "binary_crossentropy",
metrics = c("acc")
)

summary(model)

history <- model %>% fit(
x_train, y_train,
epochs = 10,
batch_size = 32,
validation_split = 0.2
)

```
What is the difference between their data or your data???  The outcome is binary, so maybe just try that first????


Put in model parameters
```{r}
dim(all_dat)

model <- keras_model_sequential() %>%
layer_embedding(input_dim = 1000, output_dim = 8,
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

#categorical_crossentropy
model
46*8
```
Now run the model
```{r}
x_train
y_train
training_y
training_x
dim(training_x)

history <- model %>% fit(
  training_x, training_y,
  validation_data = list(testing_x, testing_y)
)
summary(history)

```



