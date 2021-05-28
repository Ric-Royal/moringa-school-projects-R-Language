## Define the question
Which individuals are more likely to click on her ads?
  
  ## The metric for success
  Identifying the individuals that are most likely to click the ads.

##The context
The client owns an online cryptography course. She had previously collected data on ad clicks from her online course. This data is supposed to help her identify individuals that she should target the ads to.

## Experimental design taken
CRISP DM methodology will be used in dealing with the data. This includes: reading the data, data cleaning and Exploratory Data Analysis.

## The appropriateness of the available data to answer the given question.
The data given contains information on gender, location of the individual, the amount of time spent online by the individual e.t.c This will help answer the question stated above.


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
```
```

advert <- read.csv("http://bit.ly/IPAdvertisingData", sep= ",")


```

```

head(advert) # Checking the first 6 rows of the dataset

View(advert)

str(advert)

dim(advert) #The dataset has 1000 rows and 10 columns. Checking for the dimensions of the dataset

class(advert) # Checking for the class

```
## Data cleaning
```
# Converting numerical data into categorical data

advert$Male <- as.factor(advert$Male) 
advert$'Clicked.on.Ad' <- as.factor(advert$'Clicked.on.Ad')

# Checking for null values.
colSums(is.na(advert)) # There are no null values

# Checking for outliers.
boxplot(advert$'Daily.Time.Spent.on.Site') 

boxplot.stats(advert$'Daily.Time.Spent.on.Site')$out

boxplot(advert$Age) # Checking for outliers in Age column

boxplot.stats(advert$Age)$out 

```
## Univariate Analysis
```
# Getting a summary of the statistics.
summary(advert) 


# Fetching the Age column
# ---
# 
age <- advert$Age

# Applying the table() function to compute the frequency distribution of the age variable
# ---
# 
age_frequency <- table(age)

# Printing age_frequency below
# ---
#

age_frequency

# Then applying the barplot function to produce its bar graph
# ---
# 
barplot(age_frequency)



# Fetching the  column
# ---
# 
area_income <- advert$'Area.Income'

# Applying the table() function will compute the frequency distribution of the area_income variable
# ---
# 
area_income_frequency <- table(area_income)

# Printing area_income_frequency below
# ---
#

area_income_frequency

# Then applying the barplot function to produce its bar graph
# ---
# 
barplot(area_income_frequency)
```
## Bivariate anailysis

```
age <- advert$Age
area_income <- advert$'Area.Income'
daily_time <- advert$'Daily.Time.Spent.on.Site'
daily_internet <- advert$'Daily.Internet.Usage'
male <- advert$Male
clicked <- advert$'Clicked.on.Ad'

plot(age, area_income, xlab="Age", ylab="Area.Income")

plot(age, daily_time, xlab="Age", ylab="Daily.Time.Spent.on.Site")

plot(area_income, daily_time, xlab="Area.Income", ylab="Daily.Time.Spent.on.Site")

# plot Age vs. Area income 

ggplot(advert, 
       aes(x = age, 
           y = area_income )) +
  geom_point() + 
  labs(title = "Age by  Area Income")

# plot Age vs. Daily internet usage 

ggplot(advert, 
       aes(x = age, 
           y = daily_internet )) +
  geom_point() + 
  labs(title = "Age by  Daily Internet Usage")


# plot Daily time spent on site vs. Daily Internet Usage 

ggplot(advert, 
       aes(x = daily_time, 
           y = daily_internet )) +
  geom_point() + 
  labs(title = "Daily Time Spent on Site by  Daily Internet Usage")

```
## Multivariate analysis
```
# plot Daily time spent on site vs. Area income 
# (color represents clicked on ad, shape represents gender)
ggplot(advert, 
       aes(x = daily_time, 
           y = area_income, 
           color = clicked)) +
  geom_point(size = 3, 
             alpha = .6) +
  labs(title = "Daily Time Spent on Site by clicked on Ad, Gender, and Area Income")

# plot Daily time spent on site vs. Area income 
# (color represents gender, shape represents clicked on ad)
ggplot(advert, 
       aes(x = daily_time, 
           y = area_income, 
           shape = clicked,
           color = male)) +
  geom_point(size = 3, 
             alpha = .6) +
  labs(title = "Daily Time Spent on Site by clicked, Gender, and Area Income")


# plot Daily time spent on site vs. Daily Internet Usage 

ggplot(advert, 
       aes(x = daily_time, 
           y = daily_internet,
           color = male )) +
  geom_point(size = 3, 
             alpha = .6) + 
  labs(title = "Daily Time Spent on Site by Daily Internet Usage")

```


## 
Individuals that spent 30 - 60 minutes of their daily time on the online course, are more likely to have clicked on the ads. Majority of this individuals come from areas with incomes within the range f 15,000 to 70,000. Within this category, majority of the individuals were of the male gender.


## Conclusion
The analysis helped identify the target group for the ads.

## Recommendation
Target the ads to individuals within this categories in order to get more ad clicks.
1. Individuals that spend 30 - 50 minutes of their daily time on the site.
2. Individual that come from an area with incomes ranging from 20,000 to 70,000
3. Lastly, target more individual from the male gender.

```


# splitting the dataset into training and testing sets
intrain <- createDataPartition(y = advert$'Clicked.on.Ad' , p = 0.7, list= FALSE)

training <- advert[intrain,]


testing <- advert[-intrain,]

# Checking the dimension of the training and testing dataset.
dim(training); 
dim(testing);

#Training the model
trct <- trainControl(method = "repeatedcv", number = 10, repeats = 3)

svm_Linear <- train(Clicked.on.Ad ~., data = training, method = "svmLinear",
trControl=trct,
preProcess = c("center", "scale"),
tuneLength = 10)

# 
svm_Linear

```
Support Vector Machines with Linear Kernel 

700 samples
  9 predictor
  2 classes: '0', '1' 

Pre-processing: centered (2312), scaled (2312) 
Resampling: Cross-Validated (10 fold, repeated 3 times) 
Summary of sample sizes: 630, 630, 630, 630, 630, 630, ... 
Resampling results:

  Accuracy   Kappa    
  0.9242857  0.8485714

Tuning parameter 'C' was held constant at a value of 1
