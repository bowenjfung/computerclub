# Installing My R toolchain 

Dan Myles 2022-08-19

I started the following with a clean install (macOS Monterey).
  
  1. Install homebrew: https://brew.sh/
    - Follow any instructions to edit your path given at the end of the output.
    - Brew should automatically install Xcode command line tools, but if not you may need to install them first `xcode-select --install` 
  1. `brew install --cask xquartz`
  2. I installed r using this homebrew tap: https://github.com/sethrfore/homebrew-r-srf. Apparently the brew core version of R doesn't ship with all the features of the CRAN version. This tap should result in a fully armed and operational version of R. Follow the instructions at that link including the install of Cairo and TclTkX11.
	  - I opted for the full install below (this may change)
	  - `brew install -s sethrfore/r-srf/r --with-cairo-x11 --with-icu4c --with-libtiff --with-openblas --with-openjdk --with-tcl-tk-x11 --with-texinfo` 
  4. Install rstudio:
	  - `brew install --cask rstudio`
  5. Install Rstan
	 - Don't worry about the macOS tool chain installer (it doesn't work)
	 - Run the R code at this link: https://github.com/stan-dev/rstan/wiki/Configuring-C---Toolchain-for-Mac
	 - Then follow these instructions: https://github.com/stan-dev/rstan/wiki/RStan-Getting-Started and check that the example model runs without error.
	 - Note: You may want to work with the development version, especially if interchanging between `cmdstanr` and `rstan`
		 - R; `install.packages("rstan", repos = c("https://mc-stan.org/r-packages/", getOption("repos")))`
6. Open RStudio and confirm again that everything loads and this example model runs without error.
7. Install cmdstanR: https://mc-stan.org/cmdstanr/articles/cmdstanr.html
8. Install rethinking: 
	- R: `install.packages("devtools")`
		- If install fails check your error messages for missing dependencies, `brew install` these and repeat
		- I had to `brew install` the following:
			- Terminal: `brew install openssl` 
			- Terminal: `brew install harfbuzz`
			- Terminal: `brew install libgit2`
			- Terminal: `brew install fribidi`
	- R: `install.packages(c("coda","mvtnorm","loo","dagitty","shape"))`
	- R: `devtools::install_github("rmcelreath/rethinking")`
9. Follow data.table installation instructions. https://github.com/Rdatatable/data.table/wiki/Installation. I used `brew install libomp` to install openMP. This includes creating (or editing) a Makevars file (do this prior to installing data.table). In our case the R code used prior to installing Stan has already created this file, so we only need to edit it: 
	  - Terminal: `open ~/.R/Makevars` 
	  - Then include the edits listed on the data.table website link above under the openmp section.
	  - Confirm data.table loads with multi threaded support (load library in R)
	  - A final note that installing data.table prior to installing devtools will produce an error as one of it's dependencies doesn't have openmp support (fs, see this GitHub issue: https://github.com/r-lib/fs/issues/345). You can get around this by temporarily removing the openmp edits to the Makevars file and installing devtools, then adding the changes back to Makevars.
10. Profit


