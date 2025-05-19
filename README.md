# EasyMicrobiomeAnalysis_R
## Functional prediction of bacteria and fungi
## dependency packages:
Tax4Fun2  
## Main function:
runRefBlast2();  
makeFunctionalPrediction()  
### Programs
rm(list = ls())  
setwd('Your path')  
library(Tax4Fun2)  

### Tax4Fun2-function prediction（Tax4Fun2-Bacteria Functional predictions）

source("./function/runRefBlast2.R")  
source("./function/makeFunctionalPrediction.R")  

### New output path
funcpath = paste("./Tax4Fun2_prediction/",sep = "")  
dir.create(funcpath)  

### First configure the database - it may fail often, it is recommended to use the backup library here
buildReferenceData(path_to_working_directory = '.', use_force = FALSE, install_suggested_packages = TRUE)  
path_to_reference_data = "./Tax4Fun2_ReferenceData_v2"  

### Pre-installed blastn.exe software (optional)
blast_bin = file.path(path_to_reference_data, "blast_bin/bin/blastn.exe")  
res = system(command = paste(blast_bin, "-help"), intern = T)  

otudir = funcpath  

### Species annotation
#### Specify the OTU representative sequence, the location of the Tax4Fun2 library, the version of the reference database, the number of sequence comparison (blastn) threads, etc.
runRefBlast2(path_to_otus = './data/otu.fa',  
             path_to_reference_data = path_to_reference_data,  
             path_to_temp_folder = otudir, database_mode = 'Ref99NR',  
             use_force = TRUE, num_threads = 30)  


### Predictions of community function  
#### Specify the OTU abundance table, the location of the Tax4Fun2 library, the version of the reference database, the path of the species annotation results from the previous step, and so on.  
makeFunctionalPrediction(path_to_otu_table = './data/otu.txt',  
                         path_to_reference_data = path_to_reference_data,  
                         path_to_temp_folder = otudir,  
                         database_mode = 'Ref99NR',  
                         normalize_by_copy_number = TRUE,  
                         min_identity_to_reference = 0.97,  
                         normalize_pathways = FALSE)  
                           

                         
### File output
logfile1.txt;  
ref_blast.txt;  
logfile2.txt;  
pathway_prediction.txt  
functional_prediction.txt  


                         
