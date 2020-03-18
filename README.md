Bioinfo template
================

  - [Overview](#overview)
  - [Prerequisites](#prerequisites)
  - [Steps for getting up and
    running](#steps-for-getting-up-and-running)
      - [1. <span>Navigate in terminal</span> to the wished
        directory](#navigate-in-terminal-to-the-wished-directory)
      - [2. Clone the repository](#clone-the-repository)
      - [3. Navigate to the repository
        root](#navigate-to-the-repository-root)
      - [4. Build Docker image using
        Dockerfile](#build-docker-image-using-dockerfile)
      - [5. Run Docker container using the newly built
        image](#run-docker-container-using-the-newly-built-image)
      - [6. Access RStudio-Server running in the container through a
        browser](#access-rstudio-server-running-in-the-container-through-a-browser)
      - [7. Stopping and starting a
        container](#stopping-and-starting-a-container)
      - [8. Removing images and
        containers](#removing-images-and-containers)
      - [Bonus step: configure git](#bonus-step-configure-git)
  - [References](#references)

This is a working version of a template I use for new bioinformatic
projects. This template uses Docker containers for the R environment
called [Rocker](https://www.rocker-project.org/). There is also a paper
introducing Rocker by Boettiger and Eddelbuettel (2017).

## Overview

Here is a schema of the file structure. The file structure of this
default project is heavily inspired by [SchlossLabs new\_project github
repository](https://github.com/SchlossLab/new_project), article by
Wilson et al. (2017). [Russ Hyde](https://github.com/russHyde)’s blog
posts about [Working Directories and
RMarkdown](https://russ-hyde.rbind.io/post/working-directories-and-rmarkdown/)
and [Project
organisation](https://biolearnr.blogspot.com/2017/05/project-organisation.html)
have also given some inspiration to some parts of the project structure.

    project
    |- README.md                    # the top level description of content (this doc)
    |- CONTRIBUTING.md              # instructions for how to contribute to your project
    |- LICENSE                      # the license for this project
    |- CITATION                     # instructions on how to cite the work
    |
    |- study/
    | |- header.sty                 # LaTeX header file to format pdf version of manuscript
    | |- bibliography.bib           # BibTeX formatted references
    | |- csl/                       # csl files to format references
    | |- study.Rmd                  # executable Rmarkdown for this study, if applicable
    | |- study.md                   # Markdown (GitHub) version of the *.Rmd file
    | |- study.tex                  # TeX version of *.Rmd file
    | |- study.pdf                  # PDF version of *.Rmd file
    | |- study.html                 # html version of *.Rmd file
    |
    |- data                         # raw and primary data, are not changed once created
    | |- references/                # reference files to be used in analysis
    | |- raw/                       # raw data, will not be altered
    | |- processed/                 # cleaned data, will not be altered once created;
    |                               # will be committed to repo
    |
    |- docker                       # Files related to docker virtualisation
    | |- Dockerfile                 # Dockerfile defining the development environment
    | |- add_shiny.sh               # These four files below are all configuration files
    | |- disable_auth_rserver.conf  # used in building an image from the Dockerfile
    | |- pam-helper.sh              # obtained from Rocker projects github page:
    | |- userconf.sh                # https://github.com/rocker-org/rocker-versioned/tree/master/rstudio
    |                               # The Dockerfile has also heavily loaned code from the Rocker project:
    |                               # https://www.rocker-project.org/
    |
    |- presentations                # presentations about the project 
    | |- _output.yaml               # shared configurations for all presentations
    | |- style.css                  # css for modifying features in presentation
    | |- presentation.Rmd           # R revealjs presentation
    | |- presentation.html          # rendered html of presentation.Rmd
    | |- images/                    # images and other graphics for the presentation
    |
    |- code/                        # any programmatic code
    | |- test/                      # tests for the code
    |
    |- results                      # all output from workflows and analyses
    | |- tables/                    # tables and other tabular data
    | |- figures/                   # graphs, likely designated for manuscript figures
    | |- pictures/                  # diagrams, images, and other non-graph graphics
    |
    |- exploratory/                 # exploratory data analysis for study
    | |- notebook/                  # preliminary analyses
    | |- scratch/                   # temporary files that can be safely deleted or lost
    |
    |- Snakefile                    # executable Snakefile for this study, if applicable

## Prerequisites

There are some pieces of software that are needed to get this
development environment up and running. You should install:

  - Docker: <https://docs.docker.com/install/>
  - Git: <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git>

## Steps for getting up and running

### 1\. [Navigate in terminal](https://www.digitalocean.com/community/tutorials/basic-linux-navigation-and-file-management) to the wished directory

Your working directory should be the location where you wish to have
your Bioinformatic project in. This is the location where you will
download this Git repository with `git clone` command.

### 2\. Clone the repository

In order to get access to the files in the repository you should clone
this repository with command (on terminal):

``` bash
git clone https://github.com/ljmesi/Bioinfo-template.git
```

More information about cloning a repository can be found
[here](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository)
and
[here](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-clone)
is some more information on the `git clone` command.

### 3\. Navigate to the repository root

``` bash
cd Bioinfo-template
```

### 4\. Build Docker image using Dockerfile

[Dockerfile](https://docs.docker.com/engine/reference/builder/) is used
to [build](https://docs.docker.com/engine/reference/commandline/build/)
an image which in turn is used to
[run](https://docs.docker.com/engine/reference/commandline/run/)
containers.

``` bash
docker build -t image_name:version.number docker/
```

### 5\. Run Docker container using the newly built image

``` bash
docker run --name container_name -e PASSWORD=some_really_good_password -p 8787:8787 -v $(pwd):/home/rstudio image_name:version.number
```

Some more information on running Rocker containers can be found
[here](https://ropenscilabs.github.io/r-docker-tutorial/).

### 6\. Access RStudio-Server running in the container through a browser

[Here](https://ropenscilabs.github.io/r-docker-tutorial/02-Launching-Docker.html)
are instructions on accessing the container using a web browser.

### 7\. Stopping and starting a container

In step 5 started running container can be stopped by pressing `Ctrl` +
`C` and started again with command:

``` bash
docker start container_name
```

and stopped again with:

``` bash
docker stop container_name
```

### 8\. Removing images and containers

Local images can be listed with:

``` bash
docker images
```

and containers with:

``` bash
docker ps -a
```

Images can be removed with:

``` bash
# Using image_name:version.number
docker rmi image_name:version.number 
# Or using the image ID
docker rmi 2791ce80edf0
```

and containers with:

``` bash
# Using container's name
docker rm container_name
# Using container ID
docker rm 9fcd57ea7ccb
```

[Here](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes#removing-docker-images)
can be found general information on removing images and containers.
[Here](https://docs.docker.com/engine/reference/commandline/rm/) and
[here](https://docs.docker.com/engine/reference/commandline/rmi/) are
links to `docker rm` and `docker rmi` command descriptions respectively.

### Bonus step: configure git

Git can be configured in RStudio server by running the following
commands in the terminal panel:

``` bash
git config --global user.name "your_Github_user_name" && \
git config --global user.email "your_email@address.com" && \
git config --global color.ui true && \
# Store the Github personal access token forever in .git-credentials file
git config --global credential.helper store
```

## References

<div id="refs" class="references hanging-indent">

<div id="ref-Boettiger2017">

Boettiger, Carl, and Dirk Eddelbuettel. 2017. “An Introduction to
Rocker: Docker Containers for R.” *R Journal* 9 (2): 527–36.
[https://rocker-project.org.
http://arxiv.org/abs/1710.03675](https://rocker-project.org.%20http://arxiv.org/abs/1710.03675).

</div>

<div id="ref-Wilson2017">

Wilson, Greg, Jennifer Bryan, Karen Cranston, Justin Kitzes, Lex
Nederbragt, and Tracy K Teal. 2017. “Good enough practices in scientific
computing.” Edited by Francis Ouellette. *PLOS Computational Biology* 13
(6): e1005510. <https://doi.org/10.1371/journal.pcbi.1005510>.

</div>

</div>
