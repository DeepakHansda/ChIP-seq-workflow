 Once we are satisfied with our fastq data set, we can proceed with the alignment of our sample(s) to a reference genome (mouse genome here).  
 
 Create appropriate directory for the read files.  
 ```bash
 mkdir reads
mkdir -p genomes/mm10
mkdir -p indices/mm10
```
Now we need to get the reference genome. we download it from UCSC.  
```bash
wget http://hgdownload.soe.ucsc.edu/goldenPath/mm10/bigZips/chromFa.tar.gz -O genomes/mm10/chromFa.tar.gz
```
once we download the reference genome, we need to unwrap it.  
```bash
tar -zxvf genomes/mm10/chromFa.tar.gz -C genomes/mm10
```
Next, we will keep only the conventional chromosomes (chr1-19,X,Y,M).  
```bash
rm genomes/mm10/chromFa.tar.gz genomes/mm10/*random* genomes/mm10/chrUn*
```
before we proceed to align our fastq files to a reference genome (mouse genome here) we need to index reference genome, As indexing of the reference  
genome is quite important for efficient search and retrival.  
```bash
cd genomes/mm10/
bowtie2-build chr1.fa,chr10.fa,chr11.fa,chr12.fa,chr13.fa,chr14.fa,chr15.fa,chr16.fa,chr17.fa,chr18.fa,chr19.fa,chr2.fa,chr3.fa,chr4.fa,chr5.fa,chr6.fa,chr7.fa,chr8.fa,chr9.fa,chrM.fa,chrX.fa,chrY.fa ../../indices/mm10/mm10  
```
Once the bowtie2 index is generated it would produce 6 files with suffixes .1.bt2, .2.bt2, .3.bt2, .4.bt2, .rev.1.bt2, and .rev.2.bt2.  
it will be on indices/mm10/  
```bash
cd ../..
```
reverting back to the parent directory.  


Now that we have created bowtie2 index for our reference genome, we can align our fastq reads to it.  
Since we have trimmed our fastq reads (please look at the "_trimmed.fq.gz" files in "data" directory)  
we will use the trimmed ones; -p argument is for the nos. of cores to be used. set this value as per the availability of cores in your system.  
  
```bash
for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
	do
	bowtie2 -p 6 -x indices/mm10/mm10 -U data/${sample}_trimmed.fq.gz -S reads/${sample}.sam
	done
  ```
  


