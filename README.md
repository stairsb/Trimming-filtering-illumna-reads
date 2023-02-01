# Trimming and filtering Illumna short reads
This repository shows how to trim illumina adapters and some filtering options to prepare data for pipelines and public database submissions. This example is for paired end illumina reads but can also be used for single reads.

## fastqc
Did you just get single or paired end reads back and want to check the quality? Well you came to the right place because `fastqc` will show you the way.  `fastqc` can only handle one paired end read at at time. It's as simple as running the command below. 
Documentation: https://www.bioinformatics.babraham.ac.uk/projects/fastqc/ 
```
fastqc readfile.fastq
```
Before trimming or filtering: https://stairsb.github.io/56028_R1_fastqc.html

## How to decide what options to use based on fastqc output
## Trimming and filtering
fastp is a one stop shop for prepping you illumina data for downstream analysis. Here we will be using it to trim off illumina adapters and do some essential filtering. If you are working with paired end data you need to let the program know that those adapters need to go! fastq can detect illumina Trueseq adapters using the automatic option. Documentation: https://github.com/OpenGene/fastp#quality-filter 

### Sequences length distrobution graph 
The peak of the distrobution should be in the 140-160 bp range. Look at the y-axis of the graph. Ideally you should have 5,000,000 reads or greater, remember that this is paired end data so in total you should have 10,000,000 for in total when combining the paired fastq files. If there are a large amount of short reads that you don't want add the `-l` and specify the length that is right for you.

### Per sequence quality scores
The higher phred score the better. ideally you should see a peak in the high 30s or you might want to try the error correction option in 'fastp' that takes the advantage of the convenience of paired end data.

### Per base sequence content
You want AT and GC lines to be flatlined and overlapping

### adapter content
This one is intuitive

### N content
`fastp` will automatically filter our bases represented with N unless you manually turn off filtering. If it didn't do a sufficient job add more strict filtering parameters or error correction.

### Defaults
```
Quality filter: >15 phred scor
Unqalified filter allowed: max 40%
Low complexity filter: max 30%  ex. "TTTTTTTTTTTTTTAAAAAAAAAAAAAA"
Adapter trimming paired end: --detect_adapter_for_pe or --adapter_sequence=insertadapterseq1 --adapter_sequence_r2=insertadapterseq2
Length filter: 0
PolyG tail trimming: 10
```
```
fastp -i read_1.fastq -I read_2.fastq -l 159 --detect_adapter_for_pe -o trmd_fltrd_1.fastq -O trmd_fltrd_2.fastq -h read.html
```
Output information: https://stairsb.github.io/56028.html

Check the `fastp` output html and rerun fastqc on your trimmed reads to make sure they are ready to produce quality data.
Trimmed and filtered fastqc: https://stairsb.github.io/56028_trmd_1_fastqc.html

