In addition to the initial quality check on our raw data (`fastQC` run on fastq files), alignment of these fastq files and the way they aligned  
to a reference genome also tells us about the success/failure of one's `chip-seq` experiment. There are various matrices whic can tell about a sucessful  
`chip-seq` experiment. we wil look at some of them. we will use `R/Bioconductor package ChIPQC` that provides a comprehensive ChIP-seq-specific quality control analysis.  

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
 
 ## ChIPQC report  
 Once you are done with running `ChIPQC`, it does produce a veriety of reports (please look at the folder `chipqc_report`). you should find few figures and a `html` file among other things. Open this `html` file. In `QC Summary` table  there are certain mertices which can tell you about how good your chip-seq experiment are. Few of them are:  
 
  ### ChIPQC summary:  
 ![chipqc_summary](https://user-images.githubusercontent.com/85447250/169354546-a4c8709e-a46d-491e-99e0-48b5415c5a2b.png)  
 
1. Percentage of reads within `peaks` (RiP%): Generally a sucessfull chip-seq experiment will show >5% of reads inside `peaks` for  
 Transcription factors and > 25-30% of reads inside `peaks` for Histone modification. We can see that `RiP%`for both `WT` and `TKO` is above 5% mark. it does indicate a good chip-seq library (See RiP% column in the table above).  
2. Standardised Standard Deviation (SSD): A good enrichment shows high `SSD` score. A high SSD score means that there are regions with high signal in the sample. A similar scores for both `Input` and `Sample` indicates an unsucessful chip-seq experiment. For our case the  
difference between SSD score of `Input` (NRF1_INPUT_TKO) and `sample` (NRF1_CHIP_TKO_2) is atleast 5 times (3.0/0.58). So, we are confidnt that experiment went well from this point of view (See column SSD in table above).   
3. Enrichment in expected genomic regions: Most factors will be enriched in one or a few of the following annotated genomic regions: promoters, exons, introns, 5' or 3' untranslated regions (UTRs). The relative enrichment of ChIP-seq reads within these regions indicates whether the expected genomic distribution was captured or not. Here we are dealing with TF so you can expect that most of the pulled down reads will stick to promoter regions. See figure below you can find that most of the reads points towards `Promoters500` and `All5utrs`.  

![genomic_features](https://user-images.githubusercontent.com/85447250/169380633-b42b03f6-f4f5-41d8-8e25-462e0b876f40.png)

4. Read depth distribution: a good chip-seq sample will have many bases where read coverage (read depth) will be relatively low and 
few bases where read depth will be quite high. these high depth regions form peaks. again, an indication of good signal enrichment. (see Fig. below)   
![read_depth](https://user-images.githubusercontent.com/85447250/169386944-642006ea-9e04-4591-b0e5-c6cfcf5d2517.png)

 
 
 

 
 
 
 
















