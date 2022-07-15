Probability of two boys in a family with at least one
================
Sentient Potato •
July 14, 2022

Recently Twitter user `@RandomSprint`
[tweeted](https://twitter.com/RandomSprint/status/1547237398110691328?s=20&t=3poerHKKJbZLAsQD0vyRmA)

> Mr. Smith has two children. At least one of them is a boy. What is the
> probability both are boys?

The correct answer is 1/3, but 1/2 won the attached poll. Why is 1/3 the
correct answer? I’ll show you three ways: two simple analytical
approaches and simulation evidence.

First, assuming that every time a child is born there is a 50% chance
the child is a boy and a 50% chance the child is a girl, there are four
possible outcomes for a family with two children, all of which are
equally likely (25% chance of each outcome):

-   two boys
-   a boy born, then a girl (one boy)
-   a girl born, then a boy (one boy)
-   two girls

Since we *know* one of the children is a boy, we are now reduced to
three outcomes, each of which would be equally likely (now a 33 1/3%
chance of each outcome):

-   two boys
-   a boy born, then a girl (one boy)
-   a girl born, then a boy (one boy)

So, 1/3 is the correct answer. Some people’s confusion stems from not
being able to distinguish between the two possible outcomes that result
in the family having one boy. For me, the thing that helps is
remembering that there’s a difference between having an oldest son and
younger daughter than having an eldest daughter and younger son. Since
the question doesn’t tell us which situation we’re dealing with, we have
to treat them distinctly.

We can also consider the mathematical formula for conditional
probability, Bayes’ rule. Bayes’ rule tells us what the probability of
an event B happening, *given* that we already know event A has happened:

![\Pr(A \mid B) = {{\Pr(B \mid A) \Pr(A)} \over {\Pr(B)}}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5CPr%28A%20%5Cmid%20B%29%20%3D%20%7B%7B%5CPr%28B%20%5Cmid%20A%29%20%5CPr%28A%29%7D%20%5Cover%20%7B%5CPr%28B%29%7D%7D "\Pr(A \mid B) = {{\Pr(B \mid A) \Pr(A)} \over {\Pr(B)}}")

Here, A is the probability of two boys being born and B is the
probability of at least one being born. We know the probability of at
least one boy being born is 3/4, and the probability of two boys being
born is 1/4; the probability of at least one boy being born given that
we know two are born is 1. So plugging these numbers in,

![\Pr(\text{two boys} \mid \text{one boy}) = {{1 \times {1 \over 4}} \over {3 \over 4}} = {1/4 \over 3/4} = {1 \over 3}.](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5CPr%28%5Ctext%7Btwo%20boys%7D%20%5Cmid%20%5Ctext%7Bone%20boy%7D%29%20%3D%20%7B%7B1%20%5Ctimes%20%7B1%20%5Cover%204%7D%7D%20%5Cover%20%7B3%20%5Cover%204%7D%7D%20%3D%20%7B1%2F4%20%5Cover%203%2F4%7D%20%3D%20%7B1%20%5Cover%203%7D. "\Pr(\text{two boys} \mid \text{one boy}) = {{1 \times {1 \over 4}} \over {3 \over 4}} = {1/4 \over 3/4} = {1 \over 3}.")

In case the analytical solutions don’t convince you, let’s take a look
at some simulation evidence:

``` r
set.seed(123)
n = 100000
g = c("M", "F")
families = replicate(n = n, expr = sample(x = g, size = 2, replace = TRUE))
n_boys = apply(families, 2, function(x) sum(x == "M"))
full_table = table(n_boys)
at_least_1 = which(n_boys > 0)
n_at_least_1 = length(at_least_1)
at_least_1_table = table(n_boys[at_least_1])
percent_0_boys = round(100 * (full_table / n)["0"])
percent_1_boy  = round(100 * (full_table / n)["1"])
percent_2_boys = round(100 * (full_table / n)["2"])
percent_2_boys_given_1 = round(100 * (at_least_1_table / n_at_least_1)["2"])
```

I simulated one hundred thousand sets of two children where each child
had a 50% chance of being a boy (M) and a 50% chance of being a girl
(F). As we would expect from the analysis above, 25% of the families had
no boys, 25% had two, and 50% had one girl and one boy. Of the families
that had at least one boy, what percent had two boys? It turns out 33%.

All of these avenues of showing the correct answer were previously
presented by other twitter users:

-   The implicit Bayesian approach is given by Lucas and Roxie
    -   Lucas’ tweet is
        [here](https://twitter.com/LucasOfSunshine/status/1547370267219038208?s=20&t=3poerHKKJbZLAsQD0vyRmA)
    -   Roxie’s tweet is
        [here](https://twitter.com/Staroxvia/status/1547410231189049344?s=20&t=3poerHKKJbZLAsQD0vyRmA)
-   The explicit Bayesian approach is given by Riley
    [here](https://twitter.com/HorsePoster/status/1547554246542778368?s=20&t=3poerHKKJbZLAsQD0vyRmA)
-   Simulation evidence is provided by Erin
    [here](https://twitter.com/superinducting/status/1547424734261891073?s=20&t=3poerHKKJbZLAsQD0vyRmA)
