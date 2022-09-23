_"By the way, could you quickly..."_

This was a comment I dreaded during my PhD. Every time my professor uttered it, I knew 'quickly' meant a week of work. Over time I learned to organize my work in such a way that running a new analysis or updating an existing one did actually become quickly. This example is a collection of my experiences and lessons learn. Steal it, share it, improve it. Hopefully it will save you  some of the blood, sweat and tears that I shed duing my research.

# Example R project
This repo is intended as an example of an analysis pipeline in R. The main aim is to show how you could build up your analysis in a modular way, separating main steps for easier maintenance and updating you analysis. 

## Prerequisites
1. R (www.r-project.org)
2. RStudio (https://www.rstudio.com/)
3. Git (https://git-scm.com/)

## How to use (fork and clone)
1. Fork this repository to your own account. 
2. Rename the repository to the name of your project.
3. Open RStudio and create a New Project (Version Control)
4. Copy and paste the repository url. It should look like https://github.com/my-github-account/my-project
   RStudio will add a .gitignore and Rproj file for you.

## Design philosophy
These are general ideas and opinions, not laws. At times there are good reasons to make an exception. Feel free to do so.

* Raw data are immutable. They do not require version control.  
* Plots and output are a products of scripts. If the script is under version control, the products of the script need not be.  
* Don't install and load libraries separately in each script. Instead control them at the project level with the _requirements.txt_ file and the `R/env.R` script.
* Complexity of the project should match its stage. The current version is the result of several iterations. It is okay to start with a single script and cut it into smaller scripts over time.  

## A living project in practice
My aim is to have only a single line to run an entire analysis. I can run from the console:

> source("R/run.R")

to run the entire analysis. Here, `run.R` is a wrapper script around several modules. Some of the modules such as `validate_model.R` even call upon other helper scripts (`func.R`). These have not been there from the start but were created over time while refactoring code.  

In general I start with a small task. If that task becomes repetitive (_i.e._ I start copy-pasting it), it gets time to write a function. I usually place that function in a separate file. That makes for easier package build at a later stage (if I want).  

If the series of tasks starts to increase and the file becomes very git, start splitting it up into modules and have a wrapper script like `run.R`.  

## Recommendations
1. Adapt the .gitignore file:
* Add /data  
Data are immutable. The raw data should never be changed, use scripts to clean instead. 
Data should not live on GitHub.
* Add /plots line  
Plots take up a lot if space, GitHub is not meant to version control plots. 

2. Consider using renv for isolation and reproducibility. It comes as an option in your Rproject settings. It can be found in the Rproject options.

3. **DO NO STORE SECRETS IN YOUR VERSION CONTROL** Either use:  
a. An .Renviron file and `Sys.getenv()` as [described by Rstudio](https://support.rstudio.com/hc/en-us/articles/360047157094-Managing-R-with-Rprofile-Renviron-Rprofile-site-Renviron-site-rsession-conf-and-repos-conf).
b. Use A Key Vault as [described here](https://cran.r-project.org/web/packages/AzureKeyVault/vignettes/intro.html)