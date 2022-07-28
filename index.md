---
page.title: "MetaWorks"
layout: default
---

<p align="center">
  <img src="./images/MetaWorksLogo.jpg" width="750"/>
</p><br>

<h2> Why MetaWorks? </h2>

<h5 class="text-info">Free and open-source software</h5>
<p>MetaWorks runs at the command-line on linux-64.  The pipeline strings together popular open-source free software tools to process demultiplexed Illumina paired-end reads such as SeqPrep (St. John, 2016), CutAdapt (Martin, 2011), VSEARCH (Rognes, Flouri, Nichols, Quince, & Mahé, 2016), and the RDP Classifier (Wang, Garrity, Tiedje, & Cole, 2007).</p>

<h5 class="text-info">Versioned workflows to improve reproducibility</h5>

<p>MetaWorks is versioned and available from <a href="https://github.com/terrimporter/MetaWorks">GitHub.</a></p>

<h5 class="text-info">Harmonized Conda processing environment</h5>

<p>MetaWorks comes with a conda environment file that should be activated before running the pipeline. Conda is an open-source environment and package manager (Anaconda, 2016). The environment file contains most of the programs and dependencies needed to run MetaWorks. If pseudogene filtering will be used, then the NCBI ORFfinder program will also need to be installed.  Additional RDP-trained reference sets may need to be downloaded if the reference set needed is not already built in to the RDP classifier.</p> 

<h5 class="text-info">Uses Snakemake for scalable processing</h5>

<p>Snakemake is a Python-based workflow manager (Koster and Rahmann, 2012) that strings together workflow-steps and distributes these jobs across a high performance computing platform to efficiently manage computational resources.  Interrupted jobs can be re-started following the last successful step.</p>

<h5 class="text-info">Generates either exact sequence variants and/or operational taxonomic units</h5>

<p>MetaWorks offers workflows for generating exact sequence variants (zero-radius OTUS) and/or operational taxonomic units using a 97% identity cutoff using VSEARCH.</p> 

<h5 class="text-info">Supports popular metabarcode markers</h5>

<p>MetaWorks was specifically developed to handle different types of metabarcodes from ribosomal RNA genes + spacers to protein coding genes.  Unique marker considerations, such as the removal of conserved rRNA genes from ITS sequences and putative pseudogenes from COI is supported.  Our integrated pseudogene-filtering approaches can be used when processing protein-coding metabarcodes has been <a href="https://link.springer.com/article/10.1186/s12859-021-04180-x">published.</a></p>

<h5 class="text-info">Developed to support projects that cut across taxon lines!</h5>  

<p>Our pipelines have been around, in one form or another, since before the terms metabarcoding and eDNA were coined.  As we know, ‘best practice’ is a moving target in this field.  MetaWorks is based on ‘best practices’ from the fields of microbial and fungal molecular ecology and strives to accommodate the needs of the animal metabarcode community.  We are driven by the need to make metabarcode bioinformatic processing both scalable and tractable within reasonable timeframes.  This pipeline is in active development to keep up with improvements in the underlying programs and reference sequence databases.</p> 

<p>MetaWorks has been used as a part of the <a href="https://stream-dna.com/">STREAM</a> and <a href="https://www.canada.ca/en/environment-climate-change/services/biodiversity/ecobiomics.html">EcoBiomics</a> projects to process multi-marker metabarcode datasets from freshwater benthos, water, and soil.</p> 

<h2>Citing Metaworks</h2>

<p>If you use this dataflow or any of the provided scripts, please cite the MetaWorks preprint:</p> 

<p>Porter, T.M., Hajibabaei, M. 2020. METAWORKS: A flexible, scalable bioinformatic pipeline for multi-marker biodiversity assessments. BioRxiv, doi: https://doi.org/10.1101/2020.07.14.202960.</p>

<p>If you use the pseudogene filtering methods, please cite the pseudogene publication:</p> 

<p>Porter, T.M., & Hajibabaei, M. (2021). Profile hidden Markov model sequence analysis can help remove putative pseudogenes from DNA barcoding and metabarcoding datasets. BMC Bioinformatics, 22: 256.</p>
