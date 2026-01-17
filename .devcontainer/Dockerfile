FROM ghcr.io/rocker-org/devcontainer/tidyverse:4

RUN install2.r --error --skipinstalled --ncpus -2 \
    adabag \
    V8 \
    RcppEigen \
    boot \
    caret \
    class \
    codetools \
    DataExplorer \
    DALEX \
    GGally \
    datasets \
    data.table \
    dbscan \
    doMC \
    dplyr \
    e1071 \
    earth \
    forcats \
    foreach \
    ggforce \
    ggplot2 \
    coda \
    commonmark \
    markdown \
    rmarkdown \
    glmnet \
    glue \
    ISLR2 \
    kknn \
    klaR \
    randomForest \
    readr \
    Rtsne \
    keras3 \
    latex2exp \
    mgcv \
    mlr3 \
    mlr3learners \
    mlr3verse \
    modeldata \
    mvtnorm \
    nnet \
    pixmap \
    plotly \
    ranger \
    recipes \
    rnaturalearth \
    rnaturalearthdata \
    rpart \
    rpart.plot \
    rsample \
    skimr \
    tidyverse \
    webshot \
    xgboost \
    yardstick \
    && rm -rf /tmp/downloaded_packages \
    && strip /usr/local/lib/R/site-library/*/libs/*.so

RUN DEBIAN_FRONTEND=noninteractive apt-get -qq -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get -qq -y install \
    libgeos-dev \
    libudunits2-dev \
    libproj-dev \
    libgdal-dev \
    libxt6 \
    libglpk-dev \
    && DEBIAN_FRONTEND=noninteractive apt-get -qq -y clean \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --break-system-packages tensorflow \
    && pip install --break-system-packages tensorflow-hub tensorflow-datasets scipy requests Pillow h5py pandas pydot \
    && echo "RETICULATE_PYTHON=/usr/bin/python3" >> /usr/local/lib/R/etc/Renviron.site

RUN echo "auth-none=1" >> /etc/rstudio/rserver.conf \
    && echo "USER=rstudio" >> /etc/environment
