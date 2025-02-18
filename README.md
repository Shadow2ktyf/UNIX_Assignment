# UNIX Assignment

## Data Inspection

### Attributes of `fang_et_al_genotypes`

```
$ head fang_et_al genotypes.txt
$ file fang_et_al_genotypes.txt
$ wc fang_et_al_genotypes.txt
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
$ cut -f1 fang_et_al_genotypes.txt | column -t |head
```

By inspecting this file I learned that:

1. `fang_et_al_genotypes.txt` is ASCII text with very long lines
2. `fang_et_al_genotypes.txt` is a file with `2783` lines, `2744038` words, and `11051939` bytes
3. There are `986` columns in `fang_et_al_genotypes.txt`
4. From reading the first column `Sample_ID` to confirm the delimiter type as `Tab`



### Attributes of `snp_position.txt`

```
$ head snp_position.txt
$ file snp_position.txt
$ wc snp_position.txt
$ awk -F "\t" '{print NF; exit}' snp_position.txt

```

By inspecting this file I learned that:

1. `snp_position.txt` is a text file in ASCII format
2. `snp_position.txt` is a file with `984` lines, `13198` words, and `82763` bytes
3. There are `15` columns in `snp_position.txt`


## Data Processing

### Maize Data

```
# pick out Group = ZMMIL, ZMMLR, and ZMMMR from `fang_et_al_genotypes.txt` to `maize_genotypes.txt` with first line (information line)
(head -n 1 fang_et_al_genotypes.txt && grep -E 'ZMMIL|ZMMLR|ZMMMR' fang_et_al_genotypes.txt) > maize_genotypes.txt

# Transpose for maize groups
$ awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt

# Sort by first column of transposed_maize_genotypes.txt
$ sort -k1,1 transposed_maize_genotypes.txt > sorted_transposed_maize_genotypes.txt

# Sort by first column of snp_position.txt
$ sort -k1,1 snp_position.txt > sorted_snp_position.txt

# Join sorted_transposed_maize_genotypes.txt AND sorted_snp_position.txt
$ join -1 1 -2 1 -a 1 -a 2 -t $'\t' sorted_snp_position.txt sorted_transposed_maize_genotypes.txt> joint_transposed_maize_genotypes_snp_position.txt

# 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
$ for i in {1..10}; do
> awk -v val=$i '$3 == val || $0 ~ /Group/ || $0 ~ /Sample_ID/ || $0 ~ /SNP_ID/ || $0 ~ /JG_OTU/' joint_transposed_maize_genotypes_snp_position.txt | sort -k3,3r -k4,4n > maize_data/maize_chrom${i}_increase_sorting.txt;
> done

# 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
$ for i in {1..10}; do
> awk -v val=$i '$3 == val || $0 ~ /Group/ || $0 ~ /Sample_ID/ || $0 ~ /SNP_ID/ || $0 ~ /JG_OTU/' joint_transposed_maize_genotypes_snp_position.txt | sort -k3,3r -> k4,4nr | sed 's|\?/?|-/-|g' > maize_data/maize_chrom${i}_decrease_sorting.txt;
> done

# 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
$ for i in {1..10}; do
> grep 'multiple' maize_chrom${i}_increase_sorting.txt >> maize_multiple.txt
> done

# 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)
$ awk '$4 == "unknown"' joint_transposed_maize_genotypes_snp_position.txt > maize_data/maize_unknown_position.txt
```
### Maize Data code description
1. Pick out Group = ZMMIL, ZMMLR, and ZMMMR from `fang_et_al_genotypes.txt` to `maize_genotypes.txt` with the first line (information line)
2. Transpose
3. Sort by first column of `transposed_maize_genotypes.txt`
4. Sort by first column of `snp_position.txt`
5. Join two big file
6. 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
7. 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
8. 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
9. 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)
### Teosinte Data

```
# Pick out Group = ZMPBA, ZMPIL, and ZMPJA from `fang_et_al_genotypes.txt` to `teosinte_genotypes.txt` with first line (information line)
$ (head -n 1 fang_et_al_genotypes.txt && grep -E 'ZMPBA|ZMPIL|ZMPJA' fang_et_al_genotypes.txt) > teosinte_genotypes.txt

# Transpose for teosinte groups
$ awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt

# Sort by first column of transposed_teosinte_genotypes.txt
$ sort -k1,1 transposed_teosinte_genotypes.txt > sorted_transposed_teosinte_genotypes.txt

# Join sorted_transposed_teosinte_genotypes.txt AND sorted_snp_position.txt
$ join -1 1 -2 1 -a 1 -a 2 -t $'\t' sorted_snp_position.txt sorted_transposed_teosinte_genotypes.txt> joint_transposed_teosinte_genotypes_snp_position.txt

# 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
$ for i in {1..10}; do
> awk -v val=$i '$3 == val || $0 ~ /Group/ || $0 ~ /Sample_ID/ || $0 ~ /SNP_ID/ || $0 ~ /JG_OTU/' joint_transposed_teosinte_genotypes_snp_position.txt | sort
-k3,3r -k4,4n > teosinte_data/teosinte_chrom${i}_increase_sorting.txt;
> done

# 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
$ for i in {1..10}; do
> awk -v val=$i '$3 == val || $0 ~ /Group/ || $0 ~ /Sample_ID/ || $0 ~ /SNP_ID/ || $0 ~ /JG_OTU/' joint_transposed_teosinte_genotypes_snp_position.txt | sort -k3,3r -k4,4nr | sed 's|\?/?|-/-|g' > teosinte_data/teosinte_chrom${i}_decrease_sorting.txt;
> done

# 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
$ for i in {1..10}; do
> grep 'multiple' teosinte_chrom${i}_increase_sorting.txt >> teosinte_multiple_position.txt
> done

# 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)
$ awk '$4 == "unknown"' joint_transposed_teosinte_genotypes_snp_position.txt > teosinte_data/teosinte_unknown_position.txt

```
### Teosinte Data code desctripion
1. Pick out Group = ZMPBA, ZMPIL, and ZMPJA from `fang_et_al_genotypes.txt` to `teosinte_genotypes.txt` with first line (information line)
2. Transpose for teosinte groups
3. Sort by first column of transposed_teosinte_genotypes.txt
4. Join sorted_transposed_teosinte_genotypes.txt AND sorted_snp_position.txt
5. 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
6. 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
7. 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
8. 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)

