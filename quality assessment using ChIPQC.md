In addition to the initial quality check on our raw data (`fastQC` run on fastq files), alignment of these fastq files and the way they aligned  
to a reference genome also tells us about the success/failure of one's `chip-seq` experiment. There are various matrices whic can tell about a sucessful  
`chip-seq` experimet. we wil look at some of them. we will use `R/Bioconductor package ChIPQC` that provides a comprehensive ChIP-seq-specific quality control analysis.  

We first need to create a directory structure. First we create a folder `chipqc_project` and then inside it we need to create 4 sub directory.  
```bash
mkdir -p chipqc_project/{data,meta,results,figures}
```




At this stage we need a special `file` called `sample-sheet`.  
[sample_sheet.csv](https://github.com/DeepakHansda/ChIP-seq-workflow/files/8719429/sample_sheet.csv)  
Place it inside `meta` folder.  













