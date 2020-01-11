# surveydatalikert

Project description: Exploring survey data using Likert and ggplot2 R packages using climate change public opinion data.

Analysing Survey Data with R, using Yale University Climate Change and the American Mind project

To find data and codebook: https://osf.io/w36gn/

Likert and other rating scales are common tools in survey research. 

A typical Likert scale is a statement to which respondents rate their level of agreement. The statement may be positive or negative. Usually a five-point scale of agreement like the following is used:

1.Strongly disagree

2.Disagree

3.Neither agree nor disagree

4.Agree

5.Strongly agree

--


library(haven) # to import data from spss

library(tidyverse) # all sort of tools and packages

library(likert)  # specialized package that allow to draw Likert plots

library(ggplot2) # for graphs

library(ggthemes) # cool graphes themes

library(scales) # for the symbol(%) to appear instead of count in graphes


#Import datasets into R (using RStudio)

data <- read_sav("data.sav")

data_backup <- data # data backup (just in case...)

data2<-as_factor(data, levels = "labels")

#Basic Graphes (with ggplot2)

#Using qplot

qplot(data2$happening)

qplot(data2$when_harm_US)+coord_flip()

#Using ggplot2

p3 <- ggplot(data2, aes(happening)) +

  geom_bar(aes(y = (..count..)/sum(..count..)), width=0.55) +
  
  scale_y_continuous(labels=percent) +
  
  ylab("Percentage") +
  
  xlab("Do you think that climate change is happening?") +
  
coord_flip()

p3

#Stacked plot 

#Theme: Perception of harm caused by climate change

df1<-data.frame(data2$harm_US,data2$harm_personally,data2$harm_dev_countries,data2$harm_plants_animals,data2$harm_future_gen)

#generate a simple data framework including only the variables of interest

names(df1) <- c(

  data2.harm_US = "Global warming will harm the US",
  
  data2.harm_personally = "Global warming will harm me",
  
  data2.harm_dev_countries = "Global warming will harm developing countries",
  
  data2.harm_plants_animals = "Global warming will harm plants and animals",
  
  data2.harm_future_gen = "Global warming will harm future generations")

str(df1)

l1<-likert(df1)

plot(l1)

plot(l1, ordered=FALSE, group.order=names(df1)) #Specify the exact order of the y-axis

plot(l1, type='density')
  
 
#Heat maps

plot(l1, type='heat')

plot(l1, type='heat', wrap=30, text.size=3, digits = 3)


#Logistic regression

library(MASS)

m <- polr(happening ~ ideology, data=data2, Hess=TRUE)

m1<- polr(happening ~ ideology + educ_category, data=data2, Hess=TRUE)

m2 <- polr(happening ~ ideology + educ_category + employment + generation + race, data=data2, Hess=TRUE)

m3 <- polr(happening ~ ideology + educ_category + employment + age + race + income, data=data2, Hess=TRUE)

m4<-polr(reg_utilities ~ ideology, data=data2, Hess=TRUE)

Sources:

https://github.com/jbryer/likert/blob/master/demo/likert.R
