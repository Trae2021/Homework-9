# Homework-9
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
av
library(tidyverse)

parse_number(av[11:20])

Deaths = av %>% select(Name.Alias, Death1, Death2, Death3, Death4, Death5, Return1, Return2, Return3, Return4, Return5)
Deaths = Deaths %>% gather(Time, Death, 2:6)
Deaths = Deaths %>% mutate(Time = parse_number(Deaths$Time))

Deaths2 = Deaths %>% gather(Time, Return, 2:6)
Deaths2 = Deaths2 %>% mutate(Time = parse_number(Deaths2$Time))
Deaths2

##A few of the website's claims are as follows:
###Around 40% (69 out of 173) of Avengers die at some point
###2/3 chance of returning from 1st death but 50% chance of returning from 2nd or 3rd death
##Let's fact-check these, shall we?

##Percent of Avengers who die at least once:
Dead = Deaths2 %>% filter(Death == "YES", Time == 1)
NumDead = Dead %>% nrow()
NumAll = Deaths2 %>% filter(Time == 1) %>% nrow()
NumDead/NumAll*100
###According to this data set, 89 out of 865 Avengers die at least once. This is around 10.3% of Avengers, which is significantly from the website's claim of 40%. This could be due to updated data, different data sets, and other things like whether they counted honorary Avengers and what is classified as a death in the data set vs this website.

##Chances of returning from the dead:
Dead1 = Deaths2 %>% filter(Death == "YES", Time == 1) 
Dead2 = Deaths2 %>% filter(Death == "YES", Time == 2) 
Dead3 = Deaths2 %>% filter(Death == "YES", Time == 3) 
Dead4 = Deaths2 %>% filter(Death == "YES", Time == 4) 
Dead5 = Deaths2 %>% filter(Death == "YES", Time == 5) 
Dead1 %>% ggplot(aes(x="", y=Return, fill=Return)) +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0)
  
Dead2 %>% ggplot(aes(x="", y=Return, fill=Return)) +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0)
  
Dead3 %>% ggplot(aes(x="", y=Return, fill=Return)) +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0)
  
Dead4 %>% ggplot(aes(x="", y=Return, fill=Return)) +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0)
  
Dead5 %>% ggplot(aes(x="", y=Return, fill=Return)) +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0)

###Looking at the pie charts I made from this data set, it looks like the probability of an Avenger returning from their first death is around 85%, even higher than the website's claim! Then, their chance of returning from a second death drops to a bit below 50%, similar to the website's claims. However, the website claims that they have similar odds of coming back from a 3rd death, whereas the chart I plotted looks to say the odds are far lower, at around %15. After this, the odds of returning seem to reach their minimum at around 10% and do not drop further.