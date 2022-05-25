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
