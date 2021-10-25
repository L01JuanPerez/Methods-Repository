# Methods-Repository
Commands for Sp9 Protein 

  Lab 5
```bash 
ncbi-acc-download -F fasta -m protein XP_032240311.1
#Download the protein sequence for protein accession number XP_032240311.1 from the NCBI database. 

blastp -db allprotein.fas -query XP_032240311.1.fa -outfmt 0 -max_hsps 1 -out Sp9.blastp.typical.out
blastp -db allprotein.fas -query XP_032240311.1.fa -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out Sp9.blastp.detail.out
#Use the Blastp tool to find homologs of Sp9 protein sequence.

awk '{if ($6<0.0000000000000000000000000000000000001)print $1 }' Sp9.blastp.detail.out > Sp9.blastp.detail.filtered.out
#Filter the homologs with an e-value of 10E-37 that will result in putative homologs.

muscle -in Sp9.blastp.detail.filtered.fas -out Sp9.blastp.detail.filtered.aligned.fas
#Feed the putative homologs to Muscle, which will aligned their sequences. 

t_coffee -other_pg seq_reformat -in Sp9.blastp.detail.filtered.aligned.fas -output sim
#t_coffe will provide statistical analysis on the aligned sequences, such as their percent identity. 

t_coffee -other_pg seq_reformat -in Sp9.blastp.detail.filtered.aligned.fas -action +rm_gap 50 -out Sp9.blastp.detail.filtered.aligned.r50.fas
#Rerun t_coffee to filter positions that are gapped at 50% or more. 
```

Lab 6
```bash
sed "s/ /_/g" Sp9.blastp.detail.filtered.aligned.fas > Sp9.blastp.detail.filtered.aligned_.fas
#The sed tool removes any spaces in the annotation of the sequences. 

iqtree -s Sp9.blastp.detail.filtered.aligned_.fas -nt 2
#Iqtree outputs the most likely tree estimate of the aligned sequences. Furthermore, iqtree prvides the best substitution model for our tree estimates. Finally, the program makes an unrooted tree file. 

gotree reroot midpoint -i Sp9.blastp.detail.filtered.aligned_.fas.treefile -o Sp9.blastp.detail.filtered.aligned_.fas.midpoint.treefile
#This program reroots the phylogeny tree to the midpoint. 
```
