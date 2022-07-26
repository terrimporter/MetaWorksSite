---
page.title: "MetaWorks"
layout: default
---

<h1> Why MetaWorks? </h1>

<br>MetaWorks comes with a conda environment file MetaWorks_v1.10.0 that should be activated before running the pipeline.  Conda is an environment and package manager (Anaconda, 2016).  The environment file contains most of the programs and dependencies needed to run MetaWorks.  If pseudogene filtering will be used, then the NCBI ORFfinder program will also need to be installed.  Additional RDP-trained reference sets may need to be downloaded if the reference set needed is not already built in to the RDP classifier (see Table 1 below).

Snakemake is a python-based workflow manager (Koster and Rahmann, 2012) and it requires three sets of files to run the any one of the workflows described described in the next section (Fig 1).

**Fig 1.  Using a conda environment helps to quickly gather programs and dependencies used by MetaWorks.**.

<img src="/images/conda_env.png" width="500">

The configuration file is edited by the user to specify directory names, indicate the sample and read fields from the sequence filenames, and specify other required pipeline parameters such as primer sequences, marker name, and whether or not pseudogene filtering should be run.

The snakefile describes the pipeline itself and normally does not need to be edited in any way (Fig 2).  

**Fig 2. The snakefile describes the programs used, the settings, and the order in which to run commands.**<br>
The pipeline takes care of any reformatting needed when moving from one step to another.  In addition to the final results file, a summary of statistics and log files are also available for major steps in the pipeline.

<img src="/images/dataflow.png" width="500">


The pipeline begins with raw paired-end Illumina MiSeq fastq.gz files.  Reads are paired.  Primers are trimmed.  All the samples are pooled for a global analysis.  Reads are dereplicated, denoised, and chimeric sequences are removed producing a reference set of denoised exact sequence variants (ESVs). At this step, the pipeline diverges into several paths:  an ITS specific dataflow, a regular dataflow, and a pseudogene filtering dataflow.  For ITS sequences, flanking rRNA gene regions are removed then they are taxonomically assigned.  For the regular pipeline, the denoised ESVs are taxonomically assigned using the RDP classifier.  If a protein coding marker is being processed you have the option to filter out putative pseudogenes (Porter and Hajibabaei, 2021).  The result is a report containing a list of ESVs for each sample, with read counts, and taxonomic assignments with a measure of bootstrap support (Fig 3).

**Fig 3. The RDP classifier produces a measure of confidence for taxonomic assignments at each rank.**  
Results can be filtered by bootstrap support values to reduce false-positive assignments.  The appropriate cutoffs to use for filtering will depend on the marker/classifier used, query length, and taxonomic rank.  See links in Table 1 for classifier-specific cutoffs to ensure 95-99% accuracy.

<img src="/images/taxonomic_assignments.png" width="500">