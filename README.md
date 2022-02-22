# MSmetaProfiler

This repository hosts Metabolome Annotation Workflow from LCMS-2 spectra in mzML format. The workflow has been developed using LCMS-2 dataset from a marine diatom _Skeletonema costatum_.

## Usage
### Installation


### Input files and Directories

An input directory (input_dir) should have the following files for the Workflow to run.
1. All LCMS-2 spectra .mzML files
2. SIRIUS installation zip folder
3. MetFrag jar file which can be downloaded from <https://github.com/ipb-halle/MetFragRelaunched/releases>. You can also download the MetFrag version shared in this repository.
4. MetFrag_AdductTypes.csv can be downloaded from <https://github.com/schymane/ReSOLUTION/blob/master/inst/extdata/MetFrag_AdductTypes.csv>
5. (optional) /QC folder containing all (MS1) QC .mzML files 
6. (optional) Suspect List in csv format (important column - "SMILES")
7. (optional) Sl_metfrag.txt (This file contains the InChIKeys for all SMILES in suspect list is generated by the python function - slist_metfrag) for MetFrag
8. (optional) ScostSLS/ (This folder is a fragmentation folder for each SMILES present in the suspect list and is generated by the python function - slist_SIRIUS) for SIRIUS 

### Tutorial of Workflow

1. Load Dependencies:

```library(Spectra)
library(MsBackendMgf)
library(MsBackendHmdb)
library(MsCoreUtils)
library(MsBackendMsp)
library(readr)
library(dplyr)
library(rvest)
library(stringr)
library(xml2)
```

2. Define input directory. Make sure that you have input LCMS-2 spectra files in .mzML format in the input directory.

```
input_dir <- "usr/s_cost/"
```
3. Load the open spectral libraries. This function will take a lot of computational resources. However, to skip this function, you can download the current versions of the these databases from <write the link here and ask where can you provide datasets>. The databases are stored in the same input directory and in .rda format, as a R object.

```
# db argument can take "all", "gnps", "mbank" and "hmdb". Default (and recommended) is "all".
download_specDB(input_dir, db = "all")
```
4. Load the database .rda objects to the current R session.
  
```
load(file = paste(input_dir,"gnps.rda", sep = ""))
load(file = paste(input_dir,"hmdb.rda", sep = ""))
load(file = paste(input_dir,"mbank.rda", sep = ""))
```
5. Create a table that lists all the input .mzML files and the result directories with the same name as the input file. The function ```ms2_rfilename``` gives an id to the file as well.
  
```
input_table <- data.frame(ms2_rfilename(input_dir))
```
               
