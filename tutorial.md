---
layout: page
title: Tutorial
permalink: /tutorial/
---

<h1>Tutorial</h1>
<br>
We have provided a small set of COI paired-end Illumina MiSeq files for this tutorial. These sequence files contain reads for several pooled COI amplicons, but here we will focus on the COI-BR5 amplicon (Hajibabaei et al., 2012, Gibson et al., 2014). Same as the quickstart above, but with additional instructions here if needed.

<h2>Sections:</h2>
<h5><a href="#step1">Prepare your environment for the pipeline</a></h5>
<h5><a href="#step2">Run MetaWorks using the COI testing data provided</a></h5>
<h5><a href="#step3">Analyze the output</a></h5>
<br>
<h5 class="text-info"><a id="step1">Prepare your environment for the pipeline.</a></h5>

Begin by downloading the latest MetaWorks release available at https://github.com/terrimporter/MetaWorks/releases/tag/v1.10.0 by using wget from the command line:
<pre><code># download the pipeline
wget https://github.com/terrimporter/MetaWorks/releases/download/v1.10.0/MetaWorks1.9.5.tar.gz

# unzip the pipeline
unzip MetaWorks1.9.5.zip
</code></pre>

If you don't already have conda on your system, then you will need to install it:

<pre><code># Download miniconda3
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Install miniconda3 and initialize
sh Miniconda3-latest-Linux-x86_64.sh

# Add conda to your PATH
# If you do not have a bin folder in your home directory, then create one first.
mkdir ~/bin

# Enter bin folder
cd ~/bin

# Create symbolic link to conda
ln -s ~/miniconda3/bin/conda conda
</code></pre>

Create then activate the MetaWorks_v1.10.0 environment:

<pre><code># Move into the MetaWorks folder
cd MetaWorks1.9.5

# Create the environment from the provided environment.yml file . Only need to do this step once.
conda env create -f environment.yml

# Activate the environment. Do this everytime before running the pipeline.
conda activate MetaWorks_v1.10.0
</code></pre>

To taxonomically assign COI metabarocodes, you will need to install the RDP-trained COI Classifier from <a href="https://github.com/terrimporter/CO1Classifier/releases/tag/v4" target="_blank">https://github.com/terrimporter/CO1Classifier/releases/tag/v4</a>. You can do this at the command line using wget.

<pre><code># download the COIv4 classifier
wget https://github.com/terrimporter/CO1Classifier/releases/download/v4/CO1v4_trained.tar.gz

# decompress the file
tar -xvzf CO1v4_trained.tar.gz

# Note the full path to the rRNAClassifier.properties file, ex. mydata_trained/rRNAClassifier.properties
</code></pre>

If you wish to filter out putative pseudogenes, if you do not already have the NCBI ORFfinder installed on your system, then download it from the NCBI at ftp://ftp.ncbi.nlm.nih.gov/genomes/TOOLS/ORFfinder/linux-i64/. You can download it using wget then make it executable in your path:

<pre><code># download
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/TOOLS/ORFfinder/linux-i64/ORFfinder.gz

# decompress
gunzip ORFfinder.gz

# make executable
chmod 755 ORFfinder

# If you do not have a bin folder in your home directory, then create one first.
mkdir ~/bin

# put in your PATH (ex. ~/bin).
mv ORFfinder ~/bin/.
</code></pre>

<h5 class="text-info"><a id="step2">Run MetaWorks using the COI testing data provided.</a></h5>

The config_testing_COI_data.yaml file has been 'preset' to work with the COI_data files in the testing folder. You will, however, still need to add the path to the trained COI classifier and save your changes.

<pre><code>RDP:
# If you are using a custom-trained reference set 
# enter the path to the trained RDP classifier rRNAClassifier.properties file here:
    t: "/path/to/CO1Classifier/v4/mydata_trained/rRNAClassifier.properties"
</code></pre>    
  
Then you should be ready to run the MetaWorks pipeline on the testing data.
    
<pre><code># You may need to edit the number of jobs you would like to run, ex --jobs 1 or --jobs 4, according to how many cores you have available
snakemake --jobs 2 --snakefile snakefile --configfile config_testing_COI_data.yaml
</code></pre>

<h5 class="text-info"><a id="step3">Analyze the output.</a></h5>

The final output file is called results.csv . The results are for the COI-BR5 amplicon. This can be imported into R for bootstrap support filtering, pivot table creation, normalization, vegan analysis, etc. There are also a number of other output files in the stats directory showing the total number of reads processed at each step as well as the sequence lengths. Log files are also available for the dereplication, denoising, and chimera removal steps.

If you are done with MetaWorks, deactivate the conda environment:

<pre><code>conda deactivate</code></pre>

<a href="#top">Return to the top.</a>