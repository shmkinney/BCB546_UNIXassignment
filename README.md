UNIX Assignment


Part 1: Data Inspection

file: fang_et_al_genotypes.txt

Using code [head] gives large chunk of text, with Sample_ID at the top, followed by PZA values, followed by SNP genotypes

Using code [tail] gives SNP genotypes

Using code [wc] gives 2783  2744038 11051939, for lines, words, characters

Using code [du -h] gives 6152

Using code [awk -F "\t" '{print NF; exit}'] gives 986 


The file is large with lots of SNP and genotype data. It has 2783 lines, uses 6152 in disc space, and has 986 columns

file: snp_position.txt

Using code [head] gives several columns of infomation, including SNP_ID, Chromosome, Position, and gene

Using code [tail] gives data that correspond to previously identified columns

Using code [wc] gives 984 13198 82763, for lines, words, characters

Using code [du -h] gives 38K 

Using code [awk -F "\t" '{print NF; exit}'] gives 15


The file has lots of different information contained in columns. There are 984 lines, 15 columns, and it uses 38K in disc space.



Part 2: Data Processing
