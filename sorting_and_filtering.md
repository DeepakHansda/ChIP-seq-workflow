1. Next we make unsorted BAM files from the SAM files obtained from "bowtie2" alignment.  
```bash
for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2 
	do
	samtools view -h -S -b \
	-o reads/${sample}_unSorted.bam \
	reads/${sample}.sam
	done
  ```
  2. Sort BAM files by genomic coordinates.  
  ```bash
  for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
	do
	samtools sort reads/${sample}_unSorted.bam > reads/${sample}_Sorted.bam
	done
  ```
  3. Next step would be to index BAM files for faster access.  
  ```bash
  for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
	do
	samtools index reads/${sample}_Sorted.bam
	done
  ```
  4. Keep the uniquely mapped reads. means, we filter out unmapped reads, duplicates and multimapped reads. Next step of peak calling require these reads to be out from BAM files beforehand.  
  ```bash
  for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
	do
	sambamba view -h -t 6 -f bam \
	-F "[XS] == null and not unmapped  and not duplicate" \
	reads/${sample}_Sorted.bam > reads/${sample}.bam
	done
  ```
  
  You may remove intermediate files as it help us with our precious drive space.  
  ```bash
  for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
  do
  rm reads/${sample}.sam reads/${sample}_unSorted.bam reads/${sample}_sorted.bam  
  done
  ```
  
  We can have a look at one of the `BAM` file which is (sorted > indexed > filtered).    
  ```bash
  sample=NRF1_CHIP_WT_1
samtools view reads/${sample}.bam | head
or 
bamToBed -i reads/${sample}.bam | head
```


It has to be noted that one more filter is required for getting rid of the `Blacklisted` region of a genome; these are the regions where ChIP-seq  
signal is artificially spiked up; so we need to discard those reads as well. To filter out `Blacklisted` regions, first we need to get `BED` file of each corresponding `BAM` files.  
```bash
for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
	do
	bamToBed -i reads/${sample}.bam > reads/${sample}.bed
	done
  ```
 Removal of reads in blacklisted regions.  
 ```bash
 for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
	do
	intersectBed -v -a reads/${sample}.bam \
	-b mm10_blacklist.bed > reads/${sample}_free_of_blacklist.bam
	done
  ```
  
  Now that we done with all sorts of cleaning the data, we should rename our data sets to more comprehensive and simple names (rename all "_free_of_blacklist.bam" to ".bam" file).  
  ```bash
  for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2
	do
	mv reads/${sample}_free_of_blacklist.bam reads_final/${sample}.bam
	done
  ```
  Now that we  have sorted, indexed, filtered and removed `blacklisted` region from our sample files, we can proceed to the all important job of `peak calling`  
  it is an computational approach to identify the enriched regions of genome (reference genome) in a ChIP-seq experiment.
  
  
  
  
  
  
  
  
