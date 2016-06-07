---
layout: post
title:  "Offline barcode identification and demultiplexing with nanonet"
categories: blog
author: matt
---

Like you I'm sure, I have been watching the Zibra Project with a mix of awe, jealousy and laughter. Awe at the scale of the project being undertaken, jealousy at not being in the beautiful country of Brazil sampling the food and culture, and laughter wondering which stage of the process @scalene is going to delay next. Strictly speaking I am probably on team 5 - the home team - trying to do what I can to help out on this journey from afar. Originally I signed on to develop <a href="">read until</a> for the trip. However, with the transition of the project from R7 to R9, which increases the sequencing speed from 70 to 250 bp/s, coupled with the choice of sequencing amplicons of <a href="">around 500 bp in length</a>, my role in the zibra project has changed a little. Those of you who have never sequenced amplicons on a nanopore flowcell may not appreciate the incredible yield and read generation rate that can be achieved with these DNA fragments. On R9 (@250b/s) the read numbers are astonishing and 1D accuracy has improved significantly. So I have been focusing on trying to facilitate real-time feedback to the teams sequencing in order that they can assess the quality of data generated for each run in as close to real time as possible.

Travelling on a bus through Brazil - a very nice looking bus it has to be said - suggests that internet access is poor. Team 4 have already alluded to the fact that they are busy trying to 'fix the internet' - so they need a complete offline pipeline for analysis. The team do have access to nanonet, ONT's first RNN based local base caller. We have updated minoTour to allow it to process R9 nanonet generated reads but the team are also multiplexing 12 samples in each run. Nanonet has no built in options for de-multiplexing on barcodes. Ideally this should be done in as close to real time as possible. Given the ever increasing quality of nanopore reads, we reasoned that de-multiplexing barcodes could be done in base space.

The first attempt at this was extremely simple, using ``matcher`` from the <a href="http://www.ebi.ac.uk/Tools/psa/emboss_matcher/nucleotide.html">EMBOSS package</a> to identify local similarities with all possible barcode permutations and select the most likely match. Those familiar with the ONT barcode matching pipelines will know that a small proportion of reads cannot be identified by barcode in the standard pipelines. Exploiting the metrics from matcher, it was possible to assign reads to one of 14 categories; 1 for each of the 12 barcodes, unclassified if no barcode could be identified and un-called if the sequence had failed base calling entirely. To facilitate downstream analysis, we replicated the current ONT strategy for base called reads as much as possible. Reads are moved to a ``downloads`` folder and then further subfolders depending on the barcode assignment. This facilitates analysis with a variety of tools. As well as the physical movement of files, we have added an extra field of data to the fast5 file. Under the category of "minoTour_meta" we write the assigned barcode to the file. This will help with subsequent comparisons of our local pipeline with the 2D base calling metrichor pipelines. Oh yes - I forgot to mention - nanonet can call either template or complement sequence (but not yet 2D). For speed and simplicity the local pipeline just calls the template data. Finally we place a copy of the original file (no base calling and no barcoding assignment) in an 'uploads' folder in case of any unexpected problems with files downstream.

The reads written to the downloads folder can then be processed by minoTour running on the local machines to visualise barcoding and coverage. 

Here is a nice pie chart of barcodes as called by the nanonet pipeline: 

<img src="/images/blog/2016-06-07-barcode-matchin.png" />

We can also generate coverage plots for each barcode on the fly - note this specific experiment was looking at PCR conditions between different barcodes: 

<img src="/images/blog/2016-06-07-barcode-plot.png" />

At this point, with everything running nicely in the comfort of my office, code was shipped out to the guys on the bus. Here is the downside (along with missing the beaches, food and camaraderie) of being on the home team - the compute resource available on the bus revealed a flaw… The barcoding pipeline doubled the time of base calling. Less than ideal and, wth the luxury of running on a few more cores in the office, not something I had really bothered checking. A brief twitter exchange via DM with Nick and running a couple of quick tests on my laptop revealed the problem to be very real. The away team decided to decouple base calling and barcoding and move forwards.

This was now a challenge... we must be able to reduce the cost of barcoding and clearly the method was sub-optimal! Some frantic searching revealed the existence of a fast implementation of the Smith-Waterman algorithm using "<a href="https://github.com/mengyao/Complete-Striped-Smith-Waterman-Library">Single-Instruction Multiple-Data instructions to parallelize the algorithm at the instruction level</a>". All I knew was that it seemed fast, had a python interface and is available under the MIT license. Perfect. A  couple of hours later and some hacking, the previous emboss matcher algorithm was out and the fast SW was in. A test set of 23 reads on a single core could be base called in 13.5 seconds. The emboss matcher based barcoding increased this to 27.4 seconds (ouch - should have spotted that…). However, using the fast SW reads could be base called and demultiplexed in 14.2 seconds. Much better performance and that was emailed through to the team.

So - its getting there. All that is needed now is for team 4 to fix the internet, @scalene to do whatever everyone is waiting for and for the bus to continue safely and productively on its way! 



The minoTour_meta additions to the fast5 files are going to see some updates and improvements in the next released version of <a href="http://minotour.nottingham.ac.uk">minoTour</a>.

