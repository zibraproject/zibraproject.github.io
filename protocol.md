---
layout: default
title: "Protocol"
---

# The ZiBRA Pipeline

During the Zika project we have developed a fully integrated sequencing pipeline which is described in our paper at Nature Protocols.

This page describes in more detail the bioinformatics pipeline from that paper.

## Installing Docker

For Linux:

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04

For Windows:

Ideally Docker for Windows:

https://docs.docker.com/docker-for-windows/

Some users may need to use Docker Toolbox:

https://docs.docker.com/toolbox/toolbox_install_windows/

For Mac:

https://docs.docker.com/docker-for-mac/

## Quick start

### MinION pipeline

  docker pull zibra/zibra:latest
  docker run -t -i zibra/zibra:latest /bin/bash

  ## run on the test sample
  mkdir whoref
  cd whoref
  wget https://s3.climb.ac.uk/nanopore/Zika_Control_Material_R9.4_2D.tar
  tar xvf Zika_Control_Material_R9.4_2D.tar
  fast5_to_consensus.sh ZikaAsian WHO 20161118_Zika/downloads/pass/NB08

### Local offline basecalling

Currently we are recommending Oxford Nanopore’s Albacore for local base calling. If basecalling offline on Windows or Mac, we recommend this is done on the native operating system rather than in the Docker container, for reasons of speed.

We also do not recommend using Albacore demultiplexing at present, as this is rather lenient as it requires only a single barcode copy.

To basecall an R9.4 1D dataset on Windows with eight threads use:

``read_fast5_basecaller.exe -c r94_linear.cfg -i C:\data\reads\run -s C:\Users\nick\data\run -t 8``

### Demultiplexing

Currently we prefer Ryan Wick’s porechop, but a special version of is bundled with the Docker file used to make it compatible with nanopolish. 

To demultiplex from /data/run, do something like (for reads stored in /data/run):

poretools fasta /data/run > run.fasta
porechop --untrimmed -i run.fasta -b porechop-run --barcode_threshold 75 --threads 16 --check_reads 1000 --barcode_diff 2 —require_two_barcodes

### Mounting a local directory

You will probably want to mount a local directory where your reads are kept.

On Windows, say my data is in ``C:\Users\nick\data`` you would run:

docker run -t -v //C/Users/nick/data:/data -i zibra/zibra:latest /bin/bash

On Mac:

docker run -t -v /Users/nick/data:/data -i zibra/zibra:latest /bin/bash

Then use Porechop (in the container) to demultiplex the reads:

### Illumina pipeline- Quickstart

   docker pull zibra/zibra:latest
   docker run -t -i zibra/zibra:latest /bin/bashwget --no-check-certificate 
   wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR512/007/SRR5122847/SRR5122847_1.fastq.gz
   wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR512/007/SRR5122847/SRR5122847_2.fastq.gz
   illumina_pipeline.sh SRR5122847 SRR5122847_1.fastq.gz SRR5122847_2.fastq.gz ZikaAsian

## Credits

The ZiBRA Pipeline was developed with contributions from:

  - Nick Loman (MinION pipeline)
  - Jared Simpson (nanopolish SNP calling)
  - Matt Loose (nanopore demultiplexing script)
  - Karthik Gangavarapu (Illumina pipeline)
  - Nate Grubaugh (Illumina pipeline)
  - Kristian Andersen (Illumina pipeline)
  - Trevor Bedford (help with Docker and useful fixes)

It relies on a whole heap of open source software, thank you to all contributors.



