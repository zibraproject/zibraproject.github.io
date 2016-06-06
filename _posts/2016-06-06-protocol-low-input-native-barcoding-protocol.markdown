---
layout: post
title:  "Protocol: Low input native barcoding protocol"
categories: protocols
author: josh
---

As you may remember back in December we ran an Oxford Nanopore bootcamp called <a href="http://porecamp.github.io">Porecamp</a>. There, a team of instructors, including the mighty Matthew Loose, John Tyson, Justin O’Grady’s gang interacted with 28 eager students some all the way from down under. They came to Birmingham to participate in a course covering all areas of MinION sequencing from DNA QC through library preparation, sequencing and data analysis. Justin 'OG’ rady and I were notionally in charge of the library preparation component which was going to involve barcoding samples brought along by the students using a new native barcoding protocol from ONT. When I say new I mean the barcodes and compatible adapters arrived in hand labelled Eppendorf tubes along with a protocol in a Word document. Overall the student libraries were a huge success with essentially everyone generating some reads.

Why am I talking about Porecamp? Well, the biggest issue we had during the course was that the native barcoding protocol suggests you start with 500 ng of each sample i.e. (12 * 500 = 6 ug total) for the full set of barcodes. This seemed excessive given that you only actually end with pooled library the same concentration as you would have using the normal genomic DNA library protocol which only uses 1000 ng input material. The reason for this is that as some point you have to pool 12 samples of barcoded fragments into a single 100 ul ligation reaction which only has a 2x buffer (or 5x in the most recent version of the protocol) which therefore means you either have to elute your samples in 5 ul in the previous clean-up. This would be hard even for the most green fingered molecular biologist or start with sufficient material that you can elute in 20 ul and carry forward a quarter of the volume into the ligation reaction. 

To address this we have developed the following protocol for the ZiBRA project. This protocol requires only 100 ng per sample (i.e. 1/5th) which is useful because it reduces the number of quantification steps and SPRI cleanups. Note: native barcoded libraries are not cheap to make as they require 300 ul of Blunt/TA ligase master mix which is over £300 a tube and over 4 ml of SPRI beads. However I may attempt scaling down the ligation reaction volumes in future. The basic idea here is you quantify and normalise your input material at the start then carry the whole volume through each subsequent step rather than quantifying and repooling. Instead of performing individual clean ups on the barcoded fragments, you heat denature the ligase for 10 minutes at 65°C then pool then before performing a single SPRI clean up on the pool allowing you to elute all the DNA into the volume required for the ligation reaction, so no waste!

## Protocol

  - Dilute 100 ng pooled amplicons into 30 ul for each sample
  - Add 4.2 ul Ultra II buffer and 1.8 ul Ultra II enzyme mix (0.6x reaction size)
  - Incubate at 20°C for 5 minutes and 65°C for 5 minutes
  - Perform SPRI clean-up and elute in 22.5 ul
  - Continue without quantification

  - Perform native barcode ligation as per protocol

  - Heat denature ligase at 65°C for 10 minutes
  - Combine all samples giving a total of 600 ul
  - Perform 1x SPRI clean-up and elute in 58 ul

  - Complete adapter ligation and pull-down as per protocol
  - Quantify library add 80-100ng total into MinION flowcell

## Example output

Here is an example barcode proportion pie chart resulting from this protocol; the balancing
is nice and even (NB04 is a template negative control).

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Live barcoding visualisation courtesy of <a href="https://twitter.com/mattloose">@mattloose</a> and Minotour. Nb04 is neg control. <a href="https://t.co/yTacdr97pr">pic.twitter.com/yTacdr97pr</a></p>&mdash; Nick Loman (@zibraproject) <a href="https://twitter.com/zibraproject/status/739452897939841024">June 5, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


