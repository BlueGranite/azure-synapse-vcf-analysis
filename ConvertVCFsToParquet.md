# Convert VCFs to Parquet Format

Notes:
- The following PySpark code uses the Glow library, which can be installed using the following instructions: https://glow.readthedocs.io/en/latest/getting-started.html
- This code was written on Azure Databricks, though it can be run on other Apache Spark clusters (like Azure Synapse). Simply modify the file paths to fit your environment.

## Load in the Glow Package (and other functions)
```py
import glow
glow.register(spark)
import os
from pyspark.sql.functions import *
```

## Convert a Single VCF
```py
## Define input VCF path
input_vcf_path = "/mnt/1000genomes/phase3_vcfs/ALL.chr1.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz"

## Define output Parquet directory
output_parquet_path = "/mnt/1000genomes/phase3_parquets/ALL.chr1.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.parquet"

## Read in VCF file as a Spark DataFrame
vcf_df = spark.read.format("vcf").load(input_vcf_path)

## (Optional) Calculate Statistics
vcf_df = vcf_df.withColumn("hardyweinberg", expr("hardy_weinberg(genotypes)")) \
               .withColumn("stats", expr("call_summary_stats(genotypes)"))

## Write out Parquet file
vcf_df.write.format("parquet").save(output_parquet_path)
```

## (Optional) Convert Multiple VCFs in a Directory

```py
## Define the conversion function
def convert_vcf_to_parquet(input_vcf_path, output_parquet_path, calculate_stats = True, repartition = True):
  vcf_df = spark.read.format("vcf").load(input_vcf_path)
  
  if (calculate_stats):
    vcf_df = vcf_df.withColumn("hardyweinberg", expr("hardy_weinberg(genotypes)")) \
                   .withColumn("stats", expr("call_summary_stats(genotypes)"))
    
  if (repartition):
    vcf_df.repartition(1).write.format("parquet").save(output_parquet_path)
  else:
    vcf_df.write.format("parquet").save(output_parquet_path)

## Loop through each VCF in a directory and convert
vcfs_path = "/dbfs/mnt/1000genomes/phase3_vcfs/"

for vcf in os.listdir(vcfs_path):
  vcf_path = vcfs_path.replace("/dbfs", "") + vcf
  print(vcf_path)
  
  parquet_path = vcf_path.replace("/phase3_vcfs/", "/phase3_parquets/").replace(".vcf.gz", ".parquet")
  print(parquet_path)
  
  convert_vcf_to_parquet(input_vcf_path = vcf_path,
                         output_parquet_path = parquet_path,
                         calculate_stats = True, repartition = False)
```