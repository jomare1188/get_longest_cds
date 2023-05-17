# Project documentation for Longest CDS per Orthogroup

This project is designed to find the longest coding sequence (CDS) for each orthogroup from a set of fasta sequences.

## Requirements

- Python 3.x
- Biopython (Python module)

## Input files

- `longest_cds.fa`: Fasta file containing the coding sequences: "/Storage/data1/jorge.munoz/GET_CDS/new_all_CDS.fa"
- `Orthogroups.tsv`: Orthofinder generated file (inflation value = 6): "/Storage/data1/jorge.munoz/GET_CDS/Orthogroups.tsv"

## Workflow

First you have to reformat Orthogroups.tsv to fet a tab separated file with orthogroups in first column and protein ID in second column, you can do it by using reformatOrthoFinderGroups.pl

```sh
reformatOrthoFinderGroups.pl --infile Orthogroups.tsv
```
it will create Orthogroups.tbl that should look like this

| Orthologue Group ID | Species            | ProteinID                    |
|--------------------|--------------------|------------------------------|
| OG0000000          | B1_PEP.fix.nr100   | B1_k25_TRINITY_DN1487_c0_g1_i1 |
| OG0000000          | B1_PEP.fix.nr100   | B1_k25_TRINITY_DN1487_c0_g1_i7 |
| OG0000000          | B1_PEP.fix.nr100   | B1_k25_TRINITY_DN1526_c0_g1_i1 |
| OG0000000          | B1_PEP.fix.nr100   | B1_k25_TRINITY_DN1526_c0_g1_i2 |

We only need first and third column, we extract them using some bash

```sh
cut -f1,3 Orthogroups.tbl > Orthogroups_for_longest_cds.tsv
```
In this step maybe is possible that IDs in CDS file are not the same as in the Orthogroups_for_longest_cds.tsv so you need to fix it.
here we provide fix_names.sh with some regex who might be useful to you.

Later we need to measure the lenghts of each sequence in Orthogroups_for_longest_cds.tsv using get_lenghts.py:
you need to specify in the scripts the paths to the file with cds sequences, to the  reformated Orthofinder.tsv file (Orthogroups_for_longest_cds.tsv) and to the output file (Orthogroups_with_lenghts.tsv)

```sh
python get_lenghts.py
```
The file should contain in the first column the Orthogroup, in the second column the protein id and in the third column the protein lenght

| Orthologue Group ID| Protein-ID                                 | Protein lenght                          |
|--------------------|--------------------------------------------|-----------------------------------------|
| OG0634775          | US851008_k31_TRINITY_DN36197_c0_g1_i1      | 383                                     |
| OG0634775          | QC02-402_k25_TRINITY_DN5964_c0_g1_i5       | 455                                     |
| OG0361999          | QBYN04-26041_k31_TRINITY_DN1071_c0_g1_i20  | 5075                                    |
| OG0361999          | KQB07-24619_k25_TRINITY_DN319_c0_g1_i12    | 5078                                    |
| OG0361999          | KQB07-24619_k25_TRINITY_DN319_c0_g1_i1     | 5450                                    |


Maybe if there is some sequence missing you should exclude NAs

```sh
grep -v -w NA Orthogroups_with_lenghts.tsv > no_NA_Orthogroups_with_lenghts.tsv
```

Then we look for the largest protein for each orthogroup using get_longest_transcript.py, you need to configure the name of the orthogrups file with lenghts in the file.

```sh
python get_longest_transcript.py > longest_cds_per_orthogroup.tsv 
```
longest_cds_per_orthogroup.tsv should look like this 

It should create a file like this

| Orthologue Group ID | Sequence Length | Protein ID                              |
|--------------------|-----------------|-----------------------------------------|
| OG0000000          | 2148            | US851008_k31_TRINITY_DN1636_c0_g1_i4    |
| OG0000001          | 1155            | GXU-34176_k31_TRINITY_DN1184_c1_g1_i1   |
| OG0000002          | 3018            | QS99-2014_k31_TRINITY_DN1107_c0_g1_i8   |
| OG0000003          | 2616            | ROC26_k25_TRINITY_DN999_c0_g1_i8        |

finally we extract the fasta sequences present in orthogroups_with_lengths.tsv from the CDS file
if your cds file is a single-line fasta you can use:

```sh
grep -A 1 -f <(cut -f 2 longest_cds_per_orthogroup.tsv) all_cds.fasta > longest_cds.fa
```

