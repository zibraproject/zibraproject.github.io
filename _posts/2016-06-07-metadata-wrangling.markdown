---
layout: post
title:  "Wrangling metadata without Excel"
categories: database
author: trevor
---

# Wrangling metadata without Excel

The ZiBRA roadtrip has involved multiple teams working simultaneously on different steps of the overall sequencing pipeline, from sample selection at the LACENs to RT-PCR to library prep, etc... Each step here has some metadata associated with it. We want the eventual sequence to maintain as much linkage to associated clinical (symptoms), epidemiological (date and municipality) and experimental (Ct) metadata as possible. Without a strong push these linkages will end up distributed across a plethora of lab notebooks and Excel spreadsheets. In the last few days, I've tried to get all the important ZiBRA metadata into a single database. 

I've been working from the [nextstrain-db](http://github/blab/nextstrain-db), which was built by [Charlton Callender](http://bedford.io/team/charlton-callender/), a very talented undergraduate at the University of Washington, to manage sequence data and associated metadata for [nextstrain](http://nextstrain.org) builds. The basic strategy has been to have an AWS instance running a [RethinkDB](http://rethinkdb.org) server that maintains a table of sample-by-sample metadata and sequences. Having a single server is very helpful to keep data flowing to single location. However, lack of internet has rather complicated this plan. Because of this, I've set up a local RethinkDB instance that mirrors the AWS server. Changes can be made to this local database and then 'pushed' up to the remote database Git-style when we manage to get internet access. Changes to the remote can also be 'pulled' down to the local instance.

We're running the app [Chateau](http://github.com/neumino/chateau) to provide a web UI for making sample-by-sample changes to the database, while larger modifications are done through Python interaction scripts that can [upload tsv files](http://github/blab/nextstrain-db/vdb/zibra_metadata_upload.py) for many documents/fields simultaneously or [download canonically formatted tsvs or fastas](http://github/blab/nextstrain-db/vdb/zibra_download.py).

## Schema notes

Notes on the ZiBRA implementation can be found [here](http://github/blab/nextstrain-db/ZIBRA.md). The basic schema is as follows:

* `strain`: This is LACEN sample ID. Should be a 12-digit number. This is the *primary key* of the table.
* `amplicon_concentration`: Purity of DNA after PCR amplification. Measured in ng/ul.
* `citations`: Not crucial. This can be populated with citation information.
* `ct`: Ct value of positive RT-PCR result.
* `country`: All samples should be `brazil`.
* `date`: Collection date of the sample. Should be formatted as `2015-07-27` (YYYY-MM-DD).
* `division`: Name of Brazilian state of patient origin. Should be snakecase. Examples:
 * `bahia`
 * `para`
 * `paraiba`
 * `rio_grande_do_norte`
* `location`: Name of municipality of patient origin. Should be snakecase. Examples:
 * `canguaretama`
 * `natal` 
 * `nova_cruz`
 * `sao_goncalo_do_amarante`
* `onset_date`: Date of symptom onset. Should be formatted as `2015-07-27` (YYYY-MM-DD). 
* `patient_age`: Patient age in years.
* `patient_sex`: Patient sex, `male` or `female`.
* `public`: Whether the sample genome can be shared publicly, `true` or `false`.
* `region`: All samples should be `south_america`.
* `sequences`: Contains three fields:
 * `accession`: Same as `strain`, based on LACEN sample ID.
 * `locus`: All samples should be `genome`.
 * `sequence`: Genome sequence.
* `rt_positive`: Whether RT-PCR is positive for Zika, `true` or `false`.
* `timestamp`: The last edit time of the database entry in `2016-06-04-14-06` (YYYY-MM-DD-HH-MM) format. This should be
automatically managed by scripts/chateau.
* `virus`: All samples should be `zika`.

## Documentation

* [General database information](http://github/blab/nextstrain-db)
* [Running Chateau](http://github/blab/nextstrain-db)
* [Notes on RethinkDB](http://github/blab/nextstrain-db/RETHINKDB.md)
* [Interaction commands](http://github/blab/nextstrain-db/vdb/)
* [ZiBRA-specific notes](http://github/blab/nextstrain-db/ZIBRA.md)

## Thanks

Special thanks to Charlton Callender for building things and running support from Seattle.
