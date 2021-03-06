
# Day 2 Session 1
---

### Goals for Session 1

Session 1
* Q&A, clarification and discussions to review day 1 materials
* summary stats
* selecting variables
* selecting subset of rows
* selecting subsets of rows and columns
* create a function
* use sapply to use the new function
* look at various ways to summarize the variables
* options in Hmisc, pastecs and psych packages
* call same function name in different packages - avoiding masking issues

---

# summary stats

help(package = "datasets")
data()
mtcars
str(mtcars)

summary(mtcars[,c("mpg","cyl","disp","hp")])

mycars <- mtcars
mycars$cyl <- as.factor(mtcars$cyl)

summary(mycars[,c("mpg","cyl","disp","hp")])

mycarsNA <- mycars
mycarsNA[(mycarsNA$cyl)==6,"mpg"] <- NA

summary(mycarsNA[,c("mpg","cyl","disp","hp")])

fivenum(mycars$mpg)
quantile(mycars$mpg, c(.25,.75))
fivenum(mycarsNA$mpg)

mystats <- function(x, na.omit=FALSE){
  if(na.omit)
    x <- x[!is.na(x)]
  m <- mean(x)
  n <- length(x)
  s <- sd(x)
  md <- mad(x)
  q5 <- quantile(x, .05)
  q25 <- quantile(x, .25)
  q75 <- quantile(x, .75)
  q95 <- quantile(x, .95)
  return(c(n=n, mean=m, stdev=s, MAD=md, 
           q5, q25, 
           q75, q95))
}

mystats(mycars$mpg)
mystats(mycarsNA$mpg, na.omit=TRUE)

# let's apply these to a range of variables

myvars <- c("mpg","cyl","disp","hp")
sapply(mycars[myvars], fivenum)
sapply(mtcars[myvars], fivenum)

out <- sapply(mtcars[myvars], fivenum)
kable(out)

sapply(mtcars[myvars], mystats)
out <- sapply(mtcars[myvars], mystats)
kable(out)

# select only rows of cars that have 6 cylinders
d1 <- mtcars[mtcars$cyl==6,myvars]
sapply(d1, mystats)

# select cars that have 4 cylinders (cyl==4) and 
# are automatic transmission (am==0)
d2 <- mtcars[(mtcars$cyl==4 & mtcars$am==0),myvars]
sapply(d2, mystats)

# can also use the subset() function
d3 <- subset(mtcars, cyl==4 & am==0, select=myvars)
d3

# Hmisc package
library(Hmisc)
describe(mtcars[myvars])
describe(mycarsNA[myvars])

# pastecs package
library(pastecs)
stat.desc(mtcars[myvars])

# psych package
library(psych)
describe(mtcars[myvars])

# avoid masking problems using 2 colons ::
Hmisc::describe(mtcars[myvars])
psych::describe(mtcars[myvars])


