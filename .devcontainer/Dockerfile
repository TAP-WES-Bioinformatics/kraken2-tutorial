FROM mambaorg/micromamba:2.5.0-ubuntu22.04 AS app

# Version arguments
ARG KRAKEN2_VERSION="2.1.3"
ARG KRAKEN2_DB_DATE="20240112"

USER root
WORKDIR /

LABEL base.image="mambaorg/micromamba:2.5.0-ubuntu22.04"
LABEL dockerfile.version="1"
LABEL software="Kraken2"
LABEL software.version=${KRAKEN2_VERSION}
LABEL description="Kraken2 is a taxonomic sequence classifier that assigns taxonomic labels to DNA sequences using exact k-mer matches to a reference database"
LABEL website="https://github.com/DerrickWood/kraken2"
LABEL license="https://github.com/DerrickWood/kraken2/blob/master/LICENSE"
LABEL maintainer="Dylan Pilz"

ENV PATH="/opt/conda/envs/kraken2-env/bin:${PATH}" \
    LC_ALL=C.UTF-8 \
    KRAKEN2_DB=/kraken2-db

# Install Kraken2, KrakenTools, and KronaTools
RUN micromamba create -n kraken2-env -c conda-forge -c bioconda \
      kraken2=${KRAKEN2_VERSION} \
      krakentools \
      krona && \
    micromamba clean -a -y -f

# System packages: build-essential is required by ktUpdateTaxonomy.sh (compiles
# Krona's taxonomy lookup helper via `make`); wget/ca-certificates fetch data.
RUN apt-get update && apt-get install -y --no-install-recommends \
      build-essential \
      wget \
      ca-certificates && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

# Update Krona taxonomy database
RUN ktUpdateTaxonomy.sh

# Download pre-built k2_viral database
RUN mkdir -p /kraken2-db && \
    wget -qO- https://genome-idx.s3.amazonaws.com/kraken/k2_viral_${KRAKEN2_DB_DATE}.tar.gz \
      | tar -xz -C /kraken2-db/

RUN mkdir /data

WORKDIR /data

CMD ["kraken2", "--help"]

# Testing stage
FROM app AS test

RUN kraken2 --version && \
    kreport2krona.py --help && \
    ktImportText --help && \
    ls /kraken2-db/
