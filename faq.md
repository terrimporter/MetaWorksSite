---
layout: page
title: FAQ
permalink: /faq/
---

<h2>Frequently Asked Questions</h2>
<br>
<div class="accordion" id="accordionFAQ">
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingOne">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseOne" aria-expanded="false" aria-controls="collapseOne">
        <h5 class="text-info">What is conda?  Why do I need it?</h5>
      </button>
    </h2>
    <div id="collapseOne" class="accordion-collapse collapse" aria-labelledby="headingOne" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>Conda is an open source package and environment management system.  Miniconda is a lightweight version of conda that only contains conda, python, and their dependencies.  Using conda and the environment.yml file provided here can help get most of the necessary programs in one place to run this pipeline.  If you install miniconda and activate the conda environment provided, then you will also get the correct versions of the open source programs used in this pipeline including Snakemake.</p>
<p>Install miniconda as follows:</p>
<pre><code># Download miniconda3
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Install miniconda3 and initialize
sh Miniconda3-latest-Linux-x86_64.sh

# Add conda to your PATH, ex. to ~/bin
cd ~/bin
ln -s ~/miniconda3/bin/conda conda

# Activate conda method 1 (working in a container)
source ~/miniconda3/bin/activate MetaWorks_v1.10.0

# Activate conda method 2
conda activate MetaWorks_v1.10.0
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingTwo">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
        <h5 class="text-info">How do I check which program versions the pipeline is using?</h5>
      </button>
    </h2>
    <div id="collapseTwo" class="accordion-collapse collapse" aria-labelledby="headingTwo" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>Ensure the program versions in the environment are being used.</p>

<pre><code># Create conda environment from file.  Only need to do this once.
conda env create -f environment.yml

# activate the environment
conda activate MetaWorks_v1.10.0

# list all programs available in the environment at once
conda list > programs.list

# you can check that key programs in the conda environment are being used (not locally installed versions)
which SeqPrep
which cutadapt
which vsearch
which perl

# you can also check their version numbers one at a time instead of running 'conda list'
cutadapt --version
vsearch --version
</code></pre>

<p>Version numbers are also tracked in the snakefile.</p>
      </div>
    </div>
  </div>
    <div class="accordion-item">
    <h2 class="accordion-header" id="headingThree">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseThree" aria-expanded="false" aria-controls="collapseThree">
        <h5 class="text-info">How do I fix a GLIBC error when trying to run ORFfinder?</h5>
      </button>
    </h2>
    <div id="collapseThree" class="accordion-collapse collapse" aria-labelledby="headingThree" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>If you have an older version of GLIBC (on Centos6), then you may be missing the libc.so.6 file that ORffinder needs.  This file is already available, but you need to create some links yourself.</p>

<p>Create a symbolic link to the library:</p>

<pre><code>cd ~/miniconda3/envs/MetaWorks_v1.10.0/lib
ln -s ../glibc-2.14/lib/libc.so.6 libc.so.6
</code></pre>

<p>Create the shell script file LD_PATH.sh in the following location to set the environment variable: ~/miniconda3/envs/MetaWorks_v1.10.0/etc/conda/activate.d/LD_PATH.sh</p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH_CONDA_BACKUP=$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH
</code></pre>

<p>Create the file LD_PATH.sh in the following location to unset the environment variable:  
~/miniconda3/envs/MetaWorks_v1.10.0/etc/conda/deactivate.d/LD_PATH.sh </p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH=$LD_LIBRARY_PATH_CONDA_BACKUP
</code></pre>

<p>Deactivate then reactivate the environment.</p>

<p>Test ORFfinder:</p>

<pre><code>ORFfinder
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingFour">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseFour" aria-expanded="false" aria-controls="collapseFour">
        <h5 class="text-info">How do I fix a libnghttp2 error when trying to run ORFfinder?</h5>
      </button>
    </h2>
    <div id="collapseFour" class="accordion-collapse collapse" aria-labelledby="headingFour" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>The missing library (on Centos7), is already available, only the LD_LIBRARY_PATH needs to be updated as follows:</p>

