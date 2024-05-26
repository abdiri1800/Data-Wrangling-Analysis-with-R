# Data-Wrangling-Analysis-with-R
## Table of Contents
- [Project Overview](project-overview)
- [Data Sources](data-sources)
- [Tools](tools)
- [Data Cleaning/Preparation](data-cleaning/preparation)
- [Exploratory Data Analysis](exploratory-data-analysis)
- [Data Analysis](data-analysis)
- [Results/Findings](result/findings)
- [Limitations](Limitations)
## Project Overview
In the rapidly evolving digital marketplace, understanding customer dynamics is crucial for businesses aiming to optimize their strategies and enhance user experiences. This project aims to delve into the intricate patterns and trends of online retail sales, examining how customer behavior changes over time. By leveraging data analytics, we aim to provide comprehensive insights into the factors driving these dynamics.
## Data Sources
We got our data from a secondary source(kaggle), which was perfect for analyzing and understanding the customer dynamics. The source is authentic and reliable for use in our scenario.
## Tools
- R
  - Major packages used:
  - tidyr - for cleaning
  - tidyverse
  - ggplot2 - for visualization
  - readr - for reading csv file
## Data Cleaning/Preparation
The dataset was loaded into the R environment, clean and transform it making it ready for analysis.
Below are some of the R codes that we used in the data cleaning and wrangling phase;
- The below line of code helped us in splitting the date and time into separate columns.
```R
dataset <- separate(dataset, InvoiceDate, into = c('InvoiceDate', 'InvoiceTime'), sep = ' ')
```
- we managed to eliminate all rows that had null values, reason behind this call was because the number of rows with null values was insignificant to the total rows in our dataset. Below is the line of codes responsible for eliminating all null values;
```R
dataset <- dataset %>%
  select_all() %>%
  na.omit()
```
A new column, totalprice, was created by multiplying the quantity and price columns. This transformation was performed to facilitate the analysis of total sales value for each transaction
```R
dataset <- mutate(dataset, TotalPrice = UnitPrice * Quantity)
```
The date column and the time column was data type char, transformation needed to be made to change it to data type date and time respectively
```R
dataset <- dataset %>%
  mutate(InvoiceDate = as.Date(InvoiceDate))
#converting the invoicetime type from char to time type
dataset <- dataset %>%
  mutate(InvoiceTime = as.ITime(InvoiceTime))
```
## Exploratory Data Analysis
EDA involved exploring sales data to answer key questions;
- Which product are top sellers?
- Which product are the most sold?
- Which country brings in the most sales?
- Sales trends over time?
## Data Analysis
Looking at the sales by counties
```R
#sales by countries
sales_by_countires <- dataset %>%
  group_by(Country)%>%
  summarise(TotalPrice = sum(TotalPrice))%>%
  top_n(10, TotalPrice)%>%
  arrange(desc(TotalPrice))
sales_by_countires
```
Top 5 most sold Product
```R
x <- dataset%>%
  group_by(Description)%>%
  summarise(count_desc = n())%>%
  top_n(5, count_desc)%>%
  arrange(desc(count_desc)) %>% 
  mutate(percentage = round(count_desc / sum(count_desc) * 100,1))
```
Sales by year
```R
dataset3 <- read.csv("sheet4Csv.csv")
#dataset3 <- read.csv(file.choose())
attach(dataset3)
ds <- dataset3%>%
  mutate(dataset3, TotalPrice = Quantity * UnitPrice)%>%
  na.omit()%>%
  distinct()
yr<- ds%>%
  group_by(year)%>%
  summarise(TotalPrice = sum(TotalPrice))
yr
```
Top product by sales
```R
p_sales <- dataset%>%
  group_by(Description)%>%
  summarise(TotalPrice = sum(TotalPrice))%>%
  top_n(5, TotalPrice)%>%
  arrange(desc(TotalPrice))
p_sales
```
## Results/findings
The analysis results are summarized as follows:
1. United Kingdom was the country with the most sales followed by Netherlands and Ireland.
   
   ![bar-chart](https://github.com/abdiri1800/Data-Wrangling-Analysis-with-R/assets/107916032/7aaad4a1-d93e-4d91-95c5-fd45c5a2d4ff)

2. White Hanging Heart T-light Holder was the most frequent sold product.
   
   ![pie-chart](https://github.com/abdiri1800/Data-Wrangling-Analysis-with-R/assets/107916032/1a9ad962-7a50-4195-af45-47bf64bb43f9)

3. Regency Cakestand 3 Tier was the product that brought in a lot of sales.
   
   ![bar2-chart](https://github.com/abdiri1800/Data-Wrangling-Analysis-with-R/assets/107916032/b9679c28-8e10-4952-8fdd-e5a13dbbeb5f)

## Limitations
We had to remove all null values in the dataset because they would affect the accuracy of our conclusions from the analysis.
