Peak calling is the core of every ChIP-seq data analysis pipeline as it identifies the genomic regions occupied by the transcription factor (TF) or histone mark of interest.  
Here we will use `MACS2` peak caller.  
```bash
control=NRF1_INPUT_WT
for sample in NRF1_CHIP_WT_1 NRF1_CHIP_WT_2 
	do
	macs2 callpeak -t reads/${sample}.bam\
	-c reads/${control}.bam \
	-f BAM \
	-g mm \
	 --outdir peaks \
	-n ${sample}  2> peaks/${sample}_macs2_report.log
	done
  ```
  As for for this workflow example we use 4 samples (NRF1_CHIP_WT_1, NRF1_CHIP_WT_2, NRF1_CHIP_TKO_1, NRF1_CHIP_TKO_2) to focus on the transcription factor `NRF1`.  
  So we need to rerun `macs2 callpeak` for samples NRF1_CHIP_TKO_1, NRF1_CHIP_TKO_2.  
  ```bash
  control=NRF1_INPUT_TKO
for sample in NRF1_CHIP_TKO_1, NRF1_CHIP_TKO_2 
	do
	macs2 callpeak -t reads/${sample}.bam\
	-c reads/${control}.bam \
	-f BAM \
	-g mm \
	 --outdir peaks \
	-n ${sample}  2> peaks/${sample}_macs2_report.log
	done
  ```