<p>Create the shell script file LD_PATH.sh in the following location to set the environment variable: ~/miniconda3/envs/MetaWorks_v1.10.0/etc/conda/activate.d/LD_PATH.sh</p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH_CONDA_BACKUP=$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH
</code></pre>

<p>Create the file LD_PATH.sh in the following location to unset the environment variable:  
~/miniconda3/envs/MetaWorks_v1.10.0/etc/conda/deactivate.d/LD_PATH.sh</p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH=$LD_LIBRARY_PATH_CONDA_BACKUP
</code></pre>

<p>Deactivate then reactivate the environment.</p>

<p>Test ORFfinder:</p>

<pre><code>ORFfinder
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingFive">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseFive" aria-expanded="false" aria-controls="collapseFive">
        <h5 class="text-info">Where are the HMMs I need to run pseudogene removal method 2 on arthropod COI sequences?</h5>
      </button>
    </h2>
    <div id="collapseFive" class="accordion-collapse collapse" aria-labelledby="headingFive" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>Ensure that bold.hmm as well as the indexed files (bold.hmm.h3p, bold.hmm.h3m, bold.hmm.h3i, and bold.hmm.h3f) are available in the same directory as you snakefile.</p>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingSix">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseSix" aria-expanded="false" aria-controls="collapseSix">
        <h5 class="text-info">My files used different filename conventions. How can I rename batches of files in one go?</h5>
      </button>
    </h2>
    <div id="collapseSix" class="accordion-collapse collapse" aria-labelledby="headingSix" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>Sometimes it necessary to rename raw data files in batches.  I use Perl-rename (Gergely, 2018) that is available at <a href="https://github.com/subogero/rename" target="_blank">https://github.com/subogero/rename</a> not linux rename.  I prefer the Perl implementation so that you can easily use regular expressions.  I first run the command with the -n flag so you can review the changes without making any actual changes.  If you're happy with the results, re-run without the -n flag.</p>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingSeven">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseSeven" aria-expanded="false" aria-controls="collapseSeven">
        <h5 class="text-info">I don't want multiple copies of raw sequence data on my system, can I just link to files saved elsewhere?</h5>
      </button>
    </h2>
    <div id="collapseSeven" class="accordion-collapse collapse" aria-labelledby="headingSeven" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>Symbolic links are like shortcuts or aliases that can also be placed in your ~/bin directory that point to files or programs that reside elsewhere on your system.  So long as those scripts are executable (e.x. chmod 755 script.plx) then the shortcut will also be executable without having to type out the complete path or copy and pasting the script into the current directory.</p>

<pre><code>ln -s /path/to/target/directory shortcutName
ln -s /path/to/target/directory fileName
ln -s /path/to/script/script.sh commandName
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingEight">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseEight" aria-expanded="false" aria-controls="collapseEight">
        <h5 class="text-info">My connection isn't stable, what can I do to prevent errors caused by a lost connection?</h5>
      </button>
    </h2>
    <div id="collapseEight" class="accordion-collapse collapse" aria-labelledby="headingEight" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>Large jobs with many fastq.gz files can take a while to run.  To prevent unexpected network connection interruptions from stopping the MetaWorks pipeline, set the job up to keep running even if you disconnect from your session:</p>

<p>1. You can use nohup (no hangup).</p>

<pre><code>nohup snakemake --jobs 24 --snakefile snakefile --configfile config.yaml
</code></pre>

<p>2. You can use screen.</p>

<pre><code># to start a screen session
screen
ctrl+a+c
conda activate MetaWorks_v1.10.0
snakemake --jobs 24 --snakefile snakefile --configfile config.yaml
ctrl+a+d

# to list all screen session ids, look at top of the list
screen -ls

# to re-enter a screen session to watch job progress or view error messages
screen -r session_id
</code></pre>

