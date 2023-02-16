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

**First, make sure the common column between both files has the same name; in this case, change Sample_ID to SNP_ID in fang_et_al_genotypes.

sed 's/Sample_ID/SNP_ID/' fang_et_al_genotypes.txt > fang_gen.txt

**Next, separate out the maize and teosinte data.

(head -n 1 fang_gen.txt && grep -E "ZMMIL|ZMMLR|ZMMMR" fang_gen.txt) > mzgen.txt

(head -n 1 fang_gen.txt && grep -E "ZMPBA|ZMPIL|ZMPJA" fang_gen.txt) > tsgen.txt

**Transpose the files.

awk -f transpose.awk mzgen.txt > maize.txt

awk -f transpose.awk tsgen.txt > teosin.txt

**Sort all the files on the SNP_ID column.

(head -n 1 maize.txt && tail -n +2 maize.txt | sort -k1,1 ) > sort_maize.txt

(head -n 1 teosin.txt && tail -n +2 teosin.txt | sort -k1,1 ) > sort_teosin.txt

(head -n 1 snp_position.txt && tail -n +2 snp_position.txt | sort -k1,1 ) > sort_snp.txt

**Cut only desired columns from the snp file.

cut -f 1,3,4 sort_snp.txt > cut_snp.txt

**Join together the maize/teosinte and snp files

join -1 1 -2 1 -a 2 -t $'\t' cut_snp.txt sort_maize.txt > comaize.txt

join -1 1 -2 1 -a 2 -t $'\t' cut_snp.txt sort_teosin.txt > coteosin.txt

**Separate data out by chromosome; this gives completed files for multiple and unknown SNP locations, labeled starting with m or t for maize or teosinte, chr for chromosome, and multiple or unknown for location.

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

**Sort chromosome data by increasing position. This gives completed files for increasing position and missing data is encoded by "?"; files are labeled with m or t for maize or teosinte, i for increasing, and chr{number} for specific chromosome number.

sort -k3,3n mchr1.txt > michr1.txt

sort -k3,3n mchr2.txt > michr2.txt

sort -k3,3n mchr3.txt > michr3.txt

sort -k3,3n mchr4.txt > michr4.txt

sort -k3,3n mchr5.txt > michr5.txt

sort -k3,3n mchr6.txt > michr6.txt

sort -k3,3n mchr7.txt > michr7.txt

sort -k3,3n mchr8.txt > michr8.txt

sort -k3,3n mchr9.txt > michr9.txt

sort -k3,3n mchr10.txt > michr10.txt

sort -k3,3n tchr1.txt > tichr1.txt

sort -k3,3n tchr2.txt > tichr2.txt

sort -k3,3n tchr3.txt > tichr3.txt

sort -k3,3n tchr4.txt > tichr4.txt

sort -k3,3n tchr5.txt > tichr5.txt

sort -k3,3n tchr6.txt > tichr6.txt

sort -k3,3n tchr7.txt > tichr7.txt

sort -k3,3n tchr8.txt > tichr8.txt

sort -k3,3n tchr9.txt > tichr9.txt

sort -k3,3n tchr10.txt > tichr10.txt

**Sort chromosome data by decreasing position, while missing data is replaced by the character "-". This gives completed files for decreasing position and missing data is encoded by "-"; files are labeled with m or t for maize or teosinte, d for decreasing, and chr{number} for specific chromosome number.

sed 's/?/-/g' mchr1.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr1.txt

sed 's/?/-/g' mchr2.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr2.txt

sed 's/?/-/g' mchr3.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr3.txt

sed 's/?/-/g' mchr4.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr4.txt

sed 's/?/-/g' mchr5.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr5.txt

sed 's/?/-/g' mchr6.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr6.txt

sed 's/?/-/g' mchr7.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr7.txt

sed 's/?/-/g' mchr8.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr8.txt

sed 's/?/-/g' mchr9.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr9.txt

sed 's/?/-/g' mchr10.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > mdchr10.txt

sed 's/?/-/g' tchr1.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr1.txt

sed 's/?/-/g' tchr2.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr2.txt

sed 's/?/-/g' tchr3.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr3.txt

sed 's/?/-/g' tchr4.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr4.txt

sed 's/?/-/g' tchr5.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr5.txt

sed 's/?/-/g' tchr6.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr6.txt

sed 's/?/-/g' tchr7.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr7.txt

sed 's/?/-/g' tchr8.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr8.txt

sed 's/?/-/g' tchr9.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr9.txt

sed 's/?/-/g' tchr10.txt | (head -n 1 && tail -n +2 | sort -k3,3nr) > tdchr10.txt


###End
