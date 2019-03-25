############################################################ 
# Dockerfile to build container with shiny server to run
# genomics analysis suite
# Based on r-base 
############################################################ 

FROM rocker/r-ver:3.5.1 

# File Author / Maintainer
MAINTAINER Matthias Becker <mbecker@uni-bonn.de>

RUN apt-get update && apt-get install -y \
    sudo \
    gdebi-core \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev \
    libxt-dev \
	libssl-dev \
	libv8-dev \
	libxml2-dev \
	libcairo2-dev \
	libsqlite-dev \
	libmariadbd-dev \
	libpq-dev \
	libssh2-1-dev \
	libudunits2-dev \ 
	libgdal-dev \
	libproj-dev \
    psmisc \
	procps \
	wget \
	libgit2-dev \
	file \
    git \
    libapparmor1 \
    libedit2 \
    lsb-release \
    psmisc \
    procps \
	python-setuptools
	
# Download and install shiny server
RUN wget --no-verbose https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/VERSION -O "version.txt" && \
    VERSION=$(cat version.txt)  && \
    wget --no-verbose "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb && \
    gdebi -n ss-latest.deb && \
    rm -f version.txt ss-latest.deb

#Ver 3.00
# CRAN packages
RUN    R -e "install.packages(list.of.packages <- c('shiny','shinyBS','RcppArmadillo','GSEABase','shinyjs','RColorBrewer','formula.tools','stringr','data.table','fdrtool','VennDiagram','devtools','colorspace','officer','magrittr','openxlsx','ggrepel','V8','WGCNA','svglite','visNetwork','ggpubr','gplots','bindrcpp','pheatmap','purrr'), dependencies = T)"
# BioconductoR packages
RUN    R -e "source('https://bioconductor.org/biocLite.R'); biocLite(c('rhdf5','DESeq2','IHW','tximport','clusterProfiler','org.Hs.eg.db','org.Mm.eg.db','org.Mmu.eg.db','sva','limma','geneplotter','rhdf5','biomaRt','AnnotationDbi', 'impute', 'GO.db', 'preprocessCore','pcaGoPromoter','pcaGoPromoter.Mm.mm9','pcaGoPromoter.Hs.hg19','pathview','lpsymphony'),suppressUpdates=TRUE)"
RUN    R -e "library(devtools);devtools::install_github('ropensci/plotly', upgrade = 'never'); devtools::install_github('rstudio/crosstalk',force=TRUE, upgrade = 'never'); devtools::install_github('rstudio/DT', upgrade = 'never'); devtools::install_github('r-lib/later')"
RUN    R -e "install.packages(c('WGCNA','doMC'), dependencies = T)"
RUN    R -e "if (!requireNamespace('BiocManager', quietly = TRUE))     install.packages('BiocManager'); BiocManager::install('GSEABase', update=F, ask=F,  version = '3.8')"

RUN rm -rf /srv/shiny-server
RUN git clone https://github.com/UlasThomas/Shiny-Seq /srv/shiny-server
WORKDIR /srv/shiny-server
#RUN git checkout 0c3225bd2bcd89e309d754313c021cacf0c28c9f

EXPOSE 3838

CMD /usr/bin/shiny-server

# helper commands for testing
# docker build -t makaho/shiny-server .
# docker run -it --rm -p 3838:3838 makaho/shiny-server /bin/bash
# echo "preserve_logs true;" >> /etc/shiny-server/shiny-server.conf
