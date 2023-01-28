# Intermediate Statistics CA RStudio Notes

> **Welcome to MA 331 Spring 2023!**

> These notes are meant to help you succeed in this course. These will include necessary tips for _R_ formulas you will need throughout this course. Feel free to give feedback on these via [email](akassymo@stevens.edu): akassymo@stevens.edu,  [email](aleather@stevens.edu): aleather@stevens.edu.

> **These notes are not meant to replace the slides.** For more info, please refer to the slides.

**I highly suggest installing _R Markdown_!** It will make your life so much easier. [Here's a great guide how to install _R Markdown_.](https://alexd106.github.io/intro2R/install_rmarkdown.html)

## Lecture 1

R formulas are **very** similar to Python. If you ever used _pandas_ or _scipy_, you will easily get into _RStudio_.

create a variable: `x = ...`

> **Tip**: if you want to see what is stored in your variable, just type its name.

create a vector: `vector1 = c(x1, ..., xn)`

length of a vector: `length(v1)`

create a table / vector of vectors: `table1 = cbind(v1, ..., vn)`

> **Tip**: if you want to see what is stored in your vector, just type its name.

five-number summary: `summary(x)`

mean of a sample: `mean(x)`

median of a sample: `median(x)`

standard deviation of a sample: `sd(x)`

variance of a sample: `var(x)`

correlation coefficient between two samples: `cor(x,y)`

> **Tip**: a good practice would be to store these statistics in a separate variables. Let's say you test effectiveness of some product and store the data in a vector `data`. Use one-liners like `dataMean = mean(x)` or `dataSD = sd(x)` in the beginning of each Markdown file so you could always access those.

scatterplot: `plot(x)`

boxplot, histogram, pie-chart: `boxplot(x), hist(x), pie(x)`

### Normal Distribution

cdf of a normal distribution:
`pnorm(x, mean, sd)`

quantile of a normal distribution: `qnorm(q, mean, sd)`

normal QQ plot: `qqnorm(x)`

detrended QQ plot: `qqline(x)`

> **Tip**: you can always see the history of your console in the upper right corner, so if you ever need to remember a command you used last time you opened _RStudio_, you can always see it there.

## Lecture 2

In _R Markdown_ you can easily format math formulas! To do that, you just have to use \$ signs from both sides of an equation. There are a couple more cool things you could do inside the math equation:

1. Use an underscore \_{\<index\>} to add an index to your equation: `$H_{0}$` would give you $H_{0}$.
2. Use the tag \\overline{\<equation\>} to add a bar to your equation: `$\overline{X}$` would give you $\overline{X}$. **Don't forget the figure parentheses**.
3. You can directly type the greek letters in the formula using a backslash: `$\sigma$` would give you $\sigma$. If you need the capitalized letter $\Sigma$, just change it to `$\Sigma$`.
4. To do fractions, type \\frac{numerator}{denominanor}, so `$\frac{1}{n}$` would give you $\frac{1}{n}$.

> **Tip**: Use copy-paste. At first, this might seem tedious, but it's really easy when you get the grasp of it.

A very cool thing _R_ can do is to generate a random sample of trials.

To generate a random sample on a **normal** distribution:
`rnorm(sample size, mean, sd)`

### Binomial Distribution

cdf of a binomial distribution: `pbinom(x, number of trials, probability of a successful trial)`

quantile of a binomial distribution: `qbinom(q, number of trials, probability of a successful trial)`

pmf of a binomial distribution: `dbinom(x, number of trials, probability of a successful trial)`

> Be aware that pmf is not pdf! pmf stands for probability mass function.

To generate a random sample on a binomial distribution:
`rbinom(number of observations, number of trials, probability of a successful trial)`

> **Tip**: At this point, you probably have noticed that both normal and binomial distributions have similar _R_ function calls. For example, cdf always starts with a `p..` and quantile always starts with `q..`. This pattern will repeat in the future!

### Chi-square distribution

Chi-square distribution is a sum of squares of random variables. With chi-square distribution, you are introduced to a new parameter - degrees of freedom $df$.

In the case of chi-square distribution, $df = \mu$.

cdf of a chi-square distribution:
`pchisq(x, df)`

quantile of a chi-square distribution: `qchisq(q, df)`

### Student $\tau$-distribution

T-distribution is a ratio between a normal and a chi-square distribution.

In the case of t-distribution, $df = n - 1$, $\mu = 0$ and $var = \frac{n}{n-2}$.

cdf of a t-distribution:
`pt(x, df)`

quantile of a t-distribution: `qt(q, df)`

### F-distribution

F-distribution is a ration between to chi-square distributions. Because of that, it has two $df$ parameters.

In the case of F-distribution, $\mu = \frac{df_2}{df_2 - 2}$.

cdf of an F-distribution:
`pf(x, df1, df2)`

quantile of an F-distribution: `qf(q, df1, df2)`

> At this point, you might be really overwhelmed with the amount of theory. **I understand how you might feel**, but later all of these will start making sense. Each one of these distributions has a use, and you will be introduced to them shortly.

## Lecture 3

Confidence levels **are usually 95%**, if not stated otherwise.

### Confidence interval with a known $\sigma$
###
To find a confidence interval with a **known** standard deviation (z-interval), there is a very easy procedure:

1. Let's say, you're given a vector `data`, standard deviation `sd` and confidence level `cf`.
2. Find the sample size of your dataset: `n = length(data)`.
3. Given confidence level `cf`, your quantile alpha is `alpha = 1 - cf`.
4. Then your z-interval is in between bounds 1 and 2, where: `bound1, bound2 = mean(data) ± qnorm(alpha / 2) * sd / sqrt(n)`.
5. `zinterval = c(bound1, bound2)`.

### Confidence interval with an unknown $\sigma$
###

If you don't known standard deviation of your population, you can use sample variance as an **estimate** of your population variance. This is the main trick to get t-interval:

1. You're given a dataset `data` and confidence level `cf`.
2. Find the sample size of your dataset: `n = length(data)`.
3. Given confidence level `cf`, your quantile alpha is `alpha = 1 - cf`.
4. Find the sample standard deviation and sample mean: `sampleSD = sd(data)`.
5. Then your t-interval is in between bounds 1 and 2, where: `bound1, bound2 = mean(data) ± qt(alpha / 2, n - 1) * sampleSD / sqrt(n)`.
6. `tinterval = c(bound1, bound2)`.

> **Tip**: you can reassign a new value to a variable like `bound1`, _R Markdown_ follows the order of commands in which they're typed in.

### Confidence interval irrespective of $\mu$
###
In the case, where we don't have both population mean and variance, we can find chi-interval. We are using sample variance as an **estimate** for our population variance.

1. You're given a dataset `data` and confidence level `cf`.
2. Find the sample size of your dataset: `n = length(data)`.
3. Given confidence level `cf`, your quantile alpha is `alpha = 1 - cf`.
4. Find the sample variance: `samplevar = var(data)`.
5. Then your chi-interval is in between bounds 1 and 2, where: `bound1 = (n - 1) * samplevar / qchisq(1 - alpha / 2, n - 1)` & `bound2 = (n - 1) * samplevar / qchisq(alpha / 2, n - 1)`.
6. `chinterval = c(bound1, bound2)`.

### Population proportion interval
###
Given a dataset `data` we could convert it to binary with: `bdata = ifelse(data > ref, 1, 0)`, where `ref` is a reference point. Example: given a dataset `grades = [98, 56, 76, 47, 86]`:

```
passorfail = ifelse(grades > 70, 1, 0)
passorfail
```

```
[1] 1, 0, 1, 0, 1
```

To create a population proportion interval, given the binary dataset`bdata`:

1. Find the sample size of your dataset: `n = length(bdata)`.
2. Given confidence level `cf`, your quantile alpha is `alpha = 1 - cf`.
3. Find the sample proportion: `phat = mean(data)`.
4. Then your prop-interval is in between bounds 1 and 2, where: `bound1, bound2 = phat ± qnorm(alpha / 2) * sqrt(phat * (1 - phat)) / sqrt(n)`.
5. `propinterval = c(bound1, bound2)`.
