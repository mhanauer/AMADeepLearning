---
---
title: "BAHCS-10 Prelim Results"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Packages
```{r}
library(tidyverse)
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
both_like$truth = c(rep(1,dim(both_like)[1]/7), rep(2,dim(both_like)[1]/7), rep(3,dim(both_like)[1]/7), rep(4,dim(both_like)[1]/7), rep(5,dim(both_like)[1]/7), rep(6,dim(both_like)[1]/7), rep(7,dim(both_like)[1]/7))

both_like_text = as.character(both_like$Like)

library(keras)

max_features <- 10000
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
x_train <- both_like_seq %>%
  pad_sequences()
dim(x_train)

x_train_df = data.frame(x_train)
x_train_df
write.csv(x_train_df, "x_train_df.csv", row.names = FALSE)
y_train <- both_like_seq
length(y_train)


```
Put in model parameters
```{r}
maxlen <- 100
batch_size <- 32
embedding_dims <- 50
filters <- 64
kernel_size <- 3
hidden_dims <- 50
epochs <- 5
```


Now try the model
```{r}
model <- keras_model_sequential() %>% 
  layer_embedding(max_features, embedding_dims, input_length = maxlen) %>%
  layer_dropout(0.2) %>%
  layer_conv_1d(
    filters, kernel_size, 
    padding = "valid", activation = "relu", strides = 1
  ) %>%
  layer_global_max_pooling_1d() %>%
  layer_dense(hidden_dims) %>%
  layer_dropout(0.2) %>%
  layer_activation("relu") %>%
  layer_dense(1) %>%
  layer_activation("sigmoid") %>% compile(
  loss = "binary_crossentropy",
  optimizer = "adam",
  metrics = "accuracy"
  )
```
Now this
```{r}
hist <- model %>%
  fit(
    x_train,
    y_train,
    batch_size = batch_size,
    epochs = epochs,
    validation_split = 0.3
    )
```
Ok let us try this with AMA data that has some actual codes that make sense
```{r}
head(edu_dat)
```
