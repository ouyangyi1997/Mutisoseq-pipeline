# Mutisoseq-pipeline
Method for gene cloning in plants.
## About
Mutisoseq is a method to clone genes in plants. Prerequisites are that the gene must be related to a specific phenotype ant that EMS mutagenesis is worked well in this species. In a nutshell, you do an EMS mutagenesis screen of your wildtype and look for mutants that have lost the phenotype related to your gene, and genotyping these mutants and the wild type plants, with the illunima sequencing (mutants) and the NGS technologies (WT). Finally, confirm sites that mutated by EMS in these mutants. This method can quickly clone recessive genes. The more details about `Mutisoseq` can read the original [paper](https://doi.org/10.1038/s41588-023-01401-2).There are three parts of this pipeline, 1. Assemble the reference transcriptome by NGS data for WT plants; 2. Mapping these reads from mutants sequenced by illumina sequencing data; 3. Confirm the interested sites.

### Workflow of Mutisoseq
``` mermaid
graph TD
    subgraph Part1 ["Assembly"]
        A[NGS data of WT .fasta Files] -->|spades -> assembly| B[Reference transcriptome]
    end
    
    subgraph Part2 ["SNP Detection"]
        B -->|Used as reference| C1[Mapping reference for mutants]
        C[Raw data of mutants] --> C1 -->|bbmap -> mapping| D[.sam Files]
        D -->|samtools -> Indexing and Sorting| E[.sorted.sam Files]
        E -->|samtools -> SNP Calling| F[SNP Files .pileup.txt]
    end
    
    subgraph Part3 ["Mutation Site Confirmation"]
        F -->|Pileup2XML.jar -> Format transfer| G[.xml Files]
        G -->|MutChromSeq.jar -> Sites confirmation| H[Mutation Sites]
    end
```

## Install

With the `.yml` file install by `conda` can quickly deploy the `Mutisoseq` environment. 

````bash
conda env create -f mutisoseq.yml
conda activate mutisoseq
````
We also need [`Pileup2XML.jar, MutChromSeq.jar`](https://github.com/steuernb/MutChromSeq/tree/master) to confirm the mutation sites. Download them from github directly in your `mutisoseq` environment folder.

```bash
wget https://github.com/steuernb/MutChromSeq/blob/master/src/mutantHunter/Pileup2XML.java
wget https://github.com/steuernb/MutChromSeq/blob/master/src/mutantHunter/MutChromSeq.java
```

## Preprocessing  

### Assembly  
In the `mutisoseq.yml` file we choose the `SPADes` as the assembler for NGS data of WT. You can change the assembler, it's up to you. The parameter of `SPAdes` can read the file in [SPAdes](https://github.com/ablab/spades).

### 



