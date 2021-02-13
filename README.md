# Bioinformatics 2021 Class Project
## Laurel Nave-Powers 
#### Instructions modified from Calder Atta 
https://github.com/calderatta/ca-exon-capture 

# Note- I was stuck with a lot of errors trying to download the various modules and packages and did not get it resolved until Thursday during classtime so I only have intermediate products at this stage. 

The data used in this project are raw reads of flatfish exon capture data from Calder Atta. I am using two species: Hippoglossoides platessoides and Microstomus pacificus. The end goal of the project is to be able to look at SNP variants for these samples.

### Step 1 - Unzipping
I started with raw data in my raw data folder `~/Desktop/GitHub/Laurel_genes/data/raw`.
These are compressed fastq files from exon capture, one file for forward (R1) and one for reverse (R2) for each species. To take a look at them, they need to be unzipped (see 01_unzip.ipynb). For these samples, I don't need to merge lanes because they were only run once.

### Step 2 - Trimming
The next step is to trim adaptors and low quality bases (see 02_trim.ipynb). To use the perl script trim_adaptor.pl the input files must be in .fq NOT .fastq. Once changed, the .fq files are moved into their own folder `~/Desktop/GitHub/Laurel-genes/data/demultiplexed`. 
Before running the perl script, check the options and make sure it works by using `trim_adaptor.pl -h`.
In order for the script to work, there are 6 perl modules and two packages to be downloaded into the @INC. 

The modules can be downloaded from the CPAN website (https://metacpan.org) and moved into the folder indicated in the @INC. Modules used:  Moo, Parallel::ForkManager, Sub::Defer, Sub::Quote, Method::Generate::Constructor, Role::Tiny. 

The packages can be downloaded from their respective websites and put in the same folder as the modules.
Trim_galore is an easy download (https://github.com/FelixKrueger/TrimGalore). Cutadapt is more complicated and requires you to download via conda (https://github.com/marcelm/cutadapt, https://bioconda.github.io/user/install.html, https://cutadapt.readthedocs.io/en/stable/installation.html). To use cutadapt a cutadapt environment must be activated first.
The @INC where everything, including the trim_adaptor.pl script, is located is `/Users/laurelnave-powers/opt/miniconda3/lib/5.32.0/darwin-thread-multi-2level`. 
The location where the script and all modules/packages are downloaded needs to be added to the path. 

Before running the script, activate the cutadapt environment and change directories to where all the modules, script, etc. are located.
Then run the script! 
The output will be where you specify it, in this case in the directory `Laurel-genes/analyses/trimmed`. It will also give supplemental report files that can be put into a new directory `Laurel-genes/analyses/trimmed_supp`. 

## Plan for weeks 6-10:

### Step 3 - Remove duplicate reads from PCR
Now that the data is trimmed it's time to remove the duplicate reads from PCR (see 03_rmdup.ipynb). First I will download the script rmdup.pl and put it in the same location as all of the modules and packages `/Users/laurelnave-powers/opt/miniconda3/lib/5.32.0/darwin-thread-multi-2level`.  

To check that the script will work and has all required programs use `rmdup.pl -h`. 
Install all required programs necessary. 
Run the script! The input will be the trimmed files from the output of Step 2. The output will be files with duplicate reads removed(?). 

### Step 4 - Parse reads to each locus
Next I will take the output from Step 3 and use it as input in the script ubxandp.pl. This script will parse reads to each locus. As before, the first thing I will do is check that I have all the required modules by using `ubxandp.pl -h`. 

### Step 5 - SNP variants
Once I have the parsed reads, I can map them to our fully assembled sequences to get SNP variants. 
To find SNPs I might be able to use `samtools mpileup -no-BAQ -region ####-#### -fastaref name.fasta name.bam`. 
To view the SNPs I could use `samtools tview`. 

