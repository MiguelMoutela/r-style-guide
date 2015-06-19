# R Styleguide

This is the coding style guide we use at FreeAgent for R.
This guide is based on [Google's R Style Guide](https://google-styleguide.googlecode.com/svn/trunk/Rguide.xml).

### Some thoughts before you start

Use common sense and BE CONSISTENT.

The point of having style guidelines is to have a common vocabulary of coding so
people can concentrate on *what* you are saying, rather than on *how* you are
saying it. We present global style rules here so people know the vocabulary. But
local style is also important. If code you add to a file looks drastically different
from the existing code around it, the discontinuity will throw readers out of
their rhythm when they go to read it. Try to avoid this.

## Language

### Attach

The possibilities for creating errors when using `attach` are numerous. Avoid it.

### Functions

Errors should be raised using `stop()`.

### Objects and Methods

The S language has two object systems, S3 and S4, both of which are available in
R. S3 methods are more interactive and flexible, whereas S4 methods are more
formal and rigorous. (For an illustration of the two systems, see Thomas Lumley's
"Programmer's Niche: A Simple Class, in S3 and S4" in R News 4/1, 2004, pgs. 33 - 36:
http://cran.r-project.org/doc/Rnews/Rnews_2004-1.pdf.)

Use S3 objects and methods unless there is a strong reason to use S4 objects or
methods. A primary justification for an S4 object would be to use objects directly
in C++ code. A primary justification for an S4 generic/method would be to dispatch
on two arguments.

Avoid mixing S3 and S4: S4 methods ignore S3 inheritance and vice-versa.

## Notation and Naming
### File Names
File names should be meaningful and have the extension .R.

```R
#good
predict_ad_revenue.R

#bad
foo.R
```

### Identifiers
Don't use underscores (`_ `) or hyphens (` -`) in identifiers.
Variable names should be all lower case. and words separated withdots
(`variable.name`). Function names should have an initial capital letter and no dots
(`FunctionName`); constants are named like functions but with an initial k.

```R
# good
variable.name
avg.clicks

# bad
avgClicks
avg_Clicks
```

```R
# good
FunctionName
CalculateAvgClicks

# bad
calculate_avg_clicks
calculateAvgClicks
```

Function names should be imperative eg. `MakePlots`, `ComputeStatistics`  
*Exception: When creating a classed object, the function name (constructor)
and class should match (e.g., lm).*

```R
# good
kConstantName
```

## Syntax

### Line Length
Prefer short lines to longer ones and keep to than 115 characters if possible.
This is the width of github's diff view without wrapping.

### Indentation
When indenting your code, use two spaces. Never use tabs or mix tabs and spaces.

### Spacing
- Place spaces around all binary operators (=, +, -, <-, etc.).  
*Exception: Spaces around ='s are optional when passing parameters in a function call.*
- Do not place a space before a comma, but always place one after a comma.

```R
# good
tab.prior <- table(df[df$days.from.opt < 0, "campaign.id"])
total <- sum(x[, 1])
total <- sum(x[1, ])

# bad
tab.prior <- table(df[df$days.from.opt<0, "campaign.id"])  # Needs spaces around '<'
tab.prior <- table(df[df$days.from.opt < 0,"campaign.id"])  # Needs a space after the comma
tab.prior<- table(df[df$days.from.opt < 0, "campaign.id"])  # Needs a space before <-
tab.prior<-table(df[df$days.from.opt < 0, "campaign.id"])  # Needs spaces around <-
total <- sum(x[,1])  # Needs a space after the comma
total <- sum(x[ ,1])  # Needs a space after the comma, not before
```
- Place a space before left parenthesis, except in a function call.

```R
# good
if (debug)

# bad
if(debug)
```

- Do not place spaces around code in parentheses or square brackets.  
*Exception: Always place a space after a comma.*

```R
# good
if (debug)
x[1, ]

# bad
if ( debug )  # No spaces around debug
x[1,]  # Needs a space after the comma
```

### Curly Braces

- An opening curly brace should never go on its own line; a closing curly brace
should always go on its own line. You may omit curly braces when a block consists
of a single statement; however, you must consistently either use or not use
curly braces for single statement blocks.

```R
if (is.null(ylim)) {
  ylim <- c(0, 0.06)
}
xor (but not both)
```
```R
if (is.null(ylim))
  ylim <- c(0, 0.06)
 ```

- Always begin the body of a block on a new line.

```R
# bad

if (is.null(ylim)) ylim <- c(0, 0.06)
if (is.null(ylim)) {ylim <- c(0, 0.06)}
```

### Surround else with braces

An else statement should always be surrounded on the same line by curly braces.

```R
# good
if (condition) {
  one or more lines
} else {
  one or more lines
}

# bad
if (condition) {
  one or more lines
}
else {
  one or more lines
}

if (condition)
  one line
else
  one line
```

### Assignment

Use `<-`, not `=`, for assignment.

```R
# good
x <- 5

# bad
x = 5
```

### Semicolons

Do not terminate your lines with semicolons or use semicolons to put more than
one command on the same line.

## Organization
### General Layout and Ordering

1. Copyright statement comment
2. Author comment
3. File description comment, including purpose of program, inputs, and outputs
4. `source()` and `library()` statements
5. Function definitions
6. Executed statements, if applicable (e.g., `print`, `plot`)
7. Unit tests should go in a separate file named `originalfilename_test.R`.

### Commenting Guidelines

- Comment your codewhere the line itself is not totally obvious or additional
explanation is needed. Entire commented lines should begin with # and one space.

- Short comments can be placed after code preceded by two spaces, #, and then
one space.

```R
# Create histogram of frequency of campaigns by pct budget spent.
hist(df$pct.spent,
     breaks = "scott",  # method for choosing number of buckets
     main = "Histogram: fraction budget spent by campaignid",
     xlab = "Fraction of budget spent",
     ylab = "Frequency (count of campaignids)")
```
### Function Definitions and Calls

- Function definitions should first list arguments without default values, followed
by those with default values.
- In both function definitions and function calls, multiple arguments per line are
allowed; line breaks are only allowed between assignments.

```R
# good
PredictCTR <- function(query, property, num.days,
                       show.plot = TRUE)

# bad
PredictCTR <- function(query, property, num.days, show.plot =
                       TRUE)
```
- Ideally, unit tests should serve as sample function calls (for shared library routines).

### Function Documentation

Functions should contain a comments section immediately below the function definition
line. These comments should consist of a one-sentence description of the function;
a list of the function's arguments, denoted by `Args:`, with a description of each
(including the data type); and a description of the return value, denoted by
`Returns:`. The comments should be descriptive enough that a caller can use the
function without reading any of the function's code.

```R
CalculateSampleCovariance <- function(x, y, verbose = TRUE) {
  # Computes the sample covariance between two vectors.
  #
  # Args:
  #   x: One of two vectors whose sample covariance is to be calculated.
  #   y: The other vector. x and y must have the same length, greater than one,
  #      with no missing values.
  #   verbose: If TRUE, prints sample covariance; if not, not. Default is TRUE.
  #
  # Returns:
  #   The sample covariance between x and y.
  n <- length(x)
  # Error handling
  if (n <= 1 || n != length(y)) {
    stop("Arguments x and y have different lengths: ",
         length(x), " and ", length(y), ".")
  }
  if (TRUE %in% is.na(x) || TRUE %in% is.na(y)) {
    stop(" Arguments x and y must not have missing values.")
  }
  covariance <- var(x, y)
  if (verbose)
    cat("Covariance = ", round(covariance, 4), ".\n", sep = "")
  return(covariance)
}
```

