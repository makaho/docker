# Set the base image to debian based miniconda3 
FROM conda/miniconda3 
# File Author / Maintainer 
MAINTAINER Matthias Becker
 
RUN apt-get clean && apt-get update && apt-get install -y locales 
RUN echo "LC_ALL=en_US.UTF-8" >> /etc/environment 
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen 
RUN echo "LANG=en_US.UTF-8" > /etc/locale.conf 
RUN locale-gen en_US.UTF-8 

RUN conda update -y conda 
RUN conda config --add channels r 
RUN conda config --add channels defaults 
RUN conda config --add channels conda-forge 
RUN conda config --add channels bioconda 
RUN conda install -y -c conda-forge readline=6.2
RUN conda install -y r=3.4.1
RUN conda install -y trimmomatic=0.36 
RUN conda install -y bowtie2=2.3.4.1 
RUN conda install -y picard=2.18.4 
RUN conda install -y bedtools=2.27.1 
RUN conda install -y homer=4.9.1 
RUN conda install -y samtools=1.8 
RUN conda install -y idr=2.0.3 
RUN conda install -y ucsc-bedgraphtobigwig=366


# install MACS2
RUN apt-get install -y python python-pip git libboost-all-dev
RUN pip2 install numpy
RUN pip2 install MACS2

#
WORKDIR /tools/
RUN git clone https://github.com/kundajelab/phantompeakqualtools
WORKDIR /tools/phantompeakqualtools/


RUN R -e "install.packages(c('snow', 'snowfall', 'bitops', 'caTools'), repos='http://cran.us.r-project.org'); source('http://bioconductor.org/biocLite.R'); biocLite('Rsamtools',suppressUpdates=TRUE)"

RUN tar xzf spp_1.14.tar.gz
RUN sed -i 's/hash_map/unordered_map/g' spp-1.14/src/bed2vector.cpp
RUN sed -i 's/hash_map/unordered_map/g' spp-1.14/src/maqread.cpp
RUN sed -i 's/"pc.h"/<unordered_map>/g' spp-1.14/src/bed2vector.cpp
RUN sed -i 's/"pc.h"/<unordered_map>/g' spp-1.14/src/maqread.cpp
RUN tar czf spp_1.14.tar.gz spp-1.14/
RUN R -e "install.packages('./spp_1.14.tar.gz')"

WORKDIR /tools/
RUN apt-get install wget
RUN wget https://gist.githubusercontent.com/brianhill11/7aeeeb6d94edfb868e5595aac04a0dd6/raw/14736a259e04e1cf4ef7abb45f6a0eca9b05a23f/split_bam.py