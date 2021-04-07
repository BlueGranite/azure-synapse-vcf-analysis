# VCF Analysis in Azure Synapse
Sample code for analyzing VCF files in Azure Synapse (once converted to Parquet using Glow).

<p align="right"><img src="https://raw.githubusercontent.com/BlueGranite/azure-synapse-vcf-analysis/master/img/bg_logo.png" width="200px"></p>

<h3 align=right>Colby T. Ford, Ph.D.</h3>

## Pipeline
<img src="https://raw.githubusercontent.com/BlueGranite/azure-synapse-vcf-analysis/main/img/pipeline.png">

## Sample Code
- Convert VCF files to Parquet: [ConvertVCFsToParquet.md](ConvertVCFsToParquet.md)
- Create External Table to VCF-based Parquet Files in Azure Synapse: [CreateVCFTable.md](CreateVCFTable.md)
- Sample SQL Queries: [SampleQueries.md](SampleQueries.md)


<img src="https://raw.githubusercontent.com/BlueGranite/azure-synapse-vcf-analysis/main/img/synapse.png">

## Sample Data
The sample VCF data used in this demo is from the Phase 3 release of the [1000 Genomes Project](https://www.internationalgenome.org/data/).
This includes ~168GB of data in VCFs, which can be downloaded from their [FTP site](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/).

## BlueGranite Resources
- This repository accompanies the BlueGranite blog post: https://www.bluegranite.com/blog/query-millions-of-genomic-variants-in-minutes-with-azure-synapse
- Demo video on YouTube: [https://www.youtube.com/watch?v=4B-8cviFPYU](https://www.youtube.com/watch?v=4B-8cviFPYU)
- _Building a Genomics Data Lake in Azure_ eBook: https://www.bluegranite.com/genomics-data-lake-ebook
- BlueGranite Genomics Page: https://www.bluegranite.com/genomics
