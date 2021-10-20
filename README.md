# Methods-Repository
Commands for Sp9 Protein 

```bash 
ncbi-acc-download -F fasta -m protein XP_032240311.1
blastp -db allprotein.fas -query XP_032240311.1.fa -outfmt 0 -max_hsps 1 -out Sp9.blastp.typical.out
blastp -db allprotein.fas -query XP_032240311.1.fa -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out Sp9.blastp.detail.out
awk '{if ($6<0.0000000000000000000000000000000000001)print $1 }' Sp9.blastp.detail.out > Sp9.blastp.detail.filtered.out
wc -l Sp9.blastp.detail.filtered.out
seqkit grep --pattern-file Sp9.blastp.detail.filtered.out allprotein.fas > Sp9.blastp.detail.filtered.fas muscle -in Sp9.blastp.detail.filtered.fas -out Sp9.blastp.detail.filtered.aligned.fas
t_coffee -other_pg seq_reformat -in Sp9.blastp.detail.filtered.aligned.fas -output sim
alv -k Sp9.blastp.detail.filtered.aligned.fas | less -r alv -kli --majority Sp9.blastp.detail.filtered.aligned.fas | less -RS
t_coffee -other_pg seq_reformat -in Sp9.blastp.detail.filtered.aligned.fas -action +rm_gap 50 -out Sp9.blastp.detail.filtered.aligned.r50.fas alv -kli --majority Sp9.blastp.detail.filtered.aligned.r50.fas | less -RS
```
