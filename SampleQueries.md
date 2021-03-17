# Sample VCF Queries

## EXAMPLE: Count of Variants by Type

```sql
SELECT  JSON_VALUE(INFO_VT, '$[0]') AS VariantType
        ,COUNT(DISTINCT(names)) AS VariantCount
FROM    phase3_variants
GROUP BY JSON_VALUE(INFO_VT, '$[0]')
```

## EXAMPLE: Rare Structural Variants

```sql
SELECT  TOP 100 *
FROM    phase3_variants

WHERE   JSON_VALUE(INFO_VT, '$[0]') = 'SV'                      --Structural variants
AND     qual > 95                                               --High quality
AND     alternateAlleles LIKE '%ALU%'                           --Transposable elements
AND     JSON_VALUE(hardyWeinberg, '$.hetFreqHwe') >= 0.05       --Higher heterozygous frequency
AND     JSON_VALUE(stats, '$.alleleFrequencies[1]') <= 0.05     --Rare minor alleles (variants)
```

## EXAMPLE: Common Indels as Alternate Allele (Multiallelic)

```sql
SELECT  TOP 100 *
FROM    phase3_variants

WHERE   JSON_VALUE(INFO_VT, '$[0]') = 'INDEL'                   --Indels
AND     CAST(JSON_VALUE(INFO_AF, '$[0]') AS float) >= 0.5       --Common alternate alleles
AND     INFO_MULTI_ALLELIC = 'True'                             --Multiallelics
```

## EXAMPLE: Distribution of Indel Size

```sql
SELECT  contigName
        ,LEN(JSON_VALUE(alternateAlleles, '$[0]')) - LEN(referenceAllele) AS InsertionLength
        ,COUNT(DISTINCT(names)) AS VariantCount
FROM    phase3_variants

WHERE   JSON_VALUE(INFO_VT, '$[0]') = 'INDEL'                   --Indels
AND     INFO_MULTI_ALLELIC = 'False'                            --Biallelics Only
GROUP BY contigName,
         LEN(JSON_VALUE(alternateAlleles, '$[0]')) - LEN(referenceAllele)
ORDER BY contigName,
         LEN(JSON_VALUE(alternateAlleles, '$[0]')) - LEN(referenceAllele) DESC
```

## EXAMPLE: Search for Motif Occurences in Insertions

```sql
DECLARE @motif varchar(1000)
SET     @motif = 'TA'

SELECT  contigName
        ,[start]
        ,[end]
        ,names
        ,referenceAllele
        ,JSON_VALUE(alternateAlleles, '$[0]') AS alternateAllele
        ,(LEN(JSON_VALUE(alternateAlleles, '$[0]')) - LEN(REPLACE(JSON_VALUE(alternateAlleles, '$[0]'), @motif, ''))) / LEN(@motif) AS MotifMatches
FROM    phase3_variants

WHERE   JSON_VALUE(INFO_VT, '$[0]') = 'INDEL'                   --Indels
AND     INFO_MULTI_ALLELIC = 'False'                            --Biallelics Only
AND     JSON_VALUE(alternateAlleles, '$[0]') LIKE '%' + @motif + '%'      --TA Matches
```