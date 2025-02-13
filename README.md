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
here is my snippet of code used for data processing
```

Here is my brief description of what this code does


### Teosinte Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does
