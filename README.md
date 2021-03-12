# Install R, Python and Snakemake in Conda environments
Reproducible analysis with R, Python and Snakemake via Conda

This repository contains setup scripts to automate the installation of R, Python and Snakemake and other software used for bioinformatic and data science projects in conda environments. Setup will install 1) r-base and r-essentials, 2) python, JupyterLab and core SciPy packages and 3) Snakemake workflow management system allowing interactive or batch analysis on local, cluster and cloud platforms.

Setup will also streamline the setup of R within a conda env by creating: project-level .Rprofile and .Renvironment files, a .here file to set project-level working directory in R, and an external R library directory for ad hoc installation of R packages not yet on conda-forge or bioconda channels.

Importantly, when you use R with conda you need to stick to conda as much as you can. Whenever possible, DON'T LET R INSTALL PACKAGES FOR YOU. In other words ALWAYS USE CONDA TO INSTALL THE PACKAGES YOU NEED unless there is not a conda recipe for that package. In that case, see below.

Currently only supports unix based systems.

## Instructions

### Create project folder with Cookiecutter

Optional, but higly encouraged:
Prior to setting up an analysis environment, create a project directory with well-defined directory structure. This should contain everything needed for an analysis, with the possible exception of raw data maintained on a dedicated data storage drive. To do so, conda will need to be installed, as we will be using the cookiecutter package to create the project directory:

```
conda install cookiecutter
```

After installing cookiecutter and activating the relevant conda environment, go to the directory where to want to create your main project folder.
Clone the following cookiecutter snakemake template:

```
conda activate cookiecutter-env
cookiecutter https://github.com/NSLS-II/scientific-python-cookiecutter
```


This creates a template defined by the snakemake project with the following structure:

End optional steps.


To 

Clone this repo into your main project directory.

Run "create_envs.sh" script to create conda environments (and install conda, if needed)"

```
source ./conda-r-python-snakemake/create_envs.sh
```

Next run R setup up script (a symlink is created in the current directory):

```
source setup_R.sh
```

Important note about installing R packages (adapted from [here](https://community.rstudio.com/t/why-not-r-via-conda/9438/4)):
Do not mix conda proprietary channels (anaconda, r) with open-source channels (conda-forge, bioconda, defaults). Problems will arise. R versions in different channels are often not compatible as they are compiled with different internals. If you setup R with the setup scripts in this repo, there will be a R library folder for installing external R libraries from CRAN/Bioconductor/Github, created in the current working directory. If you must install R packages directly in R (conda recipe not available), always specify the library path in R using something like:

```
install.packages(pkgs, lib=Sys.getenv('./R/4.0/library')
```

This will very likely install packages that have already been installed via conda channels, including those installed in r-essentials. Consequently, when installing an R package externally (and if you need to do this a lot, then conda may not be the solution for you), look up the dependencies in the docs and install those first with conda.

Currently, Rstudio is not available on open-source channels. Consequently, if you want to use Rstudio with R in the installed Conda environment there are a few options:
- On a personal machine, use a locally installed Rstudio Desktop.
- On a cloud platform, use Rstudio Server, described here: [gce-startup](https://github.com/ctrhodes/gce-startup)
- On a cluster with environment modules such as LMOD, use the included Rstudio startup script:

```
source start_Rstudio.sh
```

Alternatively, the excellent Jupyter package and dependcies were installed into the current conda environment. Rstudio shortcuts were also set during IRkernel setup. To use R in Jupyter:

```
jupyter-lab
```

```
jupyter notebook --kernel=ir
```

If you have multiple R versions and/or irkernels (probably not a good idea), can choose specific kernel by:

```
jupyter-lab --kernel=ir
```

