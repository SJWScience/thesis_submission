# thesis_submission


#### This is a list of codes used. Commands have been stripped of file names and replaced with generic placeholders. It is assumed that the programs are already installed and in your path, either on your local machine, or on a server. Although many of these programs were installed by myself, different configurations of servers and machines make it impossible for me to translate that code into something that will work for everyone.

#####Section 2.3.1 - Whole genome sequencing and RNA sequencing sample processing and quality control

>**FastQC analysis on raw and trimmed sequences, code 1 was used to analyse multiple files at once, code 2 was used for individual files

```bash
zcat *.fastq.gz | fastqc stdin

fastqc input1.fastq.gz 

```

>**Trimming raw sequences, using TRIMMOMATIC**

```bash
java -jar trimmomatic-0.36.jar PE -threads N -phred33 sample_read1.fastq.gz \
sample_read2.fastq.gz output1_forward_paired.fq.gz output1_forward_unpaired.fq.gz \
output1_reverse_paired.fq.gz output1_reverse_unpaired.fq.gz \
ILLUMINACLIP:Nextera-PE.fa:2:30:10 LEADING:5 TRAILING:5 SLIDINGWINDOW:4:20 MINLEN:20

``` 
>I intentionally left the trimming to be quite lenient, this is a personal preference.


#####Section 2.3.2.	Draft genome assembly

>**Genome assembly using SPAdes**

```bash
spades.py --careful --threads N --pe1-1 output1_forward_paired.fq.gz --pe1-2 output1_reverse_paired.fq.gz -o output1_draft_assembly

```
>I am aware SPAdes has its own inbuilt read correction tool - but i find higher quality assembles with pre-trimmed and cleaned data.


#####Section 2.3.3.	Core genome reconstruction and phylogeny prediction

>**Using parsnp to generate a core genome for phylogenetic tree generation**

```bash
parsnp -g PAO1.gbk -d /Directory/of/genome/assemblies/ -p N -c -x

```

#####Section 2.3.4.	Draft genome comparison and visualisation

>**Using mauve to reorder draft genome contigs relative to a reference strain**

```bash
java -Xmx500m -cp Mauve.jar org.gel.mauve.contigs.ContigOrderer -output reordered_contigs_output1 -ref PAO1.gbk -draft output1_draft_assembly.fasta

```

>**Using BRIG to compare draft genomes to each other**
