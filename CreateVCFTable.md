# Create Variants Tables from VCF

```sql
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'SynapseParquetFormat') 
	CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] 
	WITH ( FORMAT_TYPE = PARQUET)
GO

IF NOT EXISTS (SELECT * FROM sys.external_data_sources WHERE name = '1000genomes_genomicsdls_dfs_core_windows_net') 
	CREATE EXTERNAL DATA SOURCE [1000genomes_genomicsdls_dfs_core_windows_net] 
	WITH (
		LOCATION   = 'https://genomicsdls.dfs.core.windows.net/1000genomes', 
	)
Go

CREATE EXTERNAL TABLE phase3_variants (
	[contigName] varchar(50),
	[start] bigint,
	[end] bigint,
	[names] varchar(1000),
	[referenceAllele] varchar(1000),
	[alternateAlleles] varchar(1000),
	[qual] float,
	[filters] varchar(100),
	[splitFromMultiAllelic] bit,
	[INFO_MEND] int,
	[INFO_AC] varchar(1000),
	[INFO_CIEND] varchar(1000),
	[INFO_NS] int,
	[INFO_AFR_AF] varchar(1000),
	[INFO_VT] varchar(100),
	[INFO_AN] int,
	[INFO_MULTI_ALLELIC] bit,
	[INFO_SAS_AF] varchar(1000),
	[INFO_CIPOS] varchar(1000),
	[INFO_AA] varchar(1000),
	[INFO_AF] varchar(1000),
	[INFO_EAS_AF] varchar(1000),
	[INFO_AMR_AF] varchar(1000),
	[INFO_DP] int,
	[INFO_SVLEN] varchar(1000),
	[INFO_MLEN] int,
	[INFO_MEINFO] varchar(1000),
	[INFO_IMPRECISE] bit,
	[INFO_CS] varchar(1000),
	[INFO_MC] varchar(1000),
	[INFO_END] int,
	[INFO_MSTART] int,
	[INFO_EUR_AF] varchar(1000),
	[INFO_EX_TARGET] bit,
	[INFO_TSD] varchar(1000),
	[INFO_SVTYPE] varchar(1000),
	--[genotypes] varchar(8000),
	[hardyweinberg] varchar(8000),
	[stats] varchar(8000)
	)
	WITH (
	LOCATION = 'phase3_parquets/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.parquet',
	DATA_SOURCE = [1000genomes_genomicsdls_dfs_core_windows_net],
	FILE_FORMAT = [SynapseParquetFormat]
	)
GO

SELECT TOP 100 * FROM phase3_variants
GO

```