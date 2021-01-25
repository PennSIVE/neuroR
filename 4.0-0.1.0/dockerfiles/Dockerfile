FROM pennsive/neurodocker:buster
ARG R_VERSION_MAJOR=4
ARG R_VERSION_MINOR=0
ARG R_VERSION_PATCH=3
ARG CONFIGURE_OPTIONS="--with-cairo --with-jpeglib --enable-R-shlib --with-blas --with-lapack"
RUN apt-get update && apt-get install -y \
            gfortran \
            git \
            git-annex \
            g++ \
            libreadline-dev \
            libx11-dev \
            libxt-dev \
            libpng-dev \
            libjpeg-dev \
            libcairo2-dev \   
            libssl-dev \ 
            libxml2-dev \
            libudunits2-dev \
            libgdal-dev \
            libbz2-dev \
            libzstd-dev \
            liblzma-dev \
            libpcre2-dev \
            locales \
            screen \
            texinfo \
            texlive \
            texlive-fonts-extra \
            vim \
            wget \
            xvfb \
            tcl8.6-dev \
            tk8.6-dev \
            cmake \
            curl \
            unzip \
            libcurl4-gnutls-dev \
            libgsl-dev \
            libcgal-dev \
            libglu1-mesa-dev \
            libglu1-mesa-dev \
            libtiff5-dev \
            python3 \
            python3-pip \
            && pip3 install nipype pybids pyradiomics drmaa flywheel-sdk datalad datalad-neuroimaging datalad-container numpy scipy nibabel h5py \
            && wget https://cran.rstudio.com/src/base/R-${R_VERSION_MAJOR}/R-${R_VERSION_MAJOR}.${R_VERSION_MINOR}.${R_VERSION_PATCH}.tar.gz \
            && tar zxvf R-${R_VERSION_MAJOR}.${R_VERSION_MINOR}.${R_VERSION_PATCH}.tar.gz \
            && rm R-${R_VERSION_MAJOR}.${R_VERSION_MINOR}.${R_VERSION_PATCH}.tar.gz \
            && cd /R-${R_VERSION_MAJOR}.${R_VERSION_MINOR}.${R_VERSION_PATCH} \
            && ./configure ${CONFIGURE_OPTIONS} \ 
            && make \
            && make install \
            && echo 'options(repos = c(CRAN = "https://cran.rstudio.com/"), download.file.method = "libcurl")' >> /usr/local/lib/R/etc/Rprofile.site \
            && Rscript -e "\
            install.packages(c('devtools', 'BiocManager', 'argparser', 'reticulate', 'rlist', 'oro.nifti', 'oro.dicom', 'fslr', 'WhiteStripe', 'matrixStats', 'R.matlab', 'abind', 'R.utils', 'RNifti', 'stapler', 'testthat', 'hexSticker', 'dplyr', 'oasis', 'fslr', 'plyr', 'misc3d', 'pixmap', 'colormap', 'gifti', 'cifti', 'ROCR', 'freesurfer', 'freesurferformats', 'broom', 'broom.mixed', 'geepack', 'lme4', 'magrittr', 'neurobase', 'pbapply', 'purrr', 'readr', 'stringr', 'tibble', 'tidyr', 'voxel', 'forcats', 'gridExtra', 'RIA', 'Rfast', 'RJSONIO', 'pbmcapply', 'vesselr', 'tidyverse', 'caret', 'DMwR', 'openxlsx', 'acPCA', 'randomForest', 'RSpectra', 'doRNG', 'doParallel', 'doMC', 'doRedis', 'future.apply', 'badgecreatr'), repos='http://cran.rstudio.com'); \
            BiocManager::install(c('rhdf5', 'rhdf5filters', 'DelayedArray', 'DelayedMatrixStats', 'HDF5Array'));"
RUN Rscript -e "remotes::install_github(c('ANTsX/ANTsRCore@v0.7.4.9', 'ANTsX/ANTsR@v0.5.7.4'))" \
Rscript -e "remotes::install_github(c('muschellij2/extrantsr', 'muschellij2/NiftiArray', 'muschellij2/oasis', 'avalcarcel9/mimosa', 'muschellij2/malf.templates', 'avalcarcel9/rtapas'))" \
# todo: figure out why installing lesiontools from github doesn't work
git clone https://github.com/jdwor/lesiontools.git && Rscript -e "install.packages('./lesiontools', repos = NULL, type='source')" && rm -rf lesiontools && \
# todo: not sure whether to include this PCA library in main image
git clone https://github.com/linzx06/AC-PCA.git && Rscript -e "chooseCRANmirror(graphics=FALSE, ind=60); install.packages('./AC-PCA/R_package/acPCA', repos = NULL, type='source')" && rm -rf AC-PCA && \
git clone https://github.com/PennSIVE/freesurfer.git && cd freesurfer && git checkout mri_vol2surf && Rscript -e "install.packages('.', repos = NULL, type='source')" && cd .. && rm -rf freesurfer && \
ln -s bash /bin/sh.bash && mv /bin/sh.bash /bin/sh && \
# ^freesurfer weirdly tries to call a bash script with sh, so replace sh with bash
fspython -m pip install tensorflow==1.13.1