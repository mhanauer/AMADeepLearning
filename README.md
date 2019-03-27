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

Ok try using the cleaning webiste: https://shirinsplayground.netlify.com/2019/01/text_classification_keras_data_prep/

Each data point is the freq of that word in the data document

Ok so it give you the index for each word and each word get's it own index
```{r}

edu_dat$CodesMatt = ifelse(edu_dat$CodesMatt == 1, 1, ifelse(edu_dat$CodesMatt == 0, 1, 0))
describe.factor(edu_dat$CodesMatt)
edu_text = edu_dat$Education

max_words <- 1000
tokenizer <- text_tokenizer(num_words = max_words)

edu_text_token_seq = texts_to_sequences(tokenizer, edu_text)
head(edu_text_token_seq)
summary(edu_text_token_seq)
```
Just straight copy and see if you can use Keras
Padding adds zeros to fill in.  
You place the words at the end.  Then you place zeros in front to fill up space for the max length of each response
```{r}
library(gdata)
x_all = pad_sequences(edu_text_token_seq)
dim(x_all)
y_all <- matrix(edu_dat$CodesMatt)
length(y_all)
all_dat = cbindX(x_all, y_all)
head(all_dat)

```
Ok now randomly assign them
```{r}
dim(all_dat)
all_dat = as.matrix(all_dat)
head(all_dat)

all_dat_complete = na.omit(all_dat)

inTrain = createDataPartition(y = all_dat_complete[,49], p = .5, list = FALSE)
training = all_dat_complete[inTrain,]
training_x = training[,-c(48)]
training_y = training[,c(49)]

testing = all_dat_complete[-inTrain,] 
testing_x = testing[,-c(48)]
testing_y = testing[,c(49)]
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

layer_flatten = put everything into one vector (rows*columns*dims)

input_shape = number of variables: https://blog.datascienceheroes.com/how-to-create-a-sequential-model-in-keras-for-r/

layer_dense = represents the number of columns that the y_var is in

optimizer = stochastic gradiant desent and other like it.  Use activiation function to try and predict what you want (softmax, linear whatever, relu)

Relu: does not work with negative numbers need leakly Relu: https://towardsdatascience.com/activation-functions-neural-networks-1cbd9f8d91d6

Activation functions: sigmoid, relu

output_dim = is the shape of the output does not seem to have any relationship to other values

```{r}
dim(all_dat)

model <- keras_model_sequential() %>%
layer_embedding(input_dim = 1000, output_dim = 4,
input_length = 48) %>%
#Still not sure why you need the flatten here, but won't run without it.
layer_flatten() %>%
layer_dense(units = 1, activation = "relu")

model

model %>% compile(
optimizer = "rmsprop",
loss = "binary_crossentropy",
metrics = c("acc")
)
model


## LSTM
model_ltsm <- keras_model_sequential() %>%
layer_embedding(input_dim = 1000, output_dim = 4,
input_length = 48) %>%
layer_lstm(units = 4) %>%  
layer_dense(units = 1, activation = "relu")

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
mean(history$metrics$acc)
plot(history)

history$metrics

```
Now run ltsm model
```{r}
history_ltsm <- model_ltsm %>% fit(
  training_x, training_y,
  validation_data = list(testing_x, testing_y)
)
summary(history_ltsm)
mean(history_ltsm$metrics$acc)
plot(history_ltsm)
```
Evaluation the model
Get new predictions classes and probs
```{r}
evaluate_model =  evaluate(model_ltsm, testing_x, testing_y)
evaluate_model


predict_clas_model = predict_classes(model_ltsm, testing_x)
head(predict_clas_model)

predict_prob_model = predict_proba(model, testing_x)
head(predict_prob_model)

```
Now let's save the model for later use
https://cran.r-project.org/web/packages/keras/vignettes/tutorial_save_and_restore.html
```{r}
model_save_test = save_model_hdf5(model, "model_save_test.h5")

new_model = load_model_hdf5("model_save_test.h5")
summary(new_model)


predict_clas_new = predict_classes(new_model, testing_x)
head(predict_clas_new)
library(kerasR)
```
Try getting a plot of the model
```{r}
library(kerasR)
plot_model(model)
plot_model(model)
```
Try skip grams
```{r}
test = skipgrams(training_x, vocabulary_size = 100)
test
```
Try replicating the example with your model and see if you can replicate ok results
```{r}
#imdb <- dataset_imdb(num_words = 10000)
library(dplyr)
c(train_data, train_labels) %<-% imdb$train
c(test_data, test_labels) %<-% imdb$test
word_index <- dataset_imdb_word_index()
paste0("Training entries: ", length(train_data), ", labels: ", length(train_labels))

word_index_df <- data.frame(
  word = names(word_index),
  idx = unlist(word_index, use.names = FALSE),
  stringsAsFactors = FALSE
)

# The first indices are reserved  
word_index_df <- word_index_df %>% mutate(idx = idx + 3)
word_index_df <- word_index_df %>%
  add_row(word = "<PAD>", idx = 0)%>%
  add_row(word = "<START>", idx = 1)%>%
  add_row(word = "<UNK>", idx = 2)%>%
  add_row(word = "<UNUSED>", idx = 3)

word_index_df <- word_index_df %>% arrange(idx)

decode_review <- function(text){
  paste(map(text, function(number) word_index_df %>%
              filter(idx == number) %>%
              select(word) %>% 
              pull()),
        collapse = " ")
}


train_data <- pad_sequences(
  train_data,
  value = word_index_df %>% filter(word == "<PAD>") %>% select(idx) %>% pull(),
  padding = "post",
  maxlen = 256
)

test_data <- pad_sequences(
  test_data,
  value = word_index_df %>% filter(word == "<PAD>") %>% select(idx) %>% pull(),
  padding = "post",
  maxlen = 256
)

```
Now try running your model with results
```{r}
vocab_size <- 10000

model_ltsm <- keras_model_sequential() %>%
layer_embedding(input_dim = vocab_size, output_dim = 16) %>%
layer_lstm(units = 4) %>%  
layer_dense(units = 1, activation = "relu")

model_ltsm

model_ltsm %>% compile(
optimizer = "rmsprop",
loss = "binary_crossentropy",
metrics = c("acc")
)
model_ltsm


model <- keras_model_sequential()
model %>% 
  layer_embedding(input_dim = vocab_size, output_dim = 16) %>%
  layer_global_average_pooling_1d() %>%
  layer_dense(units = 16, activation = "relu") %>%
  layer_dense(units = 1, activation = "sigmoid")

model %>% summary()

model %>% compile(
  optimizer = 'adam',
  loss = 'binary_crossentropy',
  metrics = list('accuracy')
)

x_val <- train_data[1:10000, ]
partial_x_train <- train_data[10001:nrow(train_data), ]

y_val <- train_labels[1:10000]
partial_y_train <- train_labels[10001:length(train_labels)]

```
Now see if this works
```{r}
history <- model_ltsm %>% fit(
  partial_x_train,
  partial_y_train,
  epochs = 40,
  batch_size = 512,
  validation_data = list(x_val, y_val),
  verbose=1
)
```





