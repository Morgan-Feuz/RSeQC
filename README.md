# RSeQC for RNA-seq Quality Control


### Background 
[RSeQC](https://rseqc.sourceforge.net/) is a Python package with many modules for evaluating RNA-seq data. Some basic modules quickly inspect sequence quality, nucleotide composition bias, PCR bias and GC bias, while RNA-seq specific modules evaluate sequencing saturation, mapped reads distribution, coverage uniformity, strand specificity, transcript level RNA integrity etc.

<ins>Citations:</ins>
+ Wang, L., Wang, S., & Li, W. (2012). **RSeQC: quality control of RNA-seq experiments**. _Bioinformatics_ (Oxford, England), 28(16), 2184–2185. [http://doi.org/10.1093/bioinformatics/bts356](http://doi.org/10.1093/bioinformatics/bts356)
+ Wang, L., Nie, J., Sicotte, H., Li, Y., Eckel-Passow, J. E., Dasari, S., et al. (2016). **Measure transcript integrity using RNA-seq data**. _BMC Bioinformatics_, 17(1), 1–16. [http://doi.org/10.1186/s12859-016-0922-z](http://doi.org/10.1186/s12859-016-0922-z)

---------------------------------------------------------------------------------------------

### Installation
Here, I will first create a [Conda virtual environment](https://github.com/Morgan-Feuz/conda-virual-envs) and then install RSeQC via `pip`. 

```
$  conda create -n rseqc
$  conda activate rseqc
(rseqc)$ pip install RSeQC
```

----------------------------------------------------------------------------------------------


### Usage

<ins>Input Format:</ins>
+ [BED](http://genome.ucsc.edu/FAQ/FAQformat.html#format1) file is tab separated, 12-column, plain text file to represent gene model. 
+ [SAM](http://samtools.sourceforge.net/) or [BAM](http://genome.ucsc.edu/goldenPath/help/bam.html) files are used to store reads alignments. SAM file is human readable plain text file, while BAM is binary version of SAM, a compact and index-able representation of reads alignments.
+ Chromosome size file is a two-column, plain text file. Here is an [example](http://dldcc-web.brc.bcm.edu/lilab/liguow/RSeQC/dat/sample.hg19.chrom.sizes) for human hg19 assembly. 
+ [Fasta](http://en.wikipedia.org/wiki/FASTA_format) file.

<ins>Note:</ins> For GFF/GTF format gene files, [BEDOPS](https://github.com/Morgan-Feuz/BEDOPS) can be used to easily convert them to a BED format. 

<br>

#### RSeQC Modules

`infer_experiment.py`
+ This program is used to “guess” how RNA-seq sequencing were configured, particulary how reads were stranded for strand-specific RNA-seq data, through comparing the “strandness of reads” with the “standness of transcripts”.
+ The “strandness of reads” is determiend from alignment, and the “standness of transcripts” is determined from annotation.
+ For non strand-specific RNA-seq data, “strandness of reads” and “standness of transcripts” are **independent**.
+ For strand-specific RNA-seq data, “strandness of reads” is largely determined by “standness of transcripts”.
+ You don’t need to know the RNA sequencing protocol before mapping your reads to the reference genome. Mapping your RNA-seq reads as if they were non-strand specific, this script can “guess” how RNA-seq reads were stranded.

```
# Example of infer_experiment.py usage
(rseqc)$ infer_experiment.py -r /path/to/bedfile/bed_file.gz.bed -i /path/to/bamfile/bam_file.bam

This is SingleEnd Data
Fraction of reads failed to determine: 0.1307
Fraction of reads explained by "++,--": 0.0227
Fraction of reads explained by "+-,-+": 0.8466

```
In practice, with Illumina RNA-seq protocols, you will most likely encounter either (1) unstranded RNA-seq data or (2) stranded RNA-seq data produced with kits and dUTP tagging. Knowing the strandedness of the reads is important during the mapping and counting steps of RNA-seq data pre-processing and analysis. `infer_experiment.py` is useful to check the strandness of bam files if you do not already know what protocol was used. The table below is a good reference for interpreting the output of `infer_experiment.py`. [Here](https://artbio.github.io/startbio/reference_based_RNAseq/strandness/) is there referece source for the table. 

![image](https://github.com/user-attachments/assets/371ad04c-5d2f-47f5-8be2-abd8370e8c2c)












