## mummer walkthrough



module load mummer

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