<p>3. You can submit your job to the queuing system if you use one.</p>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingNine">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseNine" aria-expanded="false" aria-controls="collapseNine">
        <h5 class="text-info">How can I extract an ESV table from the results.csv</h5>
      </button>
    </h2>
    <div id="collapseNine" class="accordion-collapse collapse" aria-labelledby="headingNine" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>The results.csv file is the final file in the MetaWorks pipeline.  An ESV table (also known as an OTU table or pivot table) can be created in Excel or R.  In R, changing the results.csv file from a wider to a longer tabular format can be done using different libraries.</p>

<pre><code># R using reshape2 library
ESVtable <- reshape2::dcast(df, SampleName ~ GlobalESV, value.var = "ESVsize", fun.aggregate = sum)
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingTen">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseTen" aria-expanded="false" aria-controls="collapseTen">
        <h5 class="text-info">How do I filter out pseudogenes for taxa with different genetic codes?</h5>
      </button>
    </h2>
    <div id="collapseTen" class="accordion-collapse collapse" aria-labelledby="headingTen" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>If you are targeting a broad group, such as Metazoa using COI primers, you can still filter out pseudogenes using removal method 1 that uses ORFfinder.  This can be done in two steps, for example by first processing invertebrate phyla, then processing phylum chordata that includes the vertebrata clade (see NCBI taxonomy).  Note that pseudogene removal method 2 that uses HMMer is currently only available for COI arthropoda at this time.<p>

<p>1. Edit the config_ESV.yaml file as follows:  
Set the taxonomy filter to '-e Annelida -e Arthropoda -e Bryozoa -e Cnidaria -e Echinodermata -e Mollusca -e Nematoda -e Nematomorpha -e Nemertea -e Platyhelminthes -e Porifera' to target invertebrate animal phyla  
Set pseudogene removal method 1 (only uses ORFfinder)  
Set genetic code to '5' to use the invertebrate mitochondrial genetic code for translation  
Run snakemake.  Move invertebrate outfiles into their own directory so they do not get over-written: taxon.zotus, chimera.denoised.nonchimeras.taxon, orf.fasta*, rdp.csv.tmp, results.csv.</p>

<p>2. Edit the config_ESV.yaml file as follows:  
Set the taxonomy filter to '-e Chordata' to target animals with a notochord (includes the Vertebrata clade) (see NCBI taxonomy)  
Keep pseudogene removal method 1  
Set genetic code to '2' to use the vertebrate mitochondrial genetic code for translation  
Run snakemake.  Move chordata outfiles into their own directory so they do not get over-written: taxon.zotus, chimera.denoised.nonchimeras.taxon, orf.fasta*, rdp.csv.tmp, results.csv .</p> 

<p>The invertebrate and chordata results.csv files can then be combined prior to downstream processing.</p>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="headingLast">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#collapseLast" aria-expanded="false" aria-controls="collapseLast">
        <h5 class="text-info">The final output file wasn't created, what happened?</h5>
      </button>
    </h2>
    <div id="collapseLast" class="accordion-collapse collapse" aria-labelledby="headingLast" data-parent="#accordionFAQ">
      <div class="accordion-body">
<p>If you get a warning and see that the last file created was rdp.csv.tmp but not the expected results.csv then you probably are probably trying to process a very large number of sequence files (thousands) and the job ran into memory problems preventing the creation of the final results.csv file.  You have a few options here:</p>
<p>a) Re-run the pipeline where there is more memory available so that the final outfile can be created.  On the GPSC, you will need to create an interactive container, activate the conda environbment, then unlock the snakemake directory first before submitting another job with more memory on a large compute node.</p>
<p>b) Stop here.  You can still use the ESV.table file together with the rdp.csv.tmp file for further processing in R.  Don't forget to filter out sequence clusters with only 1 or 2 reads from the ESV.table (a step that was done automatically by MetaWorks).  For each sequence cluster in the ESV.table, you can grab the taxonomy from the rdp.csv.tmp file using the ZotuID (sequence cluster ID).</p>
<p>There is a new MetaWorks workflow being created that handles analyses with very large number of samples more efficiently (in progress).</p>
      </div>
    </div>
  </div>
</div>
