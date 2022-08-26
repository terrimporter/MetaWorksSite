---
layout: page
title: FAQ
permalink: /faq/
---

<h1>Frequently Asked Questions</h1>
<br>
<h2>Metaworks FAQs</h2>
<div class="accordion" id="metaworksFAQ">
  <div class="accordion-item">
    <h2 class="accordion-header" id="metaHeadingOne">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseOne" aria-expanded="false" aria-controls="metaCollapseOne">
        <h5 class="text-info">What is conda?  Why do I need it?</h5>
      </button>
    </h2>
    <div id="metaCollapseOne" class="accordion-collapse collapse" aria-labelledby="metaHeadingOne" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>Conda is an open source package and environment management system. Miniconda is a lightweight version of conda that only contains conda, python, and their dependencies. Using conda and the environment.yml file provided here can help get most of the necessary programs in one place to run this pipeline. If you install miniconda and activate the conda environment provided, then you will also get the correct versions of the open source programs used in this pipeline including Snakemake.</p>
<p>Install miniconda as follows:</p>
<pre><code># Download miniconda3
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Install miniconda3 and initialize
sh Miniconda3-latest-Linux-x86_64.sh

# Add conda to your PATH, ex. to ~/bin
cd ~/bin
ln -s ~/miniconda3/bin/conda conda

# Activate conda method 1 (working in a container)
source ~/miniconda3/bin/activate MetaWorks_v1.11.1

# Activate conda method 2
conda activate MetaWorks_v1.11.1
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="metaHeadingTwo">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseTwo" aria-expanded="false" aria-controls="metaCollapseTwo">
        <h5 class="text-info">How do I check which program versions the pipeline is using?</h5>
      </button>
    </h2>
    <div id="metaCollapseTwo" class="accordion-collapse collapse" aria-labelledby="metaHeadingTwo" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>Ensure the program versions in the environment are being used.</p>

<pre><code># Create conda environment from file. Only need to do this once.
conda env create -f environment.yml

# activate the environment
conda activate MetaWorks_v1.11.1

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
    <h2 class="accordion-header" id="metaHeadingThree">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseThree" aria-expanded="false" aria-controls="metaCollapseThree">
        <h5 class="text-info">How do I fix a GLIBC error when trying to run ORFfinder?</h5>
      </button>
    </h2>
    <div id="metaCollapseThree" class="accordion-collapse collapse" aria-labelledby="metaHeadingThree" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>If you have an older version of GLIBC (on Centos6), then you may be missing the libc.so.6 file that ORffinder needs. Ideally, you would contact your system administrator to make a newer version of GLIBC available for ORFfinder.  Not ideal, but in a pinch you can use conda to install a slightly newer version of GLIBC that will work and create some links so ORF finder knows where to find it.  I've tested this solution and it works, but it does affect the versions of CUTADAPT and VSEARCH compatible with this GLIBC version in your conda environment (use 'conda list' to check program versions).</p>
        
<p>Use conda to install a slighly newer version of GLIBC</p>
        
<pre><code>conda install -c pwwang glibc214</code></pre>

<p>Create a symbolic link to the library:</p>

<pre><code>cd ~/miniconda3/envs/MetaWorks_v1.11.1/lib
ln -s ../glibc-2.14/lib/libc.so.6 libc.so.6
</code></pre>

<p>Create the shell script file LD_PATH.sh in the following location to set the environment variable: ~/miniconda3/envs/MetaWorks_v1.11.1/etc/conda/activate.d/LD_PATH.sh</p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH_CONDA_BACKUP=$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH
</code></pre>

<p>Create the file LD_PATH.sh in the following location to unset the environment variable:  
~/miniconda3/envs/MetaWorks_v1.11.1/etc/conda/deactivate.d/LD_PATH.sh </p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH=$LD_LIBRARY_PATH_CONDA_BACKUP
</code></pre>

<p>Deactivate then reactivate the environment.</p>

<p>Test ORFfinder:</p>

