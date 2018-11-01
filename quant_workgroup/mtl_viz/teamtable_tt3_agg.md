---
title: 'Team Data Table: Report 3'
author: "Savet Hong"
date: "`r format(Sys.Date(), '%b %d, %Y')`"
output: html_document
---

To use the code in this Rmarkdown for each Team/Model in separate Rmarkdown, please do as follow:
  
- ensure that all library packages (as listed in the library chunk) are installed
- copy chuck containing R libraries 
- copy appropriate section of code
+ datafile, setup, output for each model
+ table for base and experiment(s)
- include "agg_bc_exp1.xlsx" and/or "agg_exp2_exp3.xlsx" in the same folder, if not then change file pathway to the data source    

```{r library, include=FALSE}

library(readxl)
library(dplyr)
library(tibble)
library(tidyr)
library(ggplot2)
library(htmlTable)

```


## Aggregate Model

```{r dtfiles, include=FALSE}
# Read in files related to Experimental design
dgn_bc <- read_excel("agg_bc_exp1.xlsx", col_names = FALSE, range = "A12:B15")
dgn_e1 <- read_excel("agg_bc_exp1.xlsx", col_names = FALSE, range = "A265:B268")
dgn_e2 <- read_excel("agg_exp2_exp3.xlsx", col_names = FALSE, range = "A12:B15")
dgn_e3 <- read_excel("agg_exp2_exp3.xlsx", col_names = FALSE, range = "A265:B268")

#Read in files related to graph paramenters

```

```{r setup, include=FALSE}
rsh_ord <- c("Our Question:", "Our Hypothesis:", "Our Findings:", "Our Decisions:")
rsh_ord2 <- c("Our \nQuestion", "Our \nHypothesis", "Our \nFindings", "Our \nDecisions")
  
  
dgn <- dgn_bc %>%
  rename(`Base Case` = X__2) %>%
  inner_join(dgn_e1, by = "X__1") %>%
  rename(`Experiment 1` = X__2) %>%
  inner_join(dgn_e2, by = "X__1") %>%
  rename(`Experiment 2` = X__2) %>%
  inner_join(dgn_e3, by = "X__1") %>%
  rename(`Experiment 3` = X__2,
         `QHFD Process` = X__1) %>%
  slice(match(rsh_ord, `QHFD Process`)) %>%
  gather("Experiment", value, -`QHFD Process`) %>%
  mutate(`QHFD Process` = sub("[[:punct:]]$", "", `QHFD Process`),
         `QHFD Process` = sub("(Our )(.*)$", "\\1\\\n\\2", `QHFD Process`)) %>%
  spread(`QHFD Process`, value) %>%
  select( Experiment, rsh_ord2)
  

```

#### Team Experimental Design


```{r design, echo=FALSE}
htmlTable(dgn, header = names(dgn), 
          total = "tspanner",
          css.total = c("border-top: 1px dash grey;",
                        "border-top: 1px dash grey;"),
          tspanner = c(rep("",4)),
          n.tspanner = c(rep(1,4)),
          rnames = FALSE, css.cell = "padding-left: 1em; padding-right: 1em;", caption ="Team Research/Experiment Design Process", align = "clll")


```



```{r design_tb, echo=FALSE, include= FALSE}
library(knitr)
library(kableExtra)

kable(dgn) %>%
  kable_styling(bootstrap_options = c("striped", "hover"))

```

<!---

#### Team Graphs

-->

```{r graphs, echo=FALSE}


```