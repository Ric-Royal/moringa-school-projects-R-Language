## Define the question
Which are the characteristics of different customer groups?
  
## The metric for success
Identifying the different categories of customers.

##The context
Kristina Plastinina is a Russian brand that is sold through a defunct chain of retail stores in Russia, Ukraine, Kazakhstan, Belarus, China, Philippines, and Armenia. The brand's Sales and Marketing team would like to understand their customer's behavior from data that they have collected over the past year. More specifically, they would like to learn the characteristics of customer groups.

## Experimental design taken
CRISP DM methodology will be used in dealing with the data. This includes: reading the data, data cleaning and Exploratory Data Analysis.


## Installing packages

```
install.packages("tidyverse") # install packages to work with data frame - extends into visualization
library(tidyverse)

install.packages('data.table')
library("data.table")

install.packages('caret')
library("caret")

install.packages('kernlab')
library("kernlab")

install.packages('e1071')
library("e1071")

install.packages("dbscan")
library("dbscan")

```
```

ecomm <- read.csv("http://bit.ly/EcommerceCustomersDataset", sep= ",")


```

```

head(ecomm) # Checking the first 6 rows of the dataset

view(ecomm)

str(ecomm)

dim(ecomm) #The dataset has 1000 rows and 10 columns. Checking for the dimensions of the dataset

class(ecomm) # Checking for the class

```
## Data cleaning
```
# Converting numerical and character data into categorical data

ecomm$Month <- as.factor(ecomm$Month)
ecomm$OperatingSystems <- as.factor(ecomm$OperatingSystems)
ecomm$Browser <- as.factor(ecomm$Browser)
ecomm$Region <- as.factor(ecomm$Region)
ecomm$TrafficType <- as.factor(ecomm$TrafficType)
ecomm$VisitorType <- as.factor(ecomm$VisitorType)
ecomm$Weekend <- as.factor(ecomm$Weekend)
ecomm$Revenue <- as.factor(ecomm$Revenue)


# Checking for null values.
colSums(is.na(ecomm)) # There are atleast 14 rows and 8 columns with null values

# Removing Null values from the dataset.
ecomm <- na.omit(ecomm)
colSums(is.na(ecomm)) # The rows with null values have been omitted.

# Checking for outliers.
boxplot(ecomm$'Administrative') # Has outliers

boxplot.stats(ecomm$'Administrative')$out # listing the outliers

# Checking for outliers.
boxplot(ecomm$'Administrative_Duration') # Has outliers 

boxplot.stats(ecomm$'Administrative_Duration')$out # listing the outliers

# Checking for outliers.
boxplot(ecomm$'Informational') # Has Outliers

boxplot.stats(ecomm$'Informational_Duration')$out # listing the outliers

# Checking for outliers.
boxplot(ecomm$'ProductRelated') 

boxplot.stats(ecomm$'ProductRelated_Duration')$out # listing the outliers

# Checking for outliers.
boxplot(ecomm$'BounceRates') 

boxplot.stats(ecomm$'BounceRates')$out # listing the outliers

# Checking for outliers.
boxplot(ecomm$'ExitRates') 

boxplot.stats(ecomm$'PageValues')$out # listing the outliers

# Checking for outliers.
boxplot(ecomm$'SpecialDay') 

boxplot.stats(ecomm$'SpecialDay')$out # listing the outliers

# Eliminating the outliers
#
outliers <- boxplot(ecomm, plot=FALSE)$out
ecomm <- ecomm[-which(ecomm %in% outliers),]

dim(ecomm)

```
## Univariate Analysis
```
# Getting a summary of the statistics.
summary(ecomm) 


# Fetching the Administrative column
# ---
# 
admin <- ecomm$Administrative

# Applying the table() function to compute the frequency distribution of the age variable
# ---
# 
admin_frequency <- table(admin)

# Printing age_frequency below
# ---
#

admin_frequency

# Then applying the barplot function to produce its bar graph
# ---
# 
barplot(admin_frequency)



# Fetching the  column
# ---
# 
admin_dur <- ecomm$'Administrative_Duration'

# Applying the table() function will compute the frequency distribution of the area_income variable
# ---
# 
admin_dur_frequency <- table(admin_dur)

# Printing area_income_frequency below
# ---
#

admin_dur_frequency

# Then applying the barplot function to produce its bar graph
# ---
# 
barplot(admin_dur_frequency)
```
## Bivariate anailysis

```
admin <- ecomm$Administrative
admin_dur <- ecomm$'Administrative_Duration'
info <- ecomm$'Informational'
info_dur <- ecomm$'Informational_Duration'
prod_rel <- ecomm$ProductRelated
prod_rel_dur <- ecomm$'ProductRelated_Duration'
bounce_rt <- ecomm$'BounceRates'
exit_rt <- ecomm$'ExitRates'
pg_val <- ecomm$'PageValues'
spec_day <- ecomm$'SpecialDay'



# plot Administrative vs. Administrative Duration 

ggplot(ecomm, 
       aes(x = admin, 
           y = admin_dur )) +
  geom_point() + 
  labs(title = "Administrative vs. Administrative Duration")

# plot Information vs. Information Duration 

ggplot(ecomm, 
       aes(x = info, 
           y = info_dur )) +
  geom_point() + 
  labs(title = "Information vs. Information Duration ")


# plot Product Realated vs. Product Related Duration 

ggplot(ecomm, 
       aes(x = prod_rel, 
           y = prod_rel_dur )) +
  geom_point() + 
  labs(title = "Product Related vs. Product Related Duration")

```
## Multivariate analysis
```
# plot Product Related vs. Product Related Duration 
# (color represents Revenue, shape represents gender)
ggplot(ecomm, 
       aes(x = prod_rel, 
           y = prod_rel_dur, 
           color = Revenue)) +
  geom_point(size = 3, 
             alpha = .6) +
  labs(title = "Product Related vs. Product Related Duration")

# plot Administrative vs. Informational 
# 
ggplot(ecomm, 
       aes(x = admin, 
           y = info, 
           color = VisitorType)) +
  geom_point(size = 3, 
             alpha = .6) +
  labs(title = "Administrative vs. Informational")


# plot Daily time spent on site vs. Daily Internet Usage 

ggplot(advert, 
       aes(x = daily_time, 
           y = daily_internet,
           color = male )) +
  geom_point(size = 3, 
             alpha = .6) + 
  labs(title = "Daily Time Spent on Site by Daily Internet Usage")

```

## DBSCAN

```
# Creating a dataset without the class label.

ecommerce = ecomm[,c(6,7,8,9,10,14,15,17)]
dim(ecommerce)

# Applying the DBSCAN algorithm
eco <- dbscan(ecommerce,eps=0.1,MinPts = 3)

# Printing the clustering results
# 
print(eco)

# Plotting the cluster
#
hullplot(ecommerce,eco$cluster)

```
## Hierarchical Clustering

```

# Scaling the data
ecomm <- scale(ecomm[,c(6,7,8,9,10,14,15,17)])
dim(ecomm)

# Computing the distance fucntion
e <- dist(ecomm, method = "euclidean")

# Hierarchical clustering using ward's method
res.hc <- hclust(e, method = "ward.D2" )

# Plotting the dendrogram
plot(res.hc, cex = 0.6, hang = -1)