<pre><code>ORFfinder</code></pre>
              </div>
            </div>
          </div>
          <div class="accordion-item">
            <h2 class="accordion-header" id="metaHeadingFour">
              <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseFour" aria-expanded="false" aria-controls="metaCollapseFour">
                <h5 class="text-info">How do I fix a libnghttp2 error when trying to run ORFfinder?</h5>
              </button>
            </h2>
            <div id="metaCollapseFour" class="accordion-collapse collapse" aria-labelledby="metaHeadingFour" data-parent="#metaworksFAQ">
              <div class="accordion-body">
<p>The missing library (on Centos7) can be installed using conda and the LD_LIBRARY_PATH needs to be updated as follows:</p>
 
<p>Install libnghttp2 using conda</p>
<pre><code>conda install -c conda-forge libnghttp2</code></pre>

<p>Create the shell script file LD_PATH.sh in the following location to set the environment variable: ~/miniconda3/envs/MetaWorks_v1.11.1/etc/conda/activate.d/LD_PATH.sh</p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH_CONDA_BACKUP=$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH
</code></pre>

<p>Create the file LD_PATH.sh in the following location to unset the environment variable:  
~/miniconda3/envs/MetaWorks_v1.11.1/etc/conda/deactivate.d/LD_PATH.sh</p>

<p>Put the following text in the LD_PATH.sh file:</p>

<pre><code>export LD_LIBRARY_PATH=$LD_LIBRARY_PATH_CONDA_BACKUP
</code></pre>

<p>Deactivate then reactivate the environment.</p>

<p>Test ORFfinder:</p>

<pre><code>ORFfinder</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="metaHeadingFive">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseFive" aria-expanded="false" aria-controls="metaCollapseFive">
        <h5 class="text-info">Where are the HMMs I need to run pseudogene removal method 2 on arthropod COI sequences?</h5>
      </button>
    </h2>
    <div id="metaCollapseFive" class="accordion-collapse collapse" aria-labelledby="metaHeadingFive" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>Ensure that bold.hmm as well as the indexed files (bold.hmm.h3p, bold.hmm.h3m, bold.hmm.h3i, and bold.hmm.h3f) are available in the same directory as you snakefile.</p>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="metaHeadingSix">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseSix" aria-expanded="false" aria-controls="metaCollapseSix">
        <h5 class="text-info">My files used different filename conventions. How can I rename batches of files in one go?</h5>
      </button>
    </h2>
    <div id="metaCollapseSix" class="accordion-collapse collapse" aria-labelledby="metaHeadingSix" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>Sometimes it necessary to rename raw data files in batches. I use Perl-rename (Gergely, 2018) that is available at <a href="https://github.com/subogero/rename" target="_blank">https://github.com/subogero/rename</a> not linux rename. I prefer the Perl implementation so that you can easily use regular expressions. I first run the command with the -n flag so you can review the changes without making any actual changes. If you're happy with the results, re-run without the -n flag.</p>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="metaHeadingSeven">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseSeven" aria-expanded="false" aria-controls="metaCollapseSeven">
        <h5 class="text-info">I don't want multiple copies of raw sequence data on my system, can I just link to files saved elsewhere?</h5>
      </button>
    </h2>
    <div id="metaCollapseSeven" class="accordion-collapse collapse" aria-labelledby="metaHeadingSeven" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>Symbolic links are like shortcuts or aliases that can also be placed in your ~/bin directory that point to files or programs that reside elsewhere on your system. So long as those scripts are executable (e.x. chmod 755 script.plx) then the shortcut will also be executable without having to type out the complete path or copy and pasting the script into the current directory.</p>

<pre><code>ln -s /path/to/target/directory shortcutName
ln -s /path/to/target/directory fileName
ln -s /path/to/script/script.sh commandName
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="metaHeadingEight">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseEight" aria-expanded="false" aria-controls="metaCollapseEight">
        <h5 class="text-info">My connection isn't stable, what can I do to prevent errors caused by a lost connection?</h5>
      </button>
    </h2>
    <div id="metaCollapseEight" class="accordion-collapse collapse" aria-labelledby="metaHeadingEight" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>Large jobs with many fastq.gz files can take a while to run. To prevent unexpected network connection interruptions from stopping the MetaWorks pipeline, set the job up to keep running even if you disconnect from your session:</p>

