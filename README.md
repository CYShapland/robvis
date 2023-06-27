
<!-- README.md is generated from README.Rmd. Please edit that file -->

# robvis <img src="man/figures/robvis_hex_box.png" align="right" width="18%" height="18%" />

[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![CRAN
Badge.](https://www.r-pkg.org/badges/version-ago/robvis)](https://CRAN.R-project.org/package=robvis)
[![CRAN
Downloads.](https://cranlogs.r-pkg.org/badges/last-month/robvis)](https://CRAN.R-project.org/package=robvis)
<br> [![R build
status](https://github.com/mcguinlu/robvis/workflows/R-CMD-check/badge.svg)](https://github.com/mcguinlu/robvis/actions)
[![Codecov test
coverage](https://codecov.io/gh/mcguinlu/robvis/branch/master/graph/badge.svg)](https://codecov.io/gh/mcguinlu/robvis?branch=master)
<br>
[![DOI](https://img.shields.io/static/v1.svg?label=Publication&message=10.1002/jrsm.1411&color=informational)](https://doi.org/10.1002/jrsm.1411)
[![metaverse
Identifier](https://img.shields.io/static/v1.svg?label=Part%20of%20the&message=metaverse&color=informational)](https://www.github.com/rmetaverse/metaverse)

**UPDATE**: `robvis` now exists as a
[web-app](https://mcguinlu.shinyapps.io/robvis), aimed at those who are
not familiar with R or who want to explore the package’s functionality
before installing it locally.

## Description

The `robvis` package takes the summary table from risk-of-bias
assessments and produces plots formatted according to the assessment
tool used.

## Getting started

Install the development version which contains new functionality and a
range of bug fixes:

``` r
install.packages("devtools")
devtools::install_github("mcguinlu/robvis")
```

To update the package, run the `install_github("mcguinlu/robvis")`
command again.

If you wish to use the older CRAN version of the package, use the
following command:

``` r
install.packages("robvis")
```

### Load data

To load your own data from a .csv file:

``` r
mydata <- read.csv("path/to/mydata.csv", header = TRUE)
```

To help users explore `robvis`, we have included example datasets in the
package, one for each of the tool templates that currently exist within
the package. The `data_rob2` dataset ([view it
here](https://github.com/mcguinlu/robvis/blob/master/data_raw/data_rob2.csv)),
which contains example risk-of-bias assessments performed using the
RoB2.0 tool for randomized controlled trials, is used to create the
plots in subsequent sections.

### Create plots

The package contains two plotting functions:

#### 1. rob_summary()

Returns a ggplot object displaying a bar-chart of the risk of bias of
included studies across the domains of the specified tool. \[*Note: the
defaults used in this function have changed from their original
settings, so that a un-weighted barplot is now produced by default. See
the NEWS.md file for further information.*\]

``` r
summary_rob <- rob_summary(data = data_rob2, tool = "ROB2")

summary_rob
```

<div style="text-align:center">

<img src=man/figures/robplot1.png width="70%" height="70%"/>

</div>

#### 2. rob_traffic_light()

Returns a ggplot object displaying a [“traffic light
plot”](https://handbook-5-1.cochrane.org/chapter_8/figure_8_6_c_example_of_a_risk_of_bias_summary_figure.htm),
displaying the risk of bias judgment in each domain for each study.

``` r
trafficlight_rob <- rob_traffic_light(data = data_rob2,
                                      tool = "ROB2",
                                      psize = 10)

trafficlight_rob
```

<div style="text-align:center">

<img src=man/figures/robplot2.png width="70%" height="70%"/>

</div>

### Other functions

#### rob_save()

Pass the `robvis` to this function, along with a destination file, to
save your risk-of-bias plots using sensible defaults.

``` r
rob_save(trafficlight_rob, "rob_fig.png")
```

#### rob_tools()

Outputs a list of the risk of bias assessment tools for which a template
currently exists in rob_summary(). We expect this list to be updated in
the near future to include tools such as ROBIS (tool for assessing risk
of bias in systematic reviews).

``` r
rob_tools()
#> Note: the "ROB2-Cluster" template is only available for the rob_traffic_light() function.
#> [1] "ROB2"         "ROB2-Cluster" "ROBINS-I"     "ROBINS-E"     "QUADAS-2"    
#> [6] "QUIPS"        "Generic"
```

## Advanced usage

### Change the colour scheme

The `colour` argument of both plotting functions allows users to select
from two predefined colour schemes (“cochrane” or “colourblind”) or to
define their own palette by providing a vector of hex codes.

For example, to use the predefined “colourblind” palette:

``` r
rob_summary(data = data_rob2,
            tool = "ROB2",
            colour = "colourblind")
```

<div style="text-align:center">

<img src=man/figures/robplot3.png width="70%" height="70%"/>

</div>

And to define your own colour scheme:

``` r
rob_summary(
  data = data_rob2,
  tool = "ROB2",
  colour = c("#f442c8",
             "#bef441",
             "#000000",
             "#d16684")
)
```

<div style="text-align:center">

<img src=man/figures/robplot4.png width="70%" height="70%"/>

</div>

### No “Overall” judgement

By default, both functions include an “Overall” risk of bias domain. To
prevent this, remove the overall column from your dataset and set
`overall = FALSE`.

``` r
summary_rob <- rob_summary(data = data_rob2[1:6], tool = "ROB2", overall = FALSE)
summary_rob
```

<div style="text-align:center">

<img src=man/figures/robplot5.png width="70%" height="70%"/>

</div>

``` r
rob_traffic_light(data = data_rob2[1:6],
                  tool = "ROB2",
                  overall = FALSE)
```

<div style="text-align:center">

<img src=man/figures/robplot6.png width="70%" height="70%"/>

</div>

### Editing the plots

Finally, because the output (`summary_rob` and `trafficlight_rob` in the
examples above) is a ggplot2 object, it is easy to adjust the plot to
your own preferences.

For example, to add a title:

``` r
library(ggplot2)

rob_summary(data = data_rob2, tool = "ROB2") +
  ggtitle("Summary of RoB 2.0 assessments")
```

<div style="text-align:center">

<img src=man/figures/robplot7.png width="70%" height="70%"/>

</div>

## Examples of `robvis` in published papers

To date, `robvis` has been cited in more than 1500 academic articles -
these can be explored
[here](https://scholar.google.com/scholar?cites=12564214960529060925&as_sdt=2005&sciodt=0,5&hl=en).

## Code of conduct

Please note that the ‘robvis’ project is released with a [Contributor
Code of
Conduct](https://github.com/mcguinlu/robvis/blob/master/CODE_OF_CONDUCT.md).
By contributing to this project, you agree to abide by its terms.

## License

This project is licensed under the MIT License - see the
[LICENSE.md](https://github.com/mcguinlu/robvis/blob/master/LICENSE)
file for details.

## Acknowledgments

- The `rob_summary()` function was based on code forwarded by a
  colleague. I recently discovered that this code was adapted from that
  presented in the wonderful “[Doing Meta-Analysis in
  R](https://bookdown.org/MathiasHarrer/Doing_Meta_Analysis_in_R/plotting-the-summary.html)”
  guide, so I would like to acknowledge the authors here.
- [Emily Kothe](https://github.com/ekothe) for help in fixing `ggplot2`
  coding issues.
- [Eliza Grames](https://github.com/elizagrames) for creating the
  `robvis` hex sticker.
