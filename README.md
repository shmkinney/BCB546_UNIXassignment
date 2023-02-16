# UNIX Assignment

_

## Part 1: Data Inspection

file: fang_et_al_genotypes.txt

Using code [head] gives large chunk of text, with Sample_ID at the top, followed by PZA values, followed by SNP genotypes

Using code [tail] gives SNP genotypes

Using code [wc] gives 2783  2744038 11051939, for lines, words, characters

Using code [du -h] gives 6152

Using code [awk -F "\t" '{print NF; exit}'] gives 986 

Using module bioawk, and using code [file] gives ASCII text, with very long lines


The file is large with lots of SNP and genotype data. It has 2783 lines, uses 6152 in disc space, and has 986 columns. It has only ASCII characters.

_

file: snp_position.txt

Using code [head] gives several columns of infomation, including SNP_ID, Chromosome, Position, and gene

Using code [tail] gives data that correspond to previously identified columns

Using code [wc] gives 984 13198 82763, for lines, words, characters

Using code [du -h] gives 38K 

Using code [awk -F "\t" '{print NF; exit}'] gives 15

Using module bioawk, and using code [file] gives ASCII text


The file has lots of different information contained in columns. There are 984 lines, 15 columns, and it uses 38K in disc space. It has only ASCII characters.

_

## Part 2: Data Processing

_

First, transpose fang_et_al_genotypes.txt using [awk -f transpose.awk fang_et_al_genotypes.txt > transposed_genotypes.txt].

(I changed file names for ease of use: snp_position.txt -> snp.txt; transposed_genotypes.txt -> tg.txt)

Replace {Sample_ID} in the transposed_genotypes.txt file with {SNP_ID} using [sed 's/Sample_ID/SNP_ID/' tg.txt > gen.txt].

Second, sort both files using [sort]; [(head -n 1 snp.txt && tail -n +2 snp.txt | sort -k1,1 ) > sort_snp.txt] and [(head -n 1 gen.txt && tail -n +2 gen.txt | sort -k1,1 ) > sort_gen.txt].

Next, join the two sorted files using [join -1 1 -2 1 -a 2 -t $'\t' sort_snp.txt sort_gen.txt > total.txt], which will keep unmatched data and give a tab-delimited file.

Check that the join merged correctly using [wc total.txt]; received  986  2756252 11123868 total.txt. Therefore there are 986 lines in the file, which matches the column number in the original fang_et_al_genotypes.txt file.

Check the column number in order to verify the tab-delimited nature of the file using [awk -F "\t" '{print NF; exit}' total.txt]; this gives 2797 columns, so the tab-delimiting worked.


## New Part 2

_

CURRENT PROGRESS


sed 's/Sample_ID/SNP_ID/' fang_et_al_genotypes.txt > fang_gen.txt

(head -n 1 fang_gen.txt && grep -E "ZMMIL|ZMMLR|ZMMMR" fang_gen.txt) > mzgen.txt

(head -n 1 fang_gen.txt && grep -E "ZMPBA|ZMPIL|ZMPJA" fang_gen.txt) > tsgen.txt

awk -f transpose.awk mzgen.txt > maize.txt

awk -f transpose.awk tsgen.txt > teosin.txt

(head -n 1 maize.txt && tail -n +2 maize.txt | sort -k1,1 ) > sort_maize.txt

(head -n 1 teosin.txt && tail -n +2 teosin.txt | sort -k1,1 ) > sort_teosin.txt

(head -n 1 snp_position.txt && tail -n +2 snp_position.txt | sort -k1,1 ) > sort_snp.txt

cut -f 1,3,4 sort_snp.txt > cut_snp.txt

join -1 1 -2 1 -a 2 -t $'\t' cut_snp.txt sort_maize.txt > comaize.txt

join -1 1 -2 1 -a 2 -t $'\t' cut_snp.txt sort_teosin.txt > coteosin.txt

awk '$2~/^1$|Chromosome/' comaize.txt > mchr1.txt

awk '$2~/^2$|Chromosome/' comaize.txt > mchr2.txt

awk '$2~/^3$|Chromosome/' comaize.txt > mchr3.txt

awk '$2~/^4$|Chromosome/' comaize.txt > mchr4.txt

awk '$2~/^5$|Chromosome/' comaize.txt > mchr5.txt

awk '$2~/^6$|Chromosome/' comaize.txt > mchr6.txt

awk '$2~/^7$|Chromosome/' comaize.txt > mchr7.txt

awk '$2~/^8$|Chromosome/' comaize.txt > mchr8.txt

awk '$2~/^9$|Chromosome/' comaize.txt > mchr9.txt

awk '$2~/^10$|Chromosome/' comaize.txt > mchr10.txt

awk '$2~/^multiple$|Chromosome/' comaize.txt > mchrmultiple.txt

awk '$2~/^unknown$|Chromosome/' comaize.txt > mchrunknown.txt

awk '$2~/^1$|Chromosome/' coteosin.txt > tchr1.txt

awk '$2~/^2$|Chromosome/' coteosin.txt > tchr2.txt

awk '$2~/^3$|Chromosome/' coteosin.txt > tchr3.txt

awk '$2~/^4$|Chromosome/' coteosin.txt > tchr4.txt

awk '$2~/^5$|Chromosome/' coteosin.txt > tchr5.txt

awk '$2~/^6$|Chromosome/' coteosin.txt > tchr6.txt

awk '$2~/^7$|Chromosome/' coteosin.txt > tchr7.txt

awk '$2~/^8$|Chromosome/' coteosin.txt > tchr8.txt

awk '$2~/^9$|Chromosome/' coteosin.txt > tchr9.txt

awk '$2~/^10$|Chromosome/' coteosin.txt > tchr10.txt

awk '$2~/^multiple$|Chromosome/' coteosin.txt > tchrmultiple.txt

awk '$2~/^unknown$|Chromosome/' coteosin.txt > tchrunknown.txt




