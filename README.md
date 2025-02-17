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

# transpose
$ awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt

# Sort by first column of transposed_maize_genotypes.txt
$ sort -k1,1 transposed_maize_genotypes.txt > sorted_transposed_maize_genotypes.txt

# Sort by first column of snp_position.txt
$ sort -k1,1 snp_position.txt > sorted_snp_position.txt
```
### Maize Data code description
1. Pick out Group = ZMMIL, ZMMLR, and ZMMMR from `fang_et_al_genotypes.txt` to `maize_genotypes.txt` with first line (information line)
2. Transpose
3. Sort by first column of transposed_maize_genotypes.txt
4. Sort by first column of snp_position.txt
### Teosinte Data

```
# pick out Group = ZMPBA, ZMPIL, and ZMPJA from `fang_et_al_genotypes.txt` to `teosinte_genotypes.txt` with first line (information line)
(head -n 1 fang_et_al_genotypes.txt && grep -E 'ZMPBA|ZMPIL|ZMPJA' fang_et_al_genotypes.txt) > teosinte_genotypes.txt
```
### Teosinte Data code desctripion
Pick out Group = ZMPBA, ZMPIL, and ZMPJA from `fang_et_al_genotypes.txt` to `teosinte_genotypes.txt` with first line (information line)


