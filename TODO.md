###Task 1
- Implement a tool that starts with an RNA-seq BAM and produces an annotated junctions file
 
- Start with an RNA-seq BAM and produce a junctions.bed file (use code from TopHat)

- Then reimplement the functionality of this GMT in the GMS:

genome/lib/perl/Genome/Model/Tools/Transcriptome/SpliceJunctionSummary.pm

- Test case to be reproduced (from within a TGI system where the GMS is installed):

```bash  
git clone git@github.com:griffithlab/regtools.git
cd regtools/tests
wget https://xfer.genome.wustl.edu/gxfer1/project/gms/testdata/bams/brain_vs_uhr_w_ercc/downsampled_5pc_chr22/chr22.fa.gz
wget https://xfer.genome.wustl.edu/gxfer1/project/gms/testdata/bams/brain_vs_uhr_w_ercc/downsampled_5pc_chr22/genes_chr22.gtf.gz
wget https://xfer.genome.wustl.edu/gxfer1/project/gms/testdata/regtools/test.rnaseq.bam*
gunzip *.gz
rm -fr /tmp/junction_summary/
mkdir /tmp/junction_summary/
gmt transcriptome splice-junction-summary --output-directory='/tmp/junction_summary/' --observed-junctions-bed12-file='junctions.chr22.bed' --reference-fasta-file='chr22.fa' --annotation-gtf-file='genes_chr22.gtf' --annotation-name='Ensembl'
cd /tmp/junction_summary/
rm -fr SpliceJunctionSummary.R.stderr summary SpliceJunctionSummary.R.stdout Ensembl.Junction.TranscriptExpression.top1percent.tsv Ensembl.Junction.GeneExpression.top1percent.tsv
```

###MGI datasets for testing regtools analysis
Requirements: WGS (or exome maybe) and RNA-seq on the same sample. Ideally a tumor/normal pair where we have RNA-seq for a matched adjacent normal.

#Look for a model/build to use
genome model rna-seq list --filter subject.name=H_NJ-HCC1395-HCC1395_RNA --show id,processing_profile,last_complete_build.id,last_complete_build.merged_alignment_result.bam_path --noheaders --style tsv | grep -v NULL

**HCC1395 (no matched adjacent normal, but there is a matched blood normal with RNA-seq)**
Normal WGS model: 2891325882
Tumor WGS model: 2891325873
Tumor RNA-seq model: 060145d385274d258569a9fc013e4ada

```
genome model list --filter 'id in [2891325882,2891325873,060145d385274d258569a9fc013e4ada]' --show last_complete_build.merged_alignment_result.bam_path --noheaders | perl -ne 'print "https://gscweb.gsc.wustl.edu$_"'
```

- ALL1 (no matched adjacent normal, but we can use the 'healthy' normals for comparison)
- AML31 (no matched adjacent normal, but we can compare primary and relapse)
- HCC30 (has matched adjacent normal RNA-seq)
- LUC1-20 (has matched adjacent normal RNA-seq)




