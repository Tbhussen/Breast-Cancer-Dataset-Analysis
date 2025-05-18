---
title: "Breast Cancer Dataset - EDA"
author: "Tamim Hussein"
date: "`r Sys.Date()`"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

```{r}
library(dplyr)
library(ggplot2)
library(corrplot)
```

```{r}
df <- read.csv("C:\\Users\\ji160\\OneDrive\\Desktop\\My Project\\breast-cancer.csv")
head(df)
```

```{r}
str(df)
```

```{r}
ggplot(df, aes(diagnosis, radius_mean)) +
  geom_boxplot() +
  geom_jitter(color = "Blue") +
  theme_minimal()
ggplot(df, aes(diagnosis, texture_mean)) +
  geom_boxplot() +
  theme_minimal()
ggplot(df, aes(diagnosis, perimeter_mean)) +
  geom_boxplot() +
  theme_minimal()
ggplot(df, aes(diagnosis, area_mean)) +
  geom_boxplot() +
  theme_minimal()
ggplot(df, aes(diagnosis, smoothness_mean)) +
  geom_boxplot() +
  theme_minimal()
ggplot(df, aes(diagnosis, compactness_mean)) +
  geom_boxplot() +
  theme_minimal()
```

```{r}
ggplot(df, aes(texture_mean, perimeter_mean)) +
  geom_line() +
  theme_minimal()
ggplot(df, aes(perimeter_mean, area_mean)) +
  geom_line() +
  theme_minimal()
ggplot(df, aes(smoothness_mean, concavity_mean)) +
  geom_line() +
  theme_minimal()
ggplot(df, aes(symmetry_mean, fractal_dimension_mean)) +
  geom_line() +
  theme_minimal()
```

```{r}
ggplot(df, aes(texture_mean, perimeter_mean, color = diagnosis)) +
  geom_point() +
  theme_minimal()
ggplot(df, aes(area_mean, perimeter_mean, color = diagnosis)) +
  geom_point() +
  theme_minimal()
ggplot(df, aes(texture_mean, perimeter_mean, color = diagnosis)) +
  geom_point() +
  theme_minimal()
ggplot(df, aes(symmetry_mean, fractal_dimension_mean, color = diagnosis)) +
  geom_point() +
  theme_minimal()
ggplot(df, aes(radius_mean, perimeter_mean)) +
  geom_point() +
  theme_minimal()
```

```{r}
df$diagnosis <- ifelse(df$diagnosis == "M", 1, 0)

matrix <- df %>%
  select_if(is.numeric) %>%
  cor()
corrplot(matrix, method = "circle")
```

```{r}
# Choose highly correlated features excluding identical
high_cor <- which(abs(matrix) > 0.8 & abs(matrix) < 1, arr.ind = TRUE)

# Get feature names
high_cor_pairs <- apply(high_cor, 1, function(i) paste(rownames(matrix)[i[1]], colnames(matrix)[i[2]], sep = " - "))

# Show results
unique(high_cor_pairs)
```

Machine Learning Model:

```{r}

corr_target <- abs(matrix[,"diagnosis"])

features_selected <- names(corr_target[corr_target > 0.6])

names <- features_selected[features_selected != "diagnosis"]
```

```{r}
X = df[names]
Y = df[["diagnosis"]]
```

```{r}
# Load libraries
library(caret)
library(randomForest)

set.seed(123)  # for reproducibility

# Split into train (80%) and test (20%)
train_index <- createDataPartition(Y, p = 0.8, list = FALSE)

X_train <- X[train_index, ]
X_test  <- X[-train_index, ]

Y_train <- Y[train_index]
Y_test  <- Y[-train_index]
```

```{r}
# Prepare training and testing datasets
train_data <- data.frame(X_train, diagnosis = Y_train)
test_data  <- data.frame(X_test, diagnosis = Y_test)

train_data$diagnosis <- as.factor(train_data$diagnosis)
test_data$diagnosis  <- as.factor(test_data$diagnosis)

# Train the Random Forest model
set.seed(123)
rf_model <- randomForest(diagnosis ~ ., data = train_data, importance = TRUE)

# Make predictions on the test set
pred <- predict(rf_model, newdata = test_data)

# Align factor levels for evaluation
actual <- test_data$diagnosis

# Evaluate model performance
conf_matrix <- confusionMatrix(pred, actual)
print(conf_matrix)

```

```{r}
# Get importance
importance_vals <- importance(rf_model)

# Plot
varImpPlot(rf_model, main = "Feature Importance (Random Forest)")
```
