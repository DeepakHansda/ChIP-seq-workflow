data retrivel from the GEO/SRA. First make a directory where all the retrieved data will be kept.  
**mkdir data**  
And then we navigate to the *data* directory  
**cd data**  
Download raw data (fastq files)  

**for file in** SRR2500883, SRR2500884, SRR2500893, SRR2500894, SRR2500885, SRR2500886, SRR2500887, SRR2500895, SRR2500896, SRR2500888  
**do  
	fastq-dump --origfmt --outdir data --gzip -A ${file}  
	done**  
  
 Now, rename the files. It is a good practice to give the file a meaningful name. It helps us to  
 keep track of what experimental condition a particular file represent.  
 
**mv data/SRR2500883.fastq.gz data/NRF1_CHIP_WT_1.fastq.gz  
mv data/SRR2500884.fastq.gz data/NRF1_CHIP_WT_2.fastq.gz  
mv data/SRR2500893.fastq.gz data/H3K27AC_CHIP_WT_1.fastq.gz  
mv data/SRR2500894.fastq.gz data/H3K27AC_CHIP_WT_2.fastq.gz  
mv data/SRR2500885.fastq.gz data/NRF1_INPUT_WT.fastq.gz  
mv data/SRR2500886.fastq.gz data/NRF1_CHIP_TKO_1.fastq.gz  
mv data/SRR2500887.fastq.gz data/NRF1_CHIP_TKO_2.fastq.gz  
mv data/SRR2500895.fastq.gz data/H3K27AC_CHIP_TKO_1.fastq.gz  
mv data/SRR2500896.fastq.gz data/H3K27AC_CHIP_TKO_2.fastq.gz  
mv data/SRR2500888.fastq.gz data/NRF1_INPUT_TKO.fastq.gz**  

 
