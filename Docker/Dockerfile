FROM ubuntu:16.04

MAINTAINER bhaas@broadinstitute.org

RUN apt-get update && apt-get install -y gcc g++ perl python automake make \
                                       wget curl libdb-dev \
				       bzip2 zlibc zlib1g zlib1g-dev  default-jre && \
    apt-get clean

RUN curl -L https://cpanmin.us | perl - App::cpanminus

## set up tool config and deployment area:

ENV SRC /usr/local/src
ENV BIN /usr/local/bin

ENV DATA /usr/local/data
RUN mkdir $DATA


## perl lib installations

RUN cpanm install Set::IntervalTree

RUN cpanm install DB_File

RUN cpanm install URI::Escape


######################
## Tool installations:
######################


##############
## STAR

RUN STAR_URL="https://github.com/alexdobin/STAR/archive/2.5.2a.tar.gz" &&\
    wget -P $SRC $STAR_URL &&\
    tar -xvf $SRC/2.5.2a.tar.gz -C $SRC && \
    mv $SRC/STAR-2.5.2a/bin/Linux_x86_64_static/STAR /usr/local/bin


###############
## STAR-Fusion:

RUN STAR_FUSION_URL="https://github.com/STAR-Fusion/STAR-Fusion/releases/download/v1.0.0/STAR-Fusion-v1.0.0.FULL.tar.gz" && \
    cd $SRC && \
    wget $STAR_FUSION_URL && \
    tar xvf STAR-Fusion-v1.0.0.FULL.tar.gz

ENV STAR_FUSION_HOME $SRC/STAR-Fusion-v1.0.0


###################
## FusionInspector

ENV FI_VERSION v1.0.1
RUN FI_URL="https://github.com/FusionInspector/FusionInspector/releases/download/${FI_VERSION}/FusionInspector-${FI_VERSION}.FULL.tar.gz" && \
    cd $SRC && \
    wget $FI_URL && \
    tar -xvf FusionInspector-${FI_VERSION}.FULL.tar.gz

ENV FUSION_INSPECTOR_HOME $SRC/FusionInspector-${FI_VERSION}


########
# Samtools

RUN SAMTOOLS_URL="https://github.com/samtools/samtools/releases/download/1.3.1/samtools-1.3.1.tar.bz2" && \
   cd $SRC && \
   wget $SAMTOOLS_URL && \
   tar xvf samtools-1.3.1.tar.bz2 && \
   cd samtools-1.3.1/htslib-1.3.1 && ./configure && make && make install && \
   cd ../ && ./configure --without-curses && make && make install


########
# Trinity

RUN TRINITY_URL="https://github.com/trinityrnaseq/trinityrnaseq/archive/Trinity-v2.3.2.tar.gz" && \
   cd $SRC && \
   wget $TRINITY_URL && \
   tar xvf Trinity-v2.3.2.tar.gz && \
   cd trinityrnaseq-Trinity-v2.3.2 && make


ENV TRINITY_HOME /usr/local/src/trinityrnaseq-Trinity-v2.3.2


########
# GMAP

RUN GMAP_URL="http://research-pub.gene.com/gmap/src/gmap-gsnap-2017-01-14.tar.gz" && \
    cd $SRC && \
    wget $GMAP_URL && \
    tar xvf gmap-gsnap-2017-01-14.tar.gz && \
    cd gmap-2017-01-14 && ./configure && make && make install

