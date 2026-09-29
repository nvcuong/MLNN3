FROM ubuntu:jammy

## Set locale
RUN apt-get update -qq \
    && apt-get install locales \
    && echo "en_GB.UTF-8 UTF-8" > /etc/locale.gen \
    && locale-gen en_GB.utf8 \
    && /usr/sbin/update-locale LANG=en_GB.UTF-8
ENV LC_ALL en_GB.UTF-8
ENV LANG en_GB.UTF-8

## Set other env vars
ENV DEBIAN_FRONTEND noninteractive
ENV TZ UTC

## Setup deb packages
RUN apt-get install --yes --no-install-recommends wget ca-certificates gnupg \
    && apt-get install --yes --no-install-recommends python3-dbus python3-gi python3-apt make \
    && apt-get install --yes --no-install-recommends libcurl4-openssl-dev libv8-dev gcc libxml2-dev libfontconfig1-dev \
    && apt-get install --yes --no-install-recommends libharfbuzz-dev libfribidi-dev \
    && apt-get install --yes --no-install-recommends libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev \
    && apt-get install --yes --no-install-recommends fftw3 fftw3-dev pkg-config \
    && apt-get install --yes --no-install-recommends texlive-latex-base texlive-fonts-recommended texlive-fonts-extra texlive-latex-extra \
    && apt-get install --yes --no-install-recommends lmodern \
    && apt-get install --yes --no-install-recommends r-base r-base-dev 

## Setup R
RUN Rscript -e 'install.packages("codetools")' \
    && Rscript -e 'install.packages("V8")' \
    && Rscript -e 'install.packages("RcppEigen")' \
    && Rscript -e 'install.packages("data.table")' \
    && Rscript -e 'install.packages("ggplot2")' \
    && Rscript -e 'install.packages("coda")' \
    && Rscript -e 'install.packages("commonmark")' \
    && Rscript -e 'install.packages("markdown")' \
    && Rscript -e 'install.packages("rmarkdown")' \
    && Rscript -e 'install.packages("tinytex")' \
    && Rscript -e 'install.packages("tidyverse")' \
    && Rscript -e 'install.packages("rpart")' \
    && Rscript -e 'install.packages("rpart.plot")' \
    && Rscript -e 'install.packages("datasets")' \
    && Rscript -e 'install.packages("caret")' \
    && Rscript -e 'install.packages("earth")' \
    && Rscript -e 'install.packages("kknn")' \
    && Rscript -e 'install.packages("adabag")' \
    && Rscript -e 'install.packages("e1071")' \
    && Rscript -e 'install.packages("boot")' \
    && Rscript -e 'install.packages("randomForest")' \
    && Rscript -e 'install.packages("dbscan")' \
    && Rscript -e 'install.packages("klaR")' \
    && Rscript -e 'install.packages("readr")' \
    && Rscript -e 'install.packages("class")' \
    && Rscript -e 'install.packages("glmnet")' \
    && Rscript -e 'install.packages("DALEX")' \
    && Rscript -e 'install.packages("Rtsne")' \
    && Rscript -e 'install.packages("KODAMA")' \
    && Rscript -e 'install.packages("xgboost")' \
    && Rscript -e 'install.packages("keras3")'

## Install git and RStudio Server
RUN apt-get install --yes --no-install-recommends gdebi-core git-all \
    && wget -q https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2023.09.0-463-amd64.deb \
    && apt-get install --yes --no-install-recommends ./rstudio-server-2023.09.0-463-amd64.deb \
    && rm rstudio-server-2023.09.0-463-amd64.deb

## Next, setup user and sudo privileges
RUN apt-get install --yes --no-install-recommends sudo \
    && useradd -m -s /bin/bash rstudio \
    && echo "rstudio ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers \
# Disable login requirements for RStudio and ensure only listens on localhost
    && echo "auth-none=1\nwww-address=127.0.0.1\nwww-port=8787" >> /etc/rstudio/rserver.conf \
    && echo "USER=rstudio" >> /etc/environment \
# Enable rstudio to write to packages folder
    && chown root:rstudio "/usr/lib/R/site-library" \
    && chmod g+ws "/usr/lib/R/site-library"

## APT config & tidyup
RUN echo 'APT::Install-Recommends "false";' > /etc/apt/apt.conf.d/90local-no-recommends \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
    && rm -rf /var/lib/apt/lists/*
