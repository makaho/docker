############################################################ 
# Dockerfile to build container with shiny server to run
# genomics analysis suite
# Based on r-base 
############################################################ 

FROM r-base:latest 

# File Author / Maintainer
MAINTAINER Matthias Becker <mbecker@uni-bonn.de>

RUN apt-get update && apt-get install -y -t unstable \
    sudo \
    gdebi-core \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev/unstable \
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
	libproj-dev
	
# Download and install shiny server
RUN wget --no-verbose https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/VERSION -O "version.txt" && \
    VERSION=$(cat version.txt)  && \
    wget --no-verbose "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb && \
    gdebi -n ss-latest.deb && \
    rm -f version.txt ss-latest.deb


#RUN    R -e "install.packages(c('shiny', 'shinyBS', 'shinyjs', 'RcppArmadillo')); library('shinyBS'); library('RcppArmadillo'); library(shinyjs); library(shiny)"
#RUN    R -e "source('https://bioconductor.org/biocLite.R'); biocLite(c('rhdf5','DESeq2','tximport', 'clusterProfiler', 'org.Hs.eg.db', 'limma', 'geneplotter', 'sva', 'org.Mmu.eg.db', 'org.Mm.eg.db', 'biomaRt')); library('rhdf5'); library(tximport); library(DESeq2); library(clusterProfiler); library(org.Hs.eg.db); library(org.Mm.eg.db); library(org.Mmu.eg.db); library('sva'); library('limma'); library(geneplotter); library(biomaRt)"
#RUN    R -e "install.packages(c('RColorBrewer', 'stringr', 'formula.tools', 'fdrtool', 'VennDiagram', 'data.table', 'colorspace')); library(RColorBrewer); library(stringr); library(formula.tools); library('data.table'); library('fdrtool'); library('VennDiagram'); library('colorspace')"
#RUN    R -e "install.packages('devtools'); devtools::install_github('ropensci/plotly'); library(plotly); devtools::install_github('rstudio/crosstalk',force=TRUE); library(crosstalk)"
#RUN    R -e "install.packages('gplots',dependencies = TRUE); library('gplots')"
#RUN    R -e "install.packages(c('officer', 'magrittr', 'flextable', 'svglite', 'visNetwork', 'pheatmap', 'ggrepel', 'V8')); library(officer); library(magrittr); library(flextable); library(svglite); library(visNetwork); library(pheatmap); library(ggrepel); library(V8)"
#RUN    R -e "source('https://bioconductor.org/biocLite.R'); biocLite('pcaGoPromoter'); biocLite('pcaGoPromoter.Mm.mm9'); biocLite('pcaGoPromoter.Hs.hg19'); library(pcaGoPromoter); library(pcaGoPromoter.Mm.mm9); library(pcaGoPromoter.Hs.hg19)"
#RUN    R -e "install.packages('devtools'); devtools::install_github('rstudio/DT'); library(DT)"
#RUN    R -e "source('https://bioconductor.org/biocLite.R'); biocLite('pathview'); biocLite('GSEABase'); library(pathview); library(GSEABase)"
#RUN    R -e "source('http://bioconductor.org/biocLite.R'); biocLite(c('AnnotationDbi', 'impute', 'GO.db', 'preprocessCore')); install.packages('WGCNA')"


#Ver 3.00
# CRAN packages
RUN    R -e "install.packages(list.of.packages <- c('shiny','shinyBS','RcppArmadillo','shinyjs','RColorBrewer','formula.tools','stringr','data.table','fdrtool','VennDiagram','devtools','colorspace','officer','magrittr','openxlsx','ggrepel','V8','WGCNA','svglite','visNetwork','ggpubr','gplots','bindrcpp','pheatmap'), dependencies = T)"
# BioconductoR packages
RUN    R -e "source('https://bioconductor.org/biocLite.R'); biocLite(c('rhdf5','DESeq2','IHW','tximport','clusterProfiler','org.Hs.eg.db','org.Mm.eg.db','org.Mmu.eg.db','sva','limma','geneplotter','rhdf5','biomaRt','AnnotationDbi', 'impute', 'GO.db', 'preprocessCore','pcaGoPromoter','pcaGoPromoter.Mm.mm9','pcaGoPromoter.Hs.hg19','pathview'),suppressUpdates=TRUE)"
RUN    R -e "devtools::install_github('ropensci/plotly', upgrade = 'never')"
RUN    R -e "devtools::install_github('rstudio/crosstalk',force=TRUE, upgrade = 'never')"
RUN    R -e "devtools::install_github('rstudio/DT', upgrade = 'never')"
RUN    R -e "devtools::install_github('r-lib/later')"
RUN    R -e "install.packages(c('WGCNA','doMC'), dependencies = T)"


RUN    cp -R /usr/local/lib/R/site-library/shiny/examples/* /srv/shiny-server/

EXPOSE 3838

RUN echo "\nR_MAX_NUM_DLLS=200" >> /etc/R/Renviron.site

CMD /usr/bin/shiny-server

# docker build -t makaho/shiny-server .
# docker run -it --rm -p 3838:3838 -v E:\data\docker\ShinySeq\ver_3.00:/srv/shiny-server/app makaho/shiny-server /bin/bash
# export R_MAX_NUM_DLLS=200 
# in /etc/R/Renviron.site
# must be <614
# echo "\npreserve_logs true;" >> /etc/shiny-server/shiny-server.conf
