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

**Process:** GATK – sention powered workflow

**Input:** Cohort paired fastq WGS files CHD

**Output:** Merged file is ~816 GB vcf.gz

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
## Step 4

```bash
Further we will convert it to dominat/recessive coding
Process: We will load the data into python script, fill the null value and converted into dominant or recessive coding replacing the values.
Input: Reduced Matrix file
Output: Converted dominant coded file ready for ICI and GWAS
```
