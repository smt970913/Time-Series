Time Series HW1
================

## Problem 1.3

  - Generate n = 100 observations from the autoregression
      - \(x_t = -.9x_{t-2} + w_t\)
  - with \(\sigma_w = 1\).

<!-- end list -->

``` r
library(astsa)
library(ggplot2)
library(plotly)
```

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

``` r
#reproducibility
set.seed(2333)

# Generate Gaussiam WN with sd = 1
# Generate extra 50 to avoid startup problems
w = rnorm(150, 0, 1)
```

  - Next, apply the moving average filter
    
      - \(v_t = (x_t + x_{t-1} + x_{t-2} + x_{t-3})/4\)

  - to \(x_t\), the data we generated above.

  - Plot \(x_t\) as a line.

  - Superimpose \(v_t\) as a dashed line.

<!-- end list -->

``` r
x = stats::filter(w, filter = c(0, -0.9), method = "recursive")[-(1:50)] # remove first 50
plot.ts(x, main = "autoregression")
v = stats::filter(x, filter = rep(1/4, 4), sides = 1)
lines(v, lty = 2, col = "red")
```

![](Time-Series-HW1_files/figure-gfm/unnamed-chunk-2-1.png)<!-- --> -
The behavior of \(x_t\) shows high volatility. - Applying the moving
average filter makes it become much more smoother.

  - Repeat above process, but
      - \(x_t = \cos(2 \pi t/4)\)

<!-- end list -->

``` r
x_b = cos(2*pi*(1:150)/4)
x_b = x_b[-(1:50)]
v_b = stats::filter(x_b, filter = rep(1/4, 4), sides = 1)
plot.ts(x_b)
lines(v_b, lty = 2, col = "red")
```

![](Time-Series-HW1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

  - Repeat the above process, but with add \(N(0, 1)\) noise,
      - \(x_t = \cos(2\pi t/4) + w_t\)

<!-- end list -->

``` r
x_c = cos(2*pi*(1:150)/4) + w
x_c = x_c[-(1:50)]
v_c = stats::filter(x_c, filter = rep(1/4, 4), sides = 1)
plot.ts(x_c)
lines(v_c, lty = 2, col = "red")
```

![](Time-Series-HW1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

  - For the first plot, the moving average makes it become much more
    smoother; for the second plot, the moving average makes it become a
    straight line; for the last plot, the moving average makes the
    slower oscillations become more apparent and takes out some of the
    faster oscillations.
