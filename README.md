install.packages("tidyverse")
install.packages("nycflights13")
install.packages("dplyr")

library(dplyr)
library(nycflights13)
library(ggplot2)


##5.2.4
  flights
  # 1.Find all flights that:
# 1. Had an arrival delay of two or more hours
  filter(flights, arr_delay>=120)
  # 2. Flew to Houston (IAH or HOU)
  filter(flights, dest=="IAH" | dest=="HOU")
  filter(flights, dest %in% c("IAH", "HOU"))
  # 3. Were operated by United, American, or Delta
  filter(flights, carrier %in% c("UA","AA","DL"))
  # 4. Departed in summer (July, August, and September)
  filter(flights, month %in% c(7,8,9))
  # 5. Arrived more than two hours late, but didn’t leave late
  filter(flights, arr_delay>120, dep_delay<=0)
  # 6. Were delayed by at least an hour, but made up over 30 minutes in flight
  filter(flights, dep_delay>=60, dep_delay-arr_delay>30)
  # 7. Departed between midnight and 6am (inclusive)
  filter(flights, dep_time<=600 | dep_time ==2400)
# 2. Another useful dplyr filtering helper is between(). What does it do? Can you use it to simplify the code needed to answer the previous challenges?
  #it shortens comparisons to see if something falls between two arguements... yes 
  filter(flights, between(month, 7, 9))
# 3. How many flights have a missing dep_time? What other variables are missing? What might these rows represent?
  summary(flights) ##check for NAs...
  #missing dep_time - 8255, sched_dep_time-2359, dep_delay-8255, arr_time-8713, sched_arr_time-2359, arr_delay-9430
  ##flights that were scheduled ot depart/arrive and didn't
# 4. Why is NA ^ 0 not missing? Why is NA | TRUE not missing? Why is FALSE & NA not missing? Can you figure out the general rule? (NA * 0 is a tricky counterexample!)
  ## Anything to the zeroth power is 1.  
  ## With 'NA | TRUE', since the '|' part of the string returns 'TRUE' if either of the terms are true, the whole expression returns true. 
  ## '&' returns true when both are true, so when FALSE & NA are written in a string, the expression returns that same result because one of the terms is false.

##5.3.1
  # 1. How could you use arrange() to sort all missing values to the start? (Hint: use is.na()).
  arrange(flights, desc(is.na(dep_time)), dep_time)
  # 2. Sort flights to find the most delayed flights. Find the flights that left earliest.
  arrange(flights, desc(dep_delay)) ##earliest flight is B6 97
  arrange(flights, dep_delay)
  # 3. Sort flights to find the fastest flights.
  arrange(flights, air_time)
  # 4. Which flights travelled the longest? Which travelled the shortest?
  arrange(flights, distance)

##5.4.1
  # 2.What happens if you include the name of a variable multiple times in a select() call?
  select(flights,dep_delay, arr_time, dep_delay), ##nothing happens, you just get the variable once as if it was only selected once
  # 3.What does the one_of() function do? Why might it be helpful in conjunction with this vector?
  vars <- c("year", "month", "day", "dep_delay", "arr_delay"),
  # returns the variables you ask for... in the example below it's selecting all of the column names that match the 
  select(flights,one_of(vars)),
  # 4.Does the result of running the following code surprise you? How do the select helpers deal with case by default? How can you change that default?
  select(flights, contains("TIME")),
  ## the default is insenstive to case, you can change that by ising "ignore.case
  select(flights, contains("TIME",ignore.case = FALSE))