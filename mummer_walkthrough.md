## mummer walkthrough



1. module load mummer (IST)
module load MUMmer/4.0.0rc1 (UPPMAX)


We will align using nucmer

Usage: nucmer [options] ref:path qry:path+

nucmer generates nucleotide alignments between two mutli-FASTA input
files. The out.delta output file lists the distance between insertions
and deletions that produce maximal scoring alignments between each
sequence. The show-* utilities know how to read this format.

By default, nucmer uses anchor matches that are unique in in the
reference but not necessarily unique in the query. See --mum and
--maxmatch for different bevahiors.

Options (default value in (), *required):
     --mum                                Use anchor matches that are unique in both the reference and query (false)
     --maxmatch                           Use all anchor matches regardless of their uniqueness (false)
 -b, --breaklen=uint32                    Set the distance an alignment extension will attempt to extend poor scoring regions before giving up (200)
 -c, --mincluster=uint32                  Sets the minimum length of a cluster of matches (65)
 -D, --diagdiff=uint32                    Set the maximum diagonal difference between two adjacent anchors in a cluster (5)
 -d, --diagfactor=double                  Set the maximum diagonal difference between two adjacent anchors in a cluster as a differential fraction of the gap length (0.12)
     --noextend                           Do not perform cluster extension step (false)
 -f, --forward                            Use only the forward strand of the Query sequences (false)
 -g, --maxgap=uint32                      Set the maximum gap between two adjacent matches in a cluster (90)
 -l, --minmatch=uint32                    Set the minimum length of a single exact match (20)
 -L, --minalign=uint32                    Minimum length of an alignment, after clustering and extension (0)
     --nooptimize                         No alignment score optimization, i.e. if an alignment extension reaches the end of a sequence, it will not backtrack to optimize the alignment score and instead terminate the alignment at the end of the sequence (false)
 -r, --reverse                            Use only the reverse complement of the Query sequences (false)
     --nosimplify                         Don't simplify alignments by removing shadowed clusters. Use this option when aligning a sequence to itself to look for repeats (false)
 -p, --prefix=PREFIX                      Write output to PREFIX.delta (out)
     --delta=PATH                         Output delta file to PATH (instead of PREFIX.delta)
     --sam-short=PATH                     Output SAM file to PATH, short format
     --sam-long=PATH                      Output SAM file to PATH, long format
     --save=PREFIX                        Save suffix array to files starting with PREFIX
     --load=PREFIX                        Load suffix array from file starting with PREFIX
     --batch=BASES                        Proceed by batch of chunks of BASES from the reference
 -t, --threads=NUM                        Use NUM threads (# of cores)
 -U, --usage                              Usage
 -h, --help                               This message
     --full-help                          Detailed help
 -V, --version                            Version

 Hidden options:
     --banded                             Enforce absolute banding of dynamic programming matrix based on diagdiff parameter (false)
     --large                              Force the use of large offsets (false)
 -G, --genome                             Map genome to genome (long query sequences) (false)


**writing commands**

nucmer --mum -G -p nucmer_V1_tig00259822 PacBioB238_tig00259822.fa /nfs/scistore03/bartogrp/dshipili/RosEl/Amajus.chr.IGDBV1.fasta

show-coords nucmer_V1_tig00259822.delta > nucmer_V1_tig00259822.coords

nohup nucmer -c 100 -p nucmer_V1_B239 /nfs/scistore03/bartogrp/dshipili/RosEl/Amajus.chr.IGDBV1.fasta B239_genome_10k_pilon.fa.masked &

promer --mum -p nucmer_V1_tig00259822_2 /nfs/scistore03/bartogrp/dshipili/RosEl/Amajus.chr.IGDBV1.fasta PacBioB238_tig00259822.fa

show-tiling nucmer_V1_tig00259822_2.delta > nucmer_V1_tig00259822_2.delta.tiling

Transform delta file into simple readable form:
show-coords is a command within mummer package

**From developers:**

Description

This program parses the delta alignment output of nucmer and promer and displays the coordinates, and other useful information about the alignments.

show-coords nucmer_V1_B238.delta > nucmer_V1_B238.coords


How to interpret coords files:
- [S1] Start of the alignment region in the reference sequence.
- [E1] End of the alignment region in the reference sequence.
- [S2] Start of the alignment region in the query sequence.  
- [E2] End of the alignment region in the query sequence.  
- [LEN 1] Length of the alignment region in the reference sequence, measured in nucleotides.  
- [LEN 2] Length of the alignment region in the query sequence, measured in nucleotides.  
- [% IDY] Percent identity of the alignment, calculated as the (number of exact matches) / ([LEN 1] + insertions in the query).  
- [% SIM] Percent similarity of the alignment, calculated like the above value, but counting positive BLOSUM matrix scores instead of exact matches.  
- [% STP] Percent of stop codons of the alignment, calculated as (number of stop codons) / (([LEN 1] + insertions in the query) * 2).  
- [LEN R] Length of the reference sequence.  [LEN Q] Length of the query sequence.  [COV R] Percent coverage of the alignment on the reference sequence, calculated as [LEN 1] / [LEN R].  
- [COV Q] Percent coverage of the alignment on the query sequence, calculated as [LEN 2] / [LEN Q].  
- [FRM] Reading frame for the reference sequence and the reading frame for the query sequence respectively. This is one of the columns absent from the nucmer data, however, match direction can easily be determined by the start and end coordinates.  
- [TAGS] The reference FastA ID and the query FastA ID.



How to stitch genome together:)
Let's see what contigs are in the chromosome 8 of snapdragon:

Choose one chromosome to look at:
grep "Chr8" nucmer_V1_B238.coords > nucmer_V1_B238_Chr8.coords


There are quite a few of matches:
wc -l nucmer_V1_B238_Chr8.coords
93809 nucmer_V1_B238_Chr8.coords


How many contigs are there?
awk '{print $13}' nucmer_V1_B238_Chr8.coords | uniq -u | wc -l
13

You can use the sort command:

sort -k2 -n yourfile
-n, --numeric-sort compare according to string numerical value

awk '($10 >= 98)'

grep "tig00259822" nucmer_V1_B238.coords | sort -k1 -n | awk '{print $12}' > uni.out
