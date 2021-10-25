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

Lab 7
```bash 
java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s speciesTreeBilateriaCnidaria.tre -g Sp9.genes.tre --reconcile --speciestag prefix --savepng --events
#Notung will reconcile the given species tree and the midpoint rooted tree performed with gotree.

python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g Sp9.genes.tre.reconciled --include.species
thirdkind -f Sp9.genes.tre.reconciled.xml -o Sp9.genes.tre.genes.tre.reconciled.svg
#Use python2.7 to reformat the reconciled tree to PecPhyloXML, which thirdkind is able to view. 

java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s speciesTreeBilateriaCnidaria.tre -g Sp9.genes.tre --root --speciestag prefix --savepng --events
#Rerooting the Notung-3.0 allows us to use the command flag ---root that will minimize all duplications and deletions. A tree with minimals duplication and deletions are believed to be better representations of the phylogeny tree. 

iqtree -s Sp9.blastp.detail.filtered.aligned_.fas -bb 1000 -nt 2 -m PMB+F+R5 -t Sp9.genes.tre -pre sp9.gene.tree.ufboot
#Bootstrap represent the score of a particular branch, meaning how likely it is to be correct. 

gotree reroot midpoint -i sp9.gene.tree.ufboot.treefile -o sp9.gene.tree.midpoint.ufboot.treefile
#This command rerrots the tree with the boostrap support obtained with the command flag -bb 1000. 

java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s speciesTreeBilateriaCnidaria.tre -g Sp9.genes.midpoint.ufboot --root --speciestag prefix --savepng --events
#Reconciles the gene and species tree with the bootsrap support while minimizing deletion and duplication events. 
```











