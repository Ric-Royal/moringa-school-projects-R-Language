```R
# Load tidyverse and anomalize
# ---
# 
library(tidyverse)
library(anomalize)

install.packages('tibbletime')
library(tibbletime)
```

```
data2 <- read.csv('http://bit.ly/CarreFourSalesDataset')

head(data2)
str(data2)

data2$Date <- as.Date(data2$Date, "%m/%d/%Y")

head(data2)

data2_date <- as_tbl_time(data2, index = Date)

str(data2_date)

head(data2_date)

# Detecting the anomalies
data2_date %>%
    time_decompose(count) %>%
    anomalize(remainder, method= "iqr") %>%
    time_recompose() %>%
    plot_anomalies(time_recomposed = TRUE, ncol = 3, alpha_dots = 0.5)

    
    