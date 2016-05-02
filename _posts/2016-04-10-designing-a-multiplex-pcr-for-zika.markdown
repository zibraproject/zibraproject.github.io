---
layout: post
title:  "Designing a multiplex PCR scheme for Zika"
categories: lab
author: nick
---

We suspect Zika will be tricky to sequence because the amount of circulating virus
during acute infection seems to be quite low. Studies and anecdotes have demonstrated
that cycle threshold (Ct) values from RT-PCR may be around 30-35 at best, which
corresponds to a viral load of perhaps 10^4-10^5. 

Previous studies have attempted to get around this problem by passaging the virus
to amplify and enrich it. However, this is a laborious process that risks selecting
for ex vivo mutations, potentially confounding epidemiological analysis. Tissue
culture is also not very practical on the road.

During our work on Ebola we found that we could successfully amplify Cts of
up to 35 using amplicons of between 1 and 2kb long, but the chances of amplicons
dropping out seems to increase as Ct values rise, i.e. when viral titres fall.
This is presumably because of the small number of starting RNA molecules that size
(as the RNA will become fragmented during handling and during rough procedures like
RNA extraction).

When performing PCR, one way of improving sensitivity is to decrease the amplicon
size. In diagnostics amplicons as short as 80 bases may be targeted to improve
sensitivity, but at this length sequencing would be highly laborious given the
large number of reactions involved to tile across a genome (Zika is around 10
kilobases in length).

We plan to use nanopore sequencing which does not have a read length limit, which
is advantageous. 

However, we do not know until we get started on real clinical samples that the
realistic amplicon size floor will be.

Our plan is to go and test out the amplicon sizes on a range of clinical samples which
have Cts ranging from around 27 to 38 at the University of Sao Paolo on the week
of April 21st.

We think it should be possible to generate amplicons of around 250 - 500 base pairs
in size even from samples with a Ct of 35.

However, this raises its own problems. For Zika we want to sequence between 750-1000
strains within our sequencing consumable budget of £50,000 and to achieve this we
really need to be multiplexing 12 samples per flow cell (each nanopore flowcell
costs $500).

However, the best case would be 20 amplicons per genome for 500bp amplicons. This
would mean we are having to manage 240 reactions per run. That's a lot of pipetting,
but more importantly it's a worrying potential source of contamination.

So the next bit of work - after ensuring we can reliably amplify Zika at high
Ct values - is to combine reactions into a single tube multiplex.

Watch this space for updates!




