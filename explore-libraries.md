explore-libraries.R
================
Kara
2020-01-27

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!

## adding a comment
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.6/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
identical(.Library, .libPaths()[2])
```

    ## [1] FALSE

``` r
## Huh? Maybe this is an symbolic link issue?
library(fs)
identical(path_real(.Library), path_real(.libPaths()[2]))
```

    ## [1] FALSE

Installed packages

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────── tidyverse 1.2.1 ──

    ## ✓ ggplot2 3.2.1     ✓ purrr   0.3.3
    ## ✓ tibble  2.1.3     ✓ dplyr   0.8.3
    ## ✓ tidyr   1.0.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.4.0

    ## ── Conflicts ───────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 237

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                                       Priority       n
    ##   <chr>                                                         <chr>      <int>
    ## 1 /Library/Frameworks/R.framework/Versions/3.6/Resources/libra… base          14
    ## 2 /Library/Frameworks/R.framework/Versions/3.6/Resources/libra… recommend…    15
    ## 3 /Library/Frameworks/R.framework/Versions/3.6/Resources/libra… <NA>         208

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                 122 0.515 
    ## 2 yes                108 0.456 
    ## 3 <NA>                 7 0.0295

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   Built     n  prop
    ##   <chr> <int> <dbl>
    ## 1 3.6.0   201 0.848
    ## 2 3.6.1    36 0.152

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "askpass"           "assertr"           "assertthat"       
    ##   [4] "automagic"         "backports"         "base64enc"        
    ##   [7] "BH"                "bitops"            "bookdown"         
    ##  [10] "brew"              "broom"             "callr"            
    ##  [13] "caTools"           "cellranger"        "cholera"          
    ##  [16] "classInt"          "cli"               "clipr"            
    ##  [19] "clisymbols"        "colorspace"        "commonmark"       
    ##  [22] "containerit"       "covr"              "cowsay"           
    ##  [25] "crayon"            "crosstalk"         "curl"             
    ##  [28] "DBI"               "dbplyr"            "debugme"          
    ##  [31] "deldir"            "desc"              "devtools"         
    ##  [34] "diffobj"           "digest"            "dplyr"            
    ##  [37] "DT"                "e1071"             "egg"              
    ##  [40] "ellipsis"          "evaluate"          "expensecalculator"
    ##  [43] "fansi"             "fastmap"           "fontBitstreamVera"
    ##  [46] "fontLiberation"    "fontquiver"        "forcats"          
    ##  [49] "foreach"           "formatR"           "fortunes"         
    ##  [52] "freetypeharfbuzz"  "fs"                "futile.logger"    
    ##  [55] "futile.options"    "gapminder"         "gargle"           
    ##  [58] "gdtools"           "generics"          "ggbarf"           
    ##  [61] "ggplot2"           "ggtextures"        "gh"               
    ##  [64] "git2r"             "glue"              "goodshirt"        
    ##  [67] "googledrive"       "googlesheets4"     "gridExtra"        
    ##  [70] "gtable"            "h2o"               "haven"            
    ##  [73] "here"              "hexbin"            "highlight"        
    ##  [76] "highr"             "HistData"          "hms"              
    ##  [79] "holepunch"         "htmltools"         "htmlwidgets"      
    ##  [82] "httpuv"            "httr"              "hunspell"         
    ##  [85] "igraph"            "ini"               "ISOcodes"         
    ##  [88] "iterators"         "janeaustenr"       "jsonlite"         
    ##  [91] "knitr"             "labeling"          "lambda.r"         
    ##  [94] "later"             "lazyeval"          "lifecycle"        
    ##  [97] "lobstr"            "lubridate"         "magick"           
    ## [100] "magrittr"          "mapproj"           "maps"             
    ## [103] "maptools"          "markdown"          "memoise"          
    ## [106] "mime"              "modelr"            "modeltools"       
    ## [109] "munsell"           "NLP"               "nycflights13"     
    ## [112] "openssl"           "pack"              "pillar"           
    ## [115] "pkgbuild"          "pkgconfig"         "pkgload"          
    ## [118] "plogr"             "plyr"              "pracma"           
    ## [121] "praise"            "prettyunits"       "processx"         
    ## [124] "profvis"           "progress"          "promises"         
    ## [127] "ps"                "purrr"             "PythonEmbedInR"   
    ## [130] "R6"                "rappdirs"          "rcmdcheck"        
    ## [133] "RColorBrewer"      "Rcpp"              "RCurl"            
    ## [136] "readr"             "readxl"            "rematch"          
    ## [139] "rematch2"          "remotes"           "renv"             
    ## [142] "reprex"            "repurrrsive"       "requirements"     
    ## [145] "reshape2"          "rex"               "rgeos"            
    ## [148] "rjson"             "rlang"             "rmarkdown"        
    ## [151] "rmsfact"           "roxygen2"          "rprojroot"        
    ## [154] "rstudioapi"        "rversions"         "rvest"            
    ## [157] "scales"            "selectr"           "semver"           
    ## [160] "servr"             "sessioninfo"       "sf"               
    ## [163] "shiny"             "shinyBS"           "slam"             
    ## [166] "SnowballC"         "sourcetools"       "sp"               
    ## [169] "stevedore"         "stopwords"         "stringi"          
    ## [172] "stringr"           "styler"            "sys"              
    ## [175] "sysreqs"           "systemfonts"       "testthat"         
    ## [178] "textdata"          "threejs"           "tibble"           
    ## [181] "tidyr"             "tidyselect"        "tidytext"         
    ## [184] "tidyverse"         "tinytex"           "tm"               
    ## [187] "tokenizers"        "topicmodels"       "translations"     
    ## [190] "TSP"               "units"             "usethis"          
    ## [193] "utf8"              "uuid"              "vctrs"            
    ## [196] "vdiffr"            "versions"          "viridisLite"      
    ## [199] "whisker"           "withr"             "x13binary"        
    ## [202] "xaringan"          "xfun"              "xml2"             
    ## [205] "xopen"             "xtable"            "yaml"             
    ## [208] "zeallot"           "zipcode"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 FALSE     90 0.380
    ## 2 TRUE     147 0.620
