# Bioinformatics 2021 Class Project
## Laurel Nave-Powers 

### Data
The data used in this project are raw reads of flatfish exon capture data from Calder Atta. I am using two species: Hippoglossoides platessoides and Microstomus pacificus. The end goal of the project is to be able to look at SNP variants for these samples.

### Step 1 - Unzipping
I started with raw data in my raw data folder `../data/raw`.
These are compressed fastq files from exon capture, one file for forward (R1) and one for reverse (R2) for each species. To take a look at them, they need to be unzipped (see 01_unzip.ipynb). For these samples, I don't need to merge lanes because they were only run once.

### Step 2 - Trimming
The next step is to trim adaptors and low quality bases (see 02_trim.ipynb). This step is modified from Calder Atta (https://github.com/calderatta/ca-exon-capture). To use the perl script trim_adaptor.pl the input files must be in .fq NOT .fastq. Once changed, the .fq files are moved into their own folder `../data/demultiplexed`. 
Before running the perl script, check the options and make sure it works by using `trim_adaptor.pl -h`.
In order for the script to work, there are 6 perl modules and two packages to be downloaded into the @INC. 

The modules can be downloaded from the CPAN website (https://metacpan.org) and moved into the folder indicated in the @INC. Modules used:  Moo, Parallel::ForkManager, Sub::Defer, Sub::Quote, Method::Generate::Constructor, Role::Tiny. 

The packages can be downloaded from their respective websites and put in the same folder as the modules.
Trim_galore is an easy download (https://github.com/FelixKrueger/TrimGalore). Cutadapt is more complicated and requires you to download via conda (https://github.com/marcelm/cutadapt, https://bioconda.github.io/user/install.html, https://cutadapt.readthedocs.io/en/stable/installation.html). To use cutadapt a cutadapt environment must be activated first.
The @INC where everything, including the trim_adaptor.pl script, is located is `/Users/laurelnave-powers/opt/miniconda3/lib/5.32.0/darwin-thread-multi-2level`. 
The location where the script and all modules/packages are downloaded needs to be added to the path. 

Before running the script, activate the cutadapt environment and change directories to where all the modules, script, etc. are located.
Then run the script! 
The output will be where you specify it, in this case in the directory `../analyses/trimmed`. It will also give supplemental report files that can be put into a new directory `../analyses/trimmed_supp`. 

### Step 3 - Alignment
Now the trimmed reads need to be aligned to a reference genome (see 05_bowtie.ipynb). The reference genome used for this project is Danio rerio downloaded from https://uswest.ensembl.org/Danio_rerio/Info/Index.  
This step uses the program bowtie2, which can be downloaded from http://bowtie-bio.sourceforge.net/bowtie2/index.shtml. 
The first thing to do is build an index from the reference genome. Then using that index and your trimmed reads from Step 2, you can get SAM alignment files. This will also tell you the overall alignment rate. 

### Step 4 - SNP variants
Once the files are aligned, you can call SNPs (see 06_SNP.ipynb). The first thing to do is convert your SAM files to indexed and sorted BAM files. You can use `samtools` to do this, downloaded from http://www.htslib.org/download/. Then taking your sorted BAM file and the reference genome, get VCF files.
Now you can call the variants using `bcftools`, downloaded from http://www.htslib.org/download/. 
To view the variants use `zgrep`. This will also show you the quality of each variant. 


#### Project Results and Methods: https://docs.google.com/document/d/10w9FyI_e_Mdh8gLQqkYOMof-ve4HF2w1F0opoeYV2-I/edit 
#### Project Presentation: https://docs.google.com/presentation/d/1Eo_d4hXIP1hrK1YCjIBiCaEUTZT9SXZCXB4ENttzoa4/edit#slide=id.p 