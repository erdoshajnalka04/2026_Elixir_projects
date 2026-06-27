# 2026_Elixir_projects

During the summer of 2026 I participated in an ELIXIR bioinformatics course at the University of Pécs.  
Majority of the code in this directory was written by István Albert and used as part of the course exercises. However, this repository also contains code that I have written independently (*Aeromonas* and *Pseudmonas* genome analysis).

To reproduce the results, the following tasks were completed:
- The terminal was used throughout the project to download datasets, run the Makefile-based workflow, execute HISAT2, indexing and alignment commands, manage files and directories, and version-control the project using Git.
- For the bioinformatic environment setup we used pixi
- Two bioinformatic environment were used; bioinfo (pixi add make bwa samtools bcftools trimmomatic sra-tools fastqc) and stats (-optparse r-tibble r-dplyr r-gplots r-pacman  python\ bioconductor-proper bioconductor-biomart bioconductor-edger \ bioconductor-deseq2 bioconductor-tximport)
- All the examples were The Biostar Handbook (2nd Edition)


