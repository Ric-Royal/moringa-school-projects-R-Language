```R
# Loading the necessary libraries
#
library(arules)

```

```
super <- read.transactions("http://bit.ly/SupermarketDatasetII")
super

head(super)

```

```
# Identifying the class.
#
class(super)


# Previewing the first 5 transactions
#
inspect(super[1:5])


# Getting a summary of the dataset
#
summary(super)

# Exploring the frequency of some articles 
#
itemFrequency(super[, 8:10],type = "absolute")
round(itemFrequency(super[, 8:10],type = "relative")*100,2)

#
#
par(mfrow = c(1, 2))

# plot the frequency of items
itemFrequencyPlot(super, topN = 10,col="darkgreen")
itemFrequencyPlot(super, support = 0.1,col="darkred")

# Building a model based on association rules 
rules <- apriori (super, parameter = list(supp = 0.001, conf = 0.8))
rules

rules1 <- apriori (super, parameter = list(supp = 0.002, conf = 0.8))
rules1

rules2 <- apriori (super, parameter = list(supp = 0.001, conf = 0.6))
rules2

rules3 <- apriori (super, parameter = list(supp = 0.002, conf = 0.6))
rules3

# Exploring the models
#
summary(rules)

summary(rules1)

summary(rules2)

summary(rules3)

# Observing the rules of the model
inspect(rules3[1:5])

inspect(rules2[1:5])

inspect(rules[1:5])

inspect(rules1[1:5])


# Ordering the rules by confidence criteria
rules<-sort(rules1, by="confidence", decreasing=TRUE)
inspect(rules[1:5])

# To create a promotional message for indiviudals who like tea, create a subset of rules concerning tea 
tea <- subset(rules, subset = rhs %pin% "tea")
 
# Then order by confidence
tea<-sort(tea, by="confidence", decreasing=TRUE)
inspect(tea[1:5])

