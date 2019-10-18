# MoBa Genetics - interim release September 2019

## Introduction

Over the last decade several research projects have obtained funding to genotype subsets of samples from the Norwegian Mother, Father and Child Cohort Study (MoBa). Until recently the Norwegian Institute of Public Health (NIPH) was not allowed to retrieve and redistribute the results/genotypes from these projects to researchers for reuse in other projects. To obtain access to these datasets each researcher would need to initiate a direct collaboration with the PI of the project the data originated from. Fortunately, a recent legislative change now allows NIPH to retrieve and provide access to these data as a single resource. The name of this initiative is MoBa Genetics.

## Background

Compared to other large biobanks like eg. the UK Biobank, where considerable funding was secured upfront allowing for genotyping their entire cohort in a single effort, genotyping in MoBa have had to rely on several projects - each contributing with resources to genotype subsets of MoBa over the last decade. As a consequence, genotyping was performed years apart at different labs using different arrays. This increases the complexity of curating the resulting data into a single resource easily accessible to researchers for downstream analysis. Why this is currently a work-in-progress where several researchers are contributing aiming to provide high quality data to future projects. 

NIPH would like to release a fully curated resource containing all samples in MoBa after close quality scrutiny with proper documentation and an accompanying backend to quickly provide access for new projects. However, this would mean researchers with the necessary competence to utilize the data in its current state would have to wait for us to complete this work. For this reason NIPH has decided to prepare an **interim release** of the current datasets in our repository to all researchers through TSD. 

This release comes with the following disclaimer: Although all the batches in the current release have been quality controlled, pre-phased and imputed (see description below) there is still ongoing work retrieving additional metadata from the respective cohorts, running additional quality assurances and preparing proper documentation. Although we have no reason to believe there are any bugs in the quality control, researchers who want to utilize this interim release should be aware of its current status and hence scrutinize their results carefully. Due to limited resources NIPH will not have the capacity to help less experienced researchers starting out with GWAS analyses and bioinformatics in general. However, we would like to facilitate researchers working with MoBa helping out each other and have created a Slack channel at [mobagen.slack.com](https://mobagen.slack.com) where we encourage people to join and contribute. To join please use this [invitation link](https://join.slack.com/t/mobagen/shared_invite/enQtNzc3NDk1MDE4NTYzLWE5OTg5M2FiYzA4Mzg2MmEyOTlkZTc4M2UzY2Q4N2Y1ODNiZjE5Yjk3N2M4YTg3YjE4NTcwMzk2YjQ5OWZiNWE).

It is worth noting that genotyping in MoBa is still ongoing and more data will become available as projects complete their genotyping efforts.
This github repository will be updated with more information about new datasets as they become available.


## Quality control (QC)
Providing a "one size fits all"-QC is difficult. Some researchers are likely going to prefer doing their own QC for various reasons. We try to accommodate this by providing signal intensities and raw genotype calls (PLINK format) for those who would like to embark on this. Additionally, we regard the availability of signal intensities necessary in an interim release to enable researcher to check the underlying calls if needed. For those who would like to use the quality controlled data a detailed QC-report and accompanying QC-flowchart is available for the [respective projects](https://github.com/folkehelseinstituttet/mobagen/wiki/Projects-that-have-contributed-to-MoBa-Genetics) for full transparency.


## MOBAGENETICS
MoBa Genetics is the merger of all subprojects described on the [Wiki](https://github.com/folkehelseinstituttet/mobagen/wiki/Projects-that-have-contributed-to-MoBa-Genetics) and after quality control and imputation. The merged dataset contains 98110 samples. The sparse overlap between directly genotyped markers on the various chips used in the respective projects resulted in a too sparse backbone for pre-phasing all samples together that would have made harmonization a lot easier.

### Pre-phasing and imputation
All batches were imputed at the [Sanger Imputation Service](https://imputation.sanger.ac.uk/) using PBWT and the HRC v1.1 reference panel. Pre-phasing prior to imputation was performed using Shapeit2. In batches with available triads, pre-phasing was performed locally (pre-phasing using pedigree information was not possible at Sanger). In batches without triads pre-phasing and imputation were both performed at Sanger. Of special note, the X-chromosome was always pre-phased without pedigree information due to limitations in the software.

### Minor changes
Moba Genetics is for all practical purposes merely a merge of all the batches post imputation and very little tinkering is done to the files returned from the imputation service except for 1) renaming SNPs and 2) generating weighted average information scores. 

#### SNP-naming
Most markers in the HRC v1.1 reference panel have rsIDs. Those without an rsID are returned with a . (dot) which is cumbersome to work with in downstream analyses as many tools do not like duplicated markers and/or rely on markernames carrying information. For this reason markernames have been updated from . to rsID if and rsID was found in dbSNP 151. Markers without and rsID were converted from . to chr<CHR>:<POSITION>_<REF>/<ALT> to avoid duplicated markernames.

#### Information score
Since every batch of samples has been QCed and imputed separately, the same marker will be associated with a different information score/imputation quality score (INFO-score) in each batch. The INFO-score in the resulting merged MoBa Genetics datasets is calculated as a weighted average of all included batches. Although the scores are largely consistent across batches, a marker could have a very low info score in one batch but still have a decent weighted average. It is important for researchers to take this into account if they want to filter on INFO-score. They might want to look into each specific batch when deciding on what markers and samples to include in their analysis.

### Overlapping samples
Although NIPH aims at not re-genotyping samples in order to maximize the utilization of precious DNA, some earlier projects have overlapping samples with later projects due to various reasons like technical restrictions in the biobank and in some situations the desire to genotype on a more modern platform. As a consequence, researchers would need to scrutinize the dataset in order to avoid duplicated samples in their analyses.

### Rare variants
Several of the aforementioned projects used arrays targeting rare variants. These variants require a more extensive QC in order to achieve sufficient quality. These protocols often necessitate a lot of manual labour assessing cluster plots to avoid dodgy calls. Due to the many sub-projects multiplying the amount of plots that would need manual inspection, rare variants QC was outside the scope of our QC. Analyses performed on robust phenotypes on markers with MAF > 0.5% have shown satisfactory reproducibility of previously established loci. We highly recommend additional QC if analysts would like to investigate rare variants.

### How to obtain access
The datasets will be available in TSD in the shared folder between project p229 and the respective subprojects. If you have access to the shared folder the data should be available to you. If your project does not have access please review the information and apply as described at the NIPH MoBa Genetics website:
[https://www.fhi.no/en/op/data-access-from-health-registries-health-studies-and-biobanks/data-from-moba/genetic-data-from-the-norwegian-mother-and-child-cohort-study-mobagenetics/](https://www.fhi.no/en/op/data-access-from-health-registries-health-studies-and-biobanks/data-from-moba/genetic-data-from-the-norwegian-mother-and-child-cohort-study-mobagenetics/)

### How to acknowledge the use of the data
The Norwegian Mother, Father and Child Cohort Study is today one of the world's largest genotyped triad cohorts. A lot of resources at the Norwegian Institute of Public Health, and in the research groups that funded genotyping and spent a lot of time preparing the data, has been crucial for now allowing general access to this resource for all researchers. Please acknowledge their efforts by following the guidelines at the NIPH webpage [https://www.fhi.no/en/studies/moba/for-forskere-artikler/checklist-for-papers/](https://www.fhi.no/en/studies/moba/for-forskere-artikler/checklist-for-papers/)