<p>1. You can use nohup (no hangup).</p>

<pre><code>nohup snakemake --jobs 24 --snakefile snakefile --configfile config.yaml
</code></pre>

<p>2. You can use screen.</p>

<pre><code># to start a screen session
screen
ctrl+a+c
conda activate MetaWorks_v1.11.1
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
    <h2 class="accordion-header" id="metaHeadingNine">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseNine" aria-expanded="false" aria-controls="metaCollapseNine">
        <h5 class="text-info">How do I filter out pseudogenes for taxa with different genetic codes?</h5>
      </button>
    </h2>
    <div id="metaCollapseNine" class="accordion-collapse collapse" aria-labelledby="metaHeadingNine" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>If you are targeting a broad group, such as Metazoa using COI primers, you can still filter out pseudogenes using removal method 1 that uses ORFfinder. This needs to be done in two steps because vertebrates and invertebrates each have different mitochondrial genetic codes.</p>

<p>1. Edit the config_ESV.yaml file as follows to process vertebrates first:</p>   

<p>Set the taxon filter to '-e Chordata' to target vertebrate animal phyla.</p>  
<p>Set pseudogene removal method 1 (only uses ORFfinder).</p>
<p>Set genetic code to '2' to use the vertebrate mitochondrial genetic code for translation.</p>
<p>Run snakemake.</p>
<p>When done, create a new directory called chordata 'mkdir vertebrates'.</p> 
<p>Use 'ls -lhrt' to list files.  Move each file AFTER cat.denoised.nonchimeras into a the vertebrates directory so they do not get over-written in the next step: table.log, taxon.zotus, chimera.denoised.nonchimeras.taxon, orf.fasta.nt, longest.orfs.fasta, taxonomy.csv, ESV.table, results.csv.</p>

<p>2. Edit the config_ESV.yaml file to process invertebrates next:</p>
<p>Set the taxon filter to '-e Metazoa rdp.out.tmp | grep -v Chordata' to process all metazoan taxa, excluding Chordata (already processed above).</p>  
<p>Keep pseudogene removal method 1.</p>
<p>Set genetic code to '5' to use the invertebrate mitochondrial genetic code for translation.</p>
<p>Run snakemake.</p>
<p>When done, create a new directory called invertebrates 'mkdir invertebrates'.</p> 
<p>Use 'ls -lhrt' to list files.  Move each file AFTER cat.denoised.nonchimeras into the invertebrates directory: table.log, taxon.zotus, chimera.denoised.nonchimeras.taxon, orf.fasta.nt, longest.orfs.fasta, taxonomy.csv, ESV.table, results.csv.</p>

<p>The vertebrates and invertebrates results.csv files (or the component taxonomy.csv and ESV.table files) can then be combined in R during data analysis using 'rbind'.</p>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="metaHeadingTen">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#metaCollapseTen" aria-expanded="false" aria-controls="metaCollapseTen">
        <h5 class="text-info">The final output file wasn't created, what happened?</h5>
      </button>
    </h2>
    <div id="metaCollapseTen" class="accordion-collapse collapse" aria-labelledby="metaHeadingTen" data-parent="#metaworksFAQ">
      <div class="accordion-body">
<p>If you get a warning and see that the last file created was rdp.csv.tmp but not the expected results.csv then you probably are probably trying to process a very large number of sequence files (thousands) and the job ran into memory problems preventing the creation of the final results.csv file. You have a few options here:</p>
<p>a) Re-run the pipeline where there is more memory available so that the final outfile can be created. On the GPSC, you will need to create an interactive container, activate the conda environbment, then unlock the snakemake directory first before submitting another job with more memory on a large compute node.</p>
<p>b) Stop here. You can still use the ESV.table file together with the rdp.csv.tmp file for further processing in R. Don't forget to filter out sequence clusters with only 1 or 2 reads from the ESV.table (a step that was done automatically by MetaWorks). For each sequence cluster in the ESV.table, you can grab the taxonomy from the rdp.csv.tmp file using the ZotuID (sequence cluster ID).</p>
<p>There is a new MetaWorks workflow being created that handles analyses with very large number of samples more efficiently (in progress).</p>
      </div>
    </div>
  </div>
