In addition to the initial quality check on our raw data (`fastQC` run on fastq files), alignment of these fastq files and the way they aligned  
to a reference genome also tells us about the success/failure of one's `chip-seq` experiment. There are various matrices whic can tell about a sucessful  
`chip-seq` experimet. we wil look at some of them. we will use `R/Bioconductor package ChIPQC` that provides a comprehensive ChIP-seq-specific quality control analysis.  

1. We first need to create a directory structure. First we create a folder `chipqc_project` and then inside it we need to create 4 sub directory.  
```bash
mkdir -p chipqc_project/{data,meta,results,figures}
```
2. Inside `data` create two sub-directory `bam` and `peakcalls`  
3. copy `BAM` files (.bam) and corresponding index files (.bam.bai) to `data/bam` folder i.e the `BAM` files we get after (sorting > indexing > filtering).  
4. copy `narrowPeak` file (.narrowPeak) to `data/peakcalls` i.e. we get after `macs2 callpeak` and kept at folder `peaks`.  
   
At this stage we need a special `file` called `sample-sheet`.  
[sample_sheet.csv](https://github.com/DeepakHansda/ChIP-seq-workflow/files/8719429/sample_sheet.csv)  
Place it inside `meta` folder. 

Now we ready to run `ChIPQC`. Open `Rstudio` and go to (file > new file > chipqc.R) and do the following in newly created file `chipqc.R`.  

```R
load.library(ChIPQC)
samples <- read.csv('meta/sample_sheet.csv')
# create ChIPQC object
chipObj <- ChIPQC(samples, annotation="mm10")
## Create ChIPQC report
ChIPQCreport(chipObj, reportName="ChIP_QC_report: NRF1_wt and NRF1_TKO", 
             reportFolder="chipqc_report")
 ```
 

 
 
 
 
















