
PCA

```
# Installing Rtnse package
# 
install.packages("Rtsne")

# Loading our tnse library
# 
library(Rtsne)

library('tidyverse')

library("data.table")

# Loading the dataset
#
carre<- read.csv('http://bit.ly/CarreFourDataset')  

```

```R
# Previewing the dataset
# 
head(carre)
view(carre)
dim(carre)

str(carre)

```


```
# Converting numerical data into categorical data

carre$Branch <- as.factor(carre$Branch)
carre$Customer.type <- as.factor(carre$Customer.type)
carre$Gender <- as.factor(carre$Gender)
carre$Product.line <- as.factor(carre$Product.line)

carr <- carre[,c(6,7,8,12,15)]
carr

#
#
carre.pca <- prcomp(carre[,c(6,7,8,12,15)], center = TRUE, scale. = TRUE)
summary(carre.pca)

```
The result is 5 principal components

```
# Calling str() to have a look at the PCA object
# 
str(carre.pca)

# Plotting the PCA
# Installing the libraries

install.packages('devtools')
library(devtools)

install_github("vqv/ggbiplot")
library(ggbiplot)

ggbiplot(carre.pca)

# Adding more details to the plot
#
ggbiplot(carre.pca, labels=carre[,c(3)], obs.scale = 1, groups=carre[,c(4)], var.scale = 1)

