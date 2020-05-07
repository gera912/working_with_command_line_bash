```
mouse_C1.concordant_uniq.sam.gz
```
This file contains the alignment data for an RNA-seq experiment in mouse, in the SAM file format.

For each instruction, record the *single* command you used.

1.	Extract out the alignment lines (1,000,000 sequences).
```
gzcat mouse_C1.concordant_uniq.sam.gz | grep -v "^@"
```
2.	Extract out the QNAME and sequence for each read.
```
gzcat mouse_C1.concordant_uniq.sam.gz | grep -v "^@" | cut -f 1,10
```
3.	Construct a ```sed``` expression to match the QNAME and sequence. Use sed to reverse the columns.
```

gzcat mouse_C1.concordant_uniq.sam.gz | grep -v "^@" | cut -f 1,10 | gsed -E 's/(NS500.+)      ([ACTGN]+)/\2    \1/'

grep -v "^@" mouse_C1.concordant_uniq.sam | cut -f 1,10 | gsed -E 's/(.+)\t(.+)/\2 \1/'

```

4.	Use ```sed```/```tr``` to convert the two column input into a FASTA file. Capture that FASTA file.
```
gzcat mouse_C1.concordant_uniq.sam.gz | grep -v "^@" | cut -f 1,10 | sed -E 's/(NS500.+)      ([ACTG]+)/>\1|\2/' | tr "|" "\n" > mouse_C1.fasta

grep -v "^@" mouse_C1.concordant_uniq.sam | cut -f 1,10 | gsed -E 's/(.+)\t(.+)/<\1\n\2/' > mouse_C1.concordant_uniq.fa
```

5.	What are the last three FASTA records in the file you created?
```

command: tail -n 6  mouse_C1.concordant_uniq.fa 



output:
<NS500451:154:HWKTMBGXX:1:11210:11578:12578-GACATGAG^CGTTGGAT;0^0
AGAAGATGCTGTCGCCGAGGAGGAGCGGGAGGAGGACGAGGAAGAGGAGAAACCAAGACCCAAACTTACTG
<NS500451:154:HWKTMBGXX:1:11210:20863:12591-TCGTCATC^CAACGATC;0^0
CTTGAGATCAACATCAGATTTGAGCAGGCGCAATACAGAATCTGTGAAATGACAATGTCGCAGCAGAGGAA
<NS500451:154:HWKTMBGXX:1:11210:20863:12591-TCGTCATC^CAACGATC;0^0
AGCTTTCTAGTGTCGGTGTGACTCATTACCTGTTACTAAAAACTCAAAGGGAAAAGTGGCATCATCATCTC
