```bash  
for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2  
do  
	fastqc -q -o data data/${sample}.fastq.gz  
  done  
  ```
  
  Once your analysis is done. you can view the different quality score from the ```html``` files; double click on the ```html``` file.  
  If we do find some discrepensies in our data. we run ```Trim galore``` to clean the data
  
  ```bash
  for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 NRF1_INPUT_WT NRF1_CHIP_TKO_1 NRF1_CHIP_TKO_2 NRF1_INPUT_TKO H3K27AC_CHIP_WT_1 H3K27AC_CHIP_WT_2 H3K27AC_CHIP_TKO_1 H3K27AC_CHIP_TKO_2  
  do  
  trim_galore -q 20 --stringency 2 -o data data/${sample}.fastq.gz  
  done  
  ```
  We can rerun FastQC to check the trimming performance. There are good chances are that you would find an improved data sets.  
  
