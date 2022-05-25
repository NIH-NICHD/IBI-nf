## Approach

* Merged file is ~816 GB vcf.gz
* We would like to split it chromosome by chromosome
* Then it will be much more easier to work on
* It can be further filtered out based on ‘MAF’ or ‘Geno calling rate’ etc.
* Plink or SeqArray (we can do that)
* Any suggestion for using other tool?
* Further we will convert it to dominat/recessive coding
* Apply IBI algorithm to find novel variants
* Apply GWAS
* Comparison between IBI vs GWAS
* Further post-process the IBI result


## Step 1

```bash
GATK – sention powered workflow
Cohort paired fastq WGS files CHD
Merged file is ~816 GB vcf.gz
```

## Step 2

```bash
Process: We would like to split it chromosome by chromosome
Options include using bcftools in a bash script – which can be done on a large enough machine using Jupyter Lab Notebook in a terminalI have containerized samtools, my picard filterSamReads shows how to use samtools in a process. into a docker which we can use in a process or you can interactively do this – but need a large machine – advantage of wrapping it in a workflow is that you have it in hand
bcftools index -s in.vcf.gz | cut -f 1 | while read C; do bcftools view -O z -o split.${C}.vcf.gz in.vcf.gz "${C}" ; done
Input:     merged vcf.gz
Output: chr1.vcf.gz, … chr22.vcf.gz, etc
```

## Step 3

```bash
Process: It can be further filtered out based on ‘MAF’ or ‘Geno calling rate’ etc.
Can be encapsulated from Anaconda install – made as part of a workflow.  Plink or SeqArray (we can do that)
Any suggestion for using other tool? (from Anne, I'll have a look)
 Input: Each separated Chromosome.vcf.gz file
 Output: filtered MAF or Genotype calling rate or both (output from Step 3). It will be matrix with reduced columns (variants) and rows (samples). So mostly variants will be filtered as well as if need few samples will be filtered out to. So it will form a reduce matrix dimension.
Plink directly give that output. Seqarray also has ability to produce that output. 
```

For this step, we can install the tool using Anaconda we find we can install [plink from Anaconda using the bioconda channel](https://anaconda.org/bioconda/plink) -- looking here.

This conda install can be encapsulated
```
conda install -c bioconda plink
```

The command run we are testing with is:
```
plink --vcf vcf21_plink.txt --noweb --geno 0.03 --maf 0.01 --hwe 0.000001 --me 0.05 0.1 --out vcf21_plink_output --recode A-transpose
```

## Step 4

```bash
Further we will convert it to dominat/recessive coding
Process: We will load the data into python script, fill the null value and converted into dominant or recessive coding replacing the values.
Input: Reduced Matrix file
Output: Converted dominant coded file ready for ICI and GWAS
```

## Step 5

```bash
Process:  Apply IBI algorithm to find novel variants  Github Link : https://github.com/asadcfc/IBI  Next slide: Process is described through an image
Input:  Converted dominant coded file 
Output: Variants with Log Marginal Likelihood values from ICI algorithm and other statistics
```

## Step 6 

```bash
Process: Apply GWAS algorithm. Logistic regression association testing can be done using R script and glm function. If we have gds file we can run genesis package to do association testing. Alternatively, Fisher exact test in python using scipy library can be done also. It currently depends on the dominant coded dataset, file type and covariates.
Input:     Converted dominant coded file 
Output: Variants with pvalue, beta coefficients, SE and other statistics
```

## Step 7

```bash
Process: Comparison between IBI vs GWAS. It is more like deep dive data analysis. It includes violin plot for MAF of top varaints by ICI and GWAS, overlapped variants from top variants, Prediction by both GWAS and ICI, Information gain by both ICI and GWAS, Manhattan plot and others. Currently we don’t have one fixed file for that, we have multiple file for doing analysis, also we add or remove analysis tasks based on results.
 Input: Output result file from GWAS and ICI
 Output: Visualization (plot image files), small lists and tables.
 ```

## Step 8

```bash
Further post-process the ICI result
Process: Annotate result ( such as using SNPnexus or VEP). Based on annotated columns, further find the novel variants for ICI and also compare with GWAS result. For this process we also add or remove tasks based on annotated values
Input: Output result file from ICI 
Output: Small list or table of variants
```
