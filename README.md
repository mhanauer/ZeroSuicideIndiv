---
title: "ZeroSuicideAgeAdjustStand"
output:
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Load in the individualized data
Only ones without completely missing data are
Date.of.Incident
Date.Staff.aware.of.Event
```{r}
setwd("~/Desktop")
indiv_dat = read.csv("IndividualZeroSuicideData.csv", header = TRUE, na.strings = c("unknown", "?", "", " ", "???","```", "??","10-Apr"))
head(indiv_dat)
colMeans(is.na(indiv_dat))
```
Get the data by month and year again???
Just get a few variables
One person has "10-Apr" need to make them NA

```{r}
library(lubridate)
indiv_dat_sub = data.frame(location = indiv_dat$Centerstone.Location, DOB= indiv_dat$Date.of.Birth, gender = indiv_dat$Gender, DOI = indiv_dat$Date.of.Incident, suicide = indiv_dat$Type.of.Event)
indiv_dat_sub_complete = na.omit(indiv_dat_sub)
dim(indiv_dat_sub_complete)

head(indiv_dat_sub_complete$DOI)
levels(indiv_dat_sub_complete$DOI)
levels(indiv_dat_sub_complete$DOB)
indiv_dat_sub_complete$DOI = gsub("\\s", "", indiv_dat_sub_complete$DOI)
levels(as.factor(indiv_dat_sub_complete$DOI))
indiv_dat_sub_complete$DOI= gsub("-1/2/05", "", indiv_dat_sub_complete$DOI)
levels(as.factor(indiv_dat_sub_complete$DOI))
indiv_dat_sub_complete$DOI= gsub("&1/13/03", "", indiv_dat_sub_complete$DOI)
levels(as.factor(indiv_dat_sub_complete$DOI))
indiv_dat_sub_complete$DOI= gsub("//", "", indiv_dat_sub_complete$DOI)
levels(as.factor(indiv_dat_sub_complete$DOI))

indiv_dat_sub_complete$DOI= gsub("1/12/2003", "1/12/03", indiv_dat_sub_complete$DOI)
levels(as.factor(indiv_dat_sub_complete$DOI))
indiv_dat_sub_complete$DOI = mdy(indiv_dat_sub_complete$DOI)

```




We get the crude rate which is the count divided by the population, then multiple that by the percentage of the population that is that age range.

Do this for the NHCS data that you have that is broken down by age range.

For suicide data set, then you need to get the average per year for each age range (try this out)
```{r}
CSrate = rpois(15, 10)/10000
CSrate

```

