Antibiotics
================
Allyson Hur
2020-

*Purpose*: Creating effective data visualizations is an *iterative*
process; very rarely will the first graph you make be the most
effective. The most effective thing you can do to be successful in this
iterative process is to *try multiple graphs* of the same data.

Furthermore, judging the effectiveness of a visual is completely
dependent on *the question you are trying to answer*. A visual that is
totally ineffective for one question may be perfect for answering a
different question.

In this challenge, you will practice *iterating* on data visualization,
and will anchor the *assessment* of your visuals using two different
questions.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggrepel)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Rows: 16 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): bacteria, gram
    ## dbl (3): penicillin, streptomycin, neomycin
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
|:--------------------------------|-----------:|-------------:|---------:|:---------|
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

### **q1** Prototype 5 visuals

To start, construct **5 qualitatively different visualizations of the
data** `df_antibiotics`. These **cannot** be simple variations on the
same graph; for instance, if two of your visuals could be made identical
by calling `coord_flip()`, then these are *not* qualitatively different.

For all five of the visuals, you must show information on *all 16
bacteria*. For the first two visuals, you must *show all variables*.

*Hint 1*: Try working quickly on this part; come up with a bunch of
ideas, and don’t fixate on any one idea for too long. You will have a
chance to refine later in this challenge.

*Hint 2*: The data `df_antibiotics` are in a *wide* format; it may be
helpful to `pivot_longer()` the data to make certain visuals easier to
construct.

``` r
df_antibiotics %>% head
```

    ## # A tibble: 6 × 5
    ##   bacteria              penicillin streptomycin neomycin gram    
    ##   <chr>                      <dbl>        <dbl>    <dbl> <chr>   
    ## 1 Aerobacter aerogenes     870             1       1.6   negative
    ## 2 Brucella abortus           1             2       0.02  negative
    ## 3 Bacillus anthracis         0.001         0.01    0.007 positive
    ## 4 Diplococcus pneumonia      0.005        11      10     positive
    ## 5 Escherichia coli         100             0.4     0.1   negative
    ## 6 Klebsiella pneumoniae    850             1.2     1     negative

``` r
df_pivot <- df_antibiotics %>% 
  pivot_longer(
    cols = c(penicillin, streptomycin, neomycin),
    names_to = "antibiotic"
  ) %>% 
  ungroup() %>% 
  mutate(
    bacteria = as.factor(bacteria),
    bacteria = fct_reorder(bacteria, value)
  ) %>% 
  arrange(value)


df_pivot 
```

    ## # A tibble: 48 × 4
    ##    bacteria                        gram     antibiotic   value
    ##    <fct>                           <chr>    <chr>        <dbl>
    ##  1 Bacillus anthracis              positive penicillin   0.001
    ##  2 Staphylococcus albus            positive neomycin     0.001
    ##  3 Staphylococcus aureus           positive neomycin     0.001
    ##  4 Streptococcus hemolyticus       positive penicillin   0.001
    ##  5 Diplococcus pneumonia           positive penicillin   0.005
    ##  6 Streptococcus viridans          positive penicillin   0.005
    ##  7 Bacillus anthracis              positive neomycin     0.007
    ##  8 Staphylococcus albus            positive penicillin   0.007
    ##  9 Salmonella (Eberthella) typhosa negative neomycin     0.008
    ## 10 Bacillus anthracis              positive streptomycin 0.01 
    ## # ℹ 38 more rows

#### Visual 1 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

