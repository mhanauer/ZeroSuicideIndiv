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

#indiv_dat = read.csv("AO Log  consolidated 2002-2019.csv", header = TRUE, na.strings = c("unknown", "?", "", " ", "???","```", "??","10-Apr"))
dat_suicide = data.frame(death_date =  indiv_dat$Date.of.Incident, location = indiv_dat$Centerstone.Location, age = indiv_dat$Age.at.time.of.Event, gender = indiv_dat$Gender, suicide = indiv_dat$Type.of.Event, path_enroll_event = indiv_dat$Enrolled.in.Pathway...time.of.Incident,  prim_diag = indiv_dat$Primary.Diagnosis, hospital_prior_admin =indiv_dat$Number.of.Prior.Hospital.Admissions, number_of_services = indiv_dat$X..of.Services.CRISIS..280..287)
head(dat_suicide)
```
Subset set data suicide and get rid of NAs in date must have death date
```{r}
dat_suicide = subset(dat_suicide, suicide == "SDC")
dim(dat_suicide)
dat_suicide$death_date = as.character(dat_suicide$death_date)
dat_suicide = subset(dat_suicide, death_date != "NA")
dat_suicide$death_date
dim(dat_suicide)
sum(is.na(dat_suicide$death_date))

dat_suicide$death_date = ifelse(dat_suicide$death_date == "Apr-06", "01/04/2006", ifelse(dat_suicide$death_date == "June-12", "01/06/2012", dat_suicide$death_date))
dat_suicide$death_date = mdy(dat_suicide$death_date)
```
Review the rest of the variables for errors
Location just grab the first word 
```{r}
dat_suicide$location = gsub(' [A-z ]*', '' , dat_suicide$location)
dat_suicide$location = gsub("\\W", "", dat_suicide$location) 
describe.factor(dat_suicide$location)
```
Age
```{r}
dat_suicide$age
dat_suicide$age = as.character(dat_suicide$age)
dat_suicide$age = gsub(' [A-z ]*', '' , dat_suicide$age)
dat_suicide$age
dat_suicide$age = substr(dat_suicide$age, 1,2) 
dat_suicide$age = as.integer(dat_suicide$age)
dat_suicide$age
```
Next variable
Path Enroll Event = The suicide took place while the client was enrolled in the pathway.
```{r}
dat_suicide$gender = toupper(dat_suicide$gender)
#Path enrollment
describe.factor(dat_suicide$path_enroll_event)
dat_suicide$path_enroll_event = toupper(dat_suicide$path_enroll_event)
write.csv(dat_suicide, "dat_suicide.csv", row.names = TRUE)
dat_suicide = read.csv("dat_suicide.csv", header= TRUE, na.strings = c("NA"))
describe.factor(dat_suicide$path_enroll_event)
x = " Some text "
dat_suicide$path_enroll_event = trimws(dat_suicide$path_enroll_event)
describe.factor(dat_suicide$path_enroll_event)
```
Next variable
```{r}
dat_suicide$X = NULL
dat_suicide$prim_diag
```







We get the crude rate which is the count divided by the population, then multiple that by the percentage of the population that is that age range.

Do this for the NHCS data that you have that is broken down by age range.

For suicide data set, then you need to get the average per year for each age range (try this out)
```{r}
CSrate = rpois(15, 10)/10000
CSrate

```