</div>
<br>
<h2>Data Analysis FAQs</h2>
<div class="accordion" id="dataAnalysisFAQ">
  <div class="accordion-item">
    <h2 class="accordion-header" id="dataHeadingOne">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#dataCollapseOne" aria-expanded="false" aria-controls="dataCollapseOne">
        <h5 class="text-info">How can I extract an ESV table from the results.csv?</h5>
      </button>
    </h2>
    <div id="dataCollapseOne" class="accordion-collapse collapse" aria-labelledby="dataHeadingOne" data-parent="#dataAnalysisFAQ">
      <div class="accordion-body">
<p>The results.csv file is the final file in the MetaWorks pipeline. An ESV table (also known as an OTU table or pivot table) can be created in Excel or R. In R, changing the results.csv file from a wider to a longer tabular format can be done using different libraries.</p>

<pre><code># R using reshape2 library
ESVtable <- reshape2::dcast(df, SampleName ~ GlobalESV, value.var = "ESVsize", fun.aggregate = sum)
</code></pre>
      </div>
    </div>
  </div>
  <div class="accordion-item">
    <h2 class="accordion-header" id="dataHeadingTwo">
      <button class="accordion-button collapsed" type="button" data-toggle="collapse" data-target="#dataCollapseTwo" aria-expanded="false" aria-controls="dataCollapseTwo">
        <h5 class="text-info">How do I filter my results to minimize false positive taxonomic assignments?</h5>
      </button>
    </h2>
    <div id="dataCollapseTwo" class="accordion-collapse collapse" aria-labelledby="dataHeadingTwo" data-parent="#dataAnalysisFAQ">
      <div class="accordion-body">
<p>First, know that each custom-trained classifier has been screened for putative bacterial or human contaminants. These mislabelled records have been reported to GenBank and removed from the reference set. This data curation, done for you, should help remove false-positive assignments to putative contaminants that were labelled as something else.</p>

<p>Second, if you are working with a reference set that is built-in to the RDP classifier (16S prokaryote, fungal ITS, fungal LSU), use the cutoffs recommended by them, usually 0.8 unless your query sequence is < 250 bp long then use a 0.5 bootstrap support cutoff. Note that the expected accuracy as a result of these cutoffs will vary across each reference set. See the <a href="http://rdp.cme.msu.edu/classifier/classifier.jsp"  target="_blank">online documentation</a> to get an idea of the expected range of accuracy. <em>Note that this all assumes that all your query sequences are represented in the reference set. This assumption may not be valid for species from understudied regions of the world or poorly studied/cryptic/highly diverse species groups.</em>
</p>

<p>If you are working with one of our custom-trained classifiers, you can specifically filter your results for different levels of expected accuracy (80-99%). If you choose a higher expected accuracy, fewer of your assignments will pass bootstrap support thresholds. The lower the expected accuracy you can tolerate, the more assignments will pass bootstrap thresholds. On each github classifier page (see list of custom classifiers available on the <a href="../#classifier_table">main page</a>), you will find bootstrap support cutoffs to ensure a certain level of expected accuracy that is unique for each classifier, approximate query length, and taxonomic rank.</p>

<p>The best way to filter your results is when you are doing your data analysis, for example, in R. The example below removes assignments ("") that do not meet the bootstrap proportion cutoffs recommended for a particular level of taxonomic assignment accuracy. Another option would be to recode low confidence assignments to "Unknown".</p>

<pre><code>#read in MetaWorks results
COI <- read.csv("results.csv", header=TRUE, stringsAsFactors=FALSE)

# filter COI taxonomic assignments for 95% correct species, 99% correct genus & up
# See <a href="https://github.com/terrimporter/CO1Classifier" target="_blank">https://github.com/terrimporter/CO1Classifier</a> for cutoffs
COI$Species <- ifelse(COI$sBP >= 0.7, COI$Species, "")
COI$Genus <- ifelse(COI$gBP >= 0.3, COI$Genus, "")
COI$Family <- ifelse(COI$fBP >= 0.2, COI$Family, "")
</code></pre>
      </div>
    </div>
  </div>
</div>
