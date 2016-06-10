---
layout: post
title:  "Wrangling metadata <strike>without Excel</strike>"
categories: database
author: trevor
---

The ZiBRA roadtrip has involved multiple teams working simultaneously on different steps of the overall sequencing pipeline, from sample selection at the LACENs to RT-PCR to library prep, etc... Each step has some metadata associated with it. We want the eventual sequence to maintain as much linkage to associated epidemiological (date and municipality, etc...) and experimental (Ct, barcode, etc...) metadata as possible. Without a strong push these linkages will end up distributed across a plethora of lab notebooks and Excel spreadsheets. In the last few days, I've tried to get all the important ZiBRA metadata into a single database. 

I've been working from [nextstrain-db](http://github.com/blab/nextstrain-db), which was built by [Charlton Callender](http://bedford.io/team/charlton-callender/), a very talented undergraduate at the University of Washington, to manage sequence data and associated metadata for [nextstrain](http://nextstrain.org) builds. The basic strategy has been to have an AWS instance running a [RethinkDB](http://rethinkdb.org) server that maintains a table with sample-by-sample metadata and sequences. Having a single server is very helpful to keep data flowing to single location. However, lack of internet has rather complicated this plan. Because of this, I've set up a local RethinkDB instance that mirrors the AWS server. Changes can be made to this local database and then 'pushed' up to the remote database Git-style when we manage to get internet access. Changes to the remote can also be 'pulled' down to the local instance.

We're running the app [Chateau](http://github.com/neumino/chateau) to provide a web UI for making changes to individual-samples in the database, while larger modifications are done through Python interaction scripts that can [upload tsv files](http://github.com/blab/nextstrain-db/vdb/zibra_metadata_upload.py) for many documents/fields simultaneously (to upload new samples or to update metadata of existing samples) or [download canonically formatted tsvs or fastas](http://github.com/blab/nextstrain-db/vdb/zibra_download.py).

## Update 6/9/2016

Turns out having everyone on board with a web UI and these extra moving parts is overly complex and unworkable. The current protocol is:

#### **Keep one Excel notebook per collection location**

Each notebook is row-by-bow sample-list for samples processed for this location. Each notebook should be titled in the form of `ZBRA_lacen_natal_samples.xls` (for ZBRA samples) or `ZBRB_lacen_joao_pessoa_samples.xls` (for ZBRB samples). These notebooks live in dropbox under `sample-lists/`.

#### **Follow a strict template when completing these Excel notebooks**

It looks something like:

strain  | sample_number | lab           | lab_id         | rt_positive | ct      | ...
--------|---------------|---------------|----------------|-------------|---------|----
`ZBRA1` | `1`           | `lacen_natal` | `150501001949` | `TRUE`      | `38.52` | ...
`ZBRA2` | `2`           | `lacen_natal` | `160201001040` | `TRUE`      | `37.22` | ...
`ZBRA3` | `3`           | `lacen_natal` | `160201000158` | `FALSE`     |         | ...

Further details of fields are below under "Excel schema".

#### **Sync these notebooks to the RethinkDB instance**

These Excel notebooks can be exported as `.tsv` and then synced to the RethinkDB database with:

`python vdb/zibra_metadata_upload.py -db vdb -tb zibra --source zibra --virus zika --authors ZiBRA --ftype tsv --fname ZBRA_lacen_natal_samples.tsv`

## Excel schema

The Excel schema is as follows:

* `strain`: This study / strain ID in the form of `ZBRA1`, `ZBRB1`, etc... This is the *primary key* of the table and is required for every sample.
* `sample_number`: This is the sample number from this collection location, `1`, `2`, `3`, etc...
* `lab`: Collection lab, `lacen_natal`, `lacen_joao_pessoa`, `fiocruz_recife`, etc...
* `lab_id`: This is LACEN sample ID (or other institution sample ID). Should be a 12-digit number. This very important to be able to link sample to clinical metadata. Each LACEN uses its own IDs.
* `rt_positive`: Whether RT-PCR is positive for Zika, `true` or `false`.
* `ct`: Ct value of positive RT-PCR result.
* `extraction_date`: Date of RNA extraction.
* `amplicon_concentration`: Purity of DNA after PCR amplification. Measured in ng/ul.
* `minion_barcodes`: List of MinION library/barcodes associated with sample separated by commas, for example `2_nb08,2_nb09`.
* `country`: All samples should be `brazil`.
* `state`: Name of Brazilian state of patient origin. Should be snakecase without accents, `bahia`, `rio_grande_do_norte`, etc...
* `municipality`: Name of municipality of patient origin. Should be snakecase without accents, `canguaretama`, `natal`, `sao_goncalo_do_amarante`, etc...
* `collection_date`: Collection date of the sample. Should be formatted as `2015-07-27` (YYYY-MM-DD). If a sample lacks complete date information, enter as `2015-07-XX` (day unknown) or `2015-XX-XX` (month and day unknown).
* `onset_date`: Date of symptom onset. Should be formatted as `2015-07-27` (YYYY-MM-DD).
* `host_species`: Host species. Human samples are `human`.
* `patient_age`: Patient age, `30y`, `6m`, `10d`, etc...
* `patient_sex`: Patient sex, `male` or `female`.
* `pregnant`: Whether the patient was pregnant, `true` or `false`.
* `pregnancy_week`: Week of pregnancy, as number `10`, `12`, etc....
* `pregnancy_trimester`: Trimester of pregnancy `1`, `2`, `3`.
* `microcephaly`: Whether sample was linked to microcephaly, `true` or `false`.
* `sample_type`: Type of sample, `serum`, `urine`, etc...
* `symptoms`: Free-form text of clinical symptoms.
* `notes`: Free-form text for notes associated with this sample.

## Documentation

Notes on the ZiBRA-specific nextstrain-db implementation can be found [here](http://github.com/blab/nextstrain-db/ZIBRA.md). Other documentation:

* [General database information](http://github.com/blab/nextstrain-db)
* [Running Chateau](http://github.com/blab/nextstrain-db#chateau)
* [Notes on RethinkDB](http://github.com/blab/nextstrain-db/RETHINKDB.md)
* [Interaction commands](http://github.com/blab/nextstrain-db/vdb/)

## Thanks

Special thanks to Charlton Callender for building things and running support from Seattle.
