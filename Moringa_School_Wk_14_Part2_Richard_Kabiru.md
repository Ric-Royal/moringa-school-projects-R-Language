```R
  
# Installing and loading our caret package
# ---
# 
suppressWarnings(
        suppressMessages(if
                         (!require(caret, quietly=TRUE))
                install.packages("caret")))
library(caret)

```


```R
# Installing and loading the corrplot package for plotting
# ---
# 
suppressWarnings(
        suppressMessages(if
                         (!require(corrplot, quietly=TRUE))
                install.packages("corrplot")))
library(corrplot)

```


```

# path <- http://bit.ly/CarreFourDataset

carre<- read.csv('http://bit.ly/CarreFourDataset')  
carr <- carre[,c(6,7,8,12,13)]
```

```R
# Previewing the dataset
# 
head(carr)
view(carr)
dim(carr)

str(carr)

```


```


#Checking for duplicates

duplicated(carr)

car <- carr[!duplicated(carr),]
head(car)

setDT(carr)[,list(Count=.N),names(carr)]


```

```R
# Calculating the correlation matrix
# ---
#
correlationMatrix <- cor(carr)

# Find attributes that are highly correlated
# ---
#
highlyCorrelated <- findCorrelation(correlationMatrix, cutoff=0.75)

# Highly correlated attributes
# ---
# 
highlyCorrelated

names(carr[,highlyCorrelated])

```


```R
# We can remove the variables with a higher correlation 
# and comparing the results graphically as shown below
# ---
# 
# Removing Redundant Features 
# ---
# 
carr2<-carr[-highlyCorrelated]

# Performing our graphical comparison
# ---
# 
par(mfrow = c(1, 2))
corrplot(correlationMatrix, order = "hclust")
corrplot(cor(Dataset2), order = "hclust")