``` r
df_pivot %>% 
  mutate(bacteria = fct_reorder(bacteria, antibiotic)) %>% 
  ggplot(aes(
    # x = bacteria,
    x = fct_reorder(bacteria, antibiotic),
    y = value
  ) ) +
  geom_point(aes(color = antibiotic, shape = gram), size = 2) +
  scale_y_log10() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.1-1.png)<!-- -->

#### Visual 2 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_pivot %>% 
  ggplot(aes(
    x = antibiotic,
    y = value,
    fill = bacteria
  )) +
  geom_col(position = "dodge") +
  facet_wrap(~gram, scales = "free_x") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  scale_y_log10() +
  geom_hline(
    yintercept = 0.1,
    linetype = "dotted",
    color = "red"
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.2-1.png)<!-- -->

#### Visual 3 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_antibiotics %>% 
  ggplot(aes(penicillin,streptomycin, shape = gram )) + 
  geom_point(mapping = aes(color = bacteria)) +
  scale_y_log10() +
  scale_x_log10()  +
  geom_hline(
    yintercept = 0.1,
    linetype = "dotted",
    color = "red"
  ) +
  geom_vline(
    xintercept = 0.1,
    linetype = "dotted",
    color = "red"
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.3-1.png)<!-- -->

#### Visual 4 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_pivot %>% 
  ggplot(aes(
    x = value,
    y = antibiotic,
    fill = gram,
  ) ) +
  geom_col() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  facet_wrap(~bacteria, scales = "free_x") +
  scale_x_log10() +
  geom_vline(
    xintercept = 0.1,
    linetype = "dotted",
    color = "red"
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.4-1.png)<!-- -->

#### Visual 5 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_antibiotics %>% 
  ggplot(aes(streptomycin, penicillin)) + 
  geom_point(mapping = aes(color = bacteria)) +
  scale_y_log10() +
  scale_x_log10() 
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.5-1.png)<!-- -->

### **q2** Assess your visuals

There are **two questions** below; use your five visuals to help answer
both Guiding Questions. Note that you must also identify which of your
five visuals were most helpful in answering the questions.

*Hint 1*: It’s possible that *none* of your visuals is effective in
answering the questions below. You may need to revise one or more of
your visuals to answer the questions below!

*Hint 2*: It’s **highly unlikely** that the same visual is the most
effective at helping answer both guiding questions. **Use this as an
opportunity to think about why this is.**

#### Guiding Question 1

> How do the three antibiotics vary in their effectiveness against
> bacteria of different genera and Gram stain?

*Observations* - What is your response to the question above? -

Neomycin has the highest effectiveness for negative strains of bacteria,
including Diplococcus pneumonia, streptococcus hemolyticus, and
streptococcus vindans.

Penicillin has the highest effectiveness when it comes to positive gram
bacteria. Streptomycin is also mostly effective for positive gram
bacteria. On the other hand, neomycin is effective for both positive and
negative gram bacteria. All three are effective against staphylococcus
aureus and bacillus anthrasis.

\- Which of your visuals above (1 through 5) is **most effective** at
helping to answer this question? - My second visual (the bar chart with
all the information) was the most helpful in answering the question.

\- Why? - I was able to easily compare the gram strains to each of the
different bacteria Since I had them separated by the type of antibiotic,
I was able to see the effectiveness of each antibiotic against all the
different bacteria.

#### Guiding Question 2

In 1974 *Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae*,
and in 1984 *Streptococcus fecalis* was renamed *Enterococcus fecalis*
\[2\].

> Why was *Diplococcus pneumoniae* was renamed *Streptococcus
> pneumoniae*?

*Observations* - What is your response to the question above? - Only
penicillin was effective against Diplococcus Pneumoniae. Since
penicillin is effective against all bacteria within the streptococcus
genus, that could explain why it was renamed.

Streptococcus fecalis was renamed since penicillin was actually not the
most effective against it. It seems actually seems like neocymin is the
most effective at around 0.1, which is probably why it was renamed.

Which of your visuals above (1 through 5) is **most effective** at
helping to answer this question? - Visuals 4 and 5 - Why? - In visual 4,
I was able to compare the values of the different antibiotics quite
easily, especially since it was separated by bacteria. Visual 5 was
helpful in answering the second part of the question since I was able to
see that the neomycin level of streptococcus fecalis was was present
(around 0.1) whereas the other bacteria in the streptococcus genus had a
penicillin level lower than 0.1.

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” *American Scientist* (2009)
[link](https://www.americanscientist.org/article/thats-funny)
