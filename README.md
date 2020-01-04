# externalATACdata
notes and code for importing and processing external ATAC seq data

Started by downloading mouse tissue ATAC atlas data from PMID:
    31110271 (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6527694/)
downloaded the NCBI sratoolkit.2.9.6-1-mac64/bin and used to download NCBI deposited raw data files (gzipped fastq)
Found these files using the accession number in the paper via NCBI SRA Run selector--downloaded list of runs then used the prefetch tool (not sure why prefetch commands are missing from bash history)
In a few cases, used the fastq-dump tool to convert compressed files to fastq.
Now need to deal with trimming, but don't know the adaptors. Will try to use fastQC and TrimGalore; installed fastQC, but now need to download TrimGalore script from GitHub.
Installed TrimGalore using curl -fsSL https://github.com/FelixKrueger/TrimGalore/archive/0.6.5.tar.gz -o trim_galore.tar.gz
then tar xvzf trim_galore.tar.gz

Installed cutadapt using pip3 install --user --upgrade cutadapt

Added '/Users/sachanelson/.local/bin' to the PATH

Downloaded fastqc to /Users/sachanelson/Applications and added link: sudo ln -s ~/Applications/FastQC/fastqc /usr/local/bin/fastqc

FastQC shows what looks like might be adaptors in ~0-10 and ~50-60, but no repetitive sequences identified. I realized this may be because the fastq file has both paired reads in it instead of these being separated into different files. Found https://edwards.sdsu.edu/research/fastq-dump/ which describes using --split-files and -I options with fastq-dump to produce paired files from downloaded SRA data. 

Somehow I have wound up with unzipped files--need to fix that.

trim_galore found the Nextera transposase sequence and trimmed using that. But then failed validation: "Read 2 output is truncated at sequence count: 48281844, please check your paired-end input files! Terminating..."

Used fasterq-dump (defaults to split 3, doesn't deal with gzip, but much faster when run on local solid state drive. gzip is taking a long time though.
