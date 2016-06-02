---
layout: post
title:  "Tidying up run folders with poretools organise"
categories: protocols
author: nick
---

On the flight to Natal I started thinking about the process of handling the sequencing
data files we will (hopefully!) be generating on this trip. There's something about starting new projects that gives you the incentive to avoid all the problems of the last one, isn't there?

And given that I'm about as useful in the lab as a chocolate teapot it's also a chance to
write some scripts that will be useful for the folks running the sequencing.

One thing I've been meaning to do for literally two years now is to add a function
to <a href="http://poretools.readthedocs.org">poretools</a> that helps organise the
files that come off MinKNOW during a sequencing run. Even a short Zika sequencing run
on R9 can generate 100k-500k reads as our amplicon scheme is quite short.

Managing these files has traditionally been quite tedious: by default the software
dumps everything indiscriminately into ``c:\data\reads``.
This means if you start a couple of runs without clearing the folder in-between
you end up with a huge directory of files from different runs.

This is a real pain and is also potentially a problem if you don't realise this has
happened - you could potentially analyse a mixture of runs by mistake with
disastrous consequences.

Enter ``poretools organise``.

This simple poretools command takes a directory of FAST5 files (it is intended to operate on
prebasecalled files, but it would work on post-basecalled files too).

For each file it will strip out important metadata:

  * ``hostname`` - the name of the laptop or PC that processed this read 
  * ``asic_id`` - the ASIC ID on the flowcell being used
  * ``run_id`` - the randomly generated run ID that gets made each time MinKNOW is started
  * ``exp_start_time`` and ``start_time`` - the start time of the run and the
    time of the read

From this information it will make a new file path for the read in the format:

  ``hostname/asic_id/run_id/hour``

And either copy or move the file depending on if you specify the ``--copy`` argument.

So, an intended usage would be:

  ``poretools c:\data\reads c:\tidydir``

> A word of warning on the ASIC ID: although the reads written by MinKNOW
> have a placeholder for ``flowcell_id``, this field I think is always
> null. The ASIC ID can be used as a proxy for the flowcell_id, but with 
> a caveat-- flowcells get recycled > via ONT, so there is a non-zero chance
> that you could see the same asic_id twice on different shipments of flowcells!

A frustrating finding is that I cannot see an easy way of getting the actual
user-supplied sample information from the FAST5 files other than through
parsing of the filename, which I wanted to avoid. This does mean that the directory
names are quite ugly and unintuitive right now.

When I get a bit more time the next feature to add would be to automatically ``tar``
the run up by hour (or configurable length) chunks.

It would also be nice to make the format of the directory names configurable.

Then the plan would be to ``rsync`` these folders to a backup hard drive and online.
``rsync`` will likely perform better with larger file chunks.

``poretools organise`` is in the ``master`` branch of the
<a href="http://github.com/arq5x/poretools">Github repository</a>
but not yet in an official release.

As always thanks to Aaron Quinlan for being the brains behind poretools and
for tidying up any messes I leave behind!

And of course, pull requests always welcome, particularly if folks can think
of a way of prettying up that directory tree from the information contained in
the FAST5 metadata.















