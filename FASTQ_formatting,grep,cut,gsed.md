Your goal for this assignment is to transform a SAM file (```mouse_C1.concordant_uniq.sam.gz```) into a FASTQ file. Using your knowledge about SAM and FASTQ file formats, craft a single command (probably composed of multiple parts) to make the transformation. 

Submit:
1.	The command you used to generate your FASTQ file
```
gunzip -c mouse_C1.concordant_uniq.sam.gz | grep -v '^@' | cut -f 1,10,11| gsed -E 's/(.+)\t(.+)\t(.+)/@\1\n\2\n\+\n\3/' 

gzcat mouse_C1.concordant_uniq.sam.gz | grep -v "^@" | cut -f 1,10,11 | sed -E 's/(NS500.+)	([TCGAN]+)	(.+)/@\1|\2|+|\3/' | tr "|" "\n" > new_file.fq
```
2.	The 1024th record in your new FASTQ file (and the command you used to identify it).
```
command: gunzip -c mouse_C1.concordant_uniq.sam.gz | grep -v '^@' | cut -f 1,10,11| gsed -E 's/(.+)\t(.+)\t(.+)/@\1\n\2\n\+\n\3/' |  gsed -n '4093,4096p'

cat new_file.fq | head -4096 | tail -4


@NS500451:154:HWKTMBGXX:1:11101:20675:1544-TACGAACC^GTGATGTC;0^0
ACACCCGCCTAGCCAGCCAGATCAGCCGAATCAACCCTGGCGATCAATGGGGTGACAGATGTCGCAGCCAG
+
EEEEEEEEEEEE<EEEEEEEEEEEEEEEEEEEEEAEAEEEAEEEEEEEEE/E6EEEEEEEEAEEEEEEAEE
```

3.	Answers to the following questions:
    1. How many lines are in your FASTQ file?
    ```
    Command: gunzip -c mouse_C1.concordant_uniq.sam.gz | grep -v '^@' | cut -f 1,10,11| gsed -E 's/(.+)\t(.+)\t(.+)/@\1\n\2\n\+\n\3/'| wc -l
    
    Output: 4000000
    ```
    2. Is there a problem with your FASTQ file?
```
    Yes the sequence the indetifiers are not unique since data was from paired-end exp
    
     gunzip -c mouse_C1.concordant_uniq.sam.gz | grep -v '^@' | cut -f 1,10,11| gsed -E 's/(.+)\t(.+)\t(.+)/@\1\n\2\n\+\n\3/'| grep '^@'| sort| uniq -c| head
     
   2 @NS500451:154:HWKTMBGXX:1:11101:10002:10922-AGGACATG^CTGATGTG;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10004:1249-GAAGACCA^CTGACTGA;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10004:2205-CAACTGGT^ACTGTCAG;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10005:7243-ATCGTTGG^CTGATGTG;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10006:2342-GCGGTATT^GTTCTGCT;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10008:11579-CAACTGGT^CAACGTTG;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10009:16403-AGGACATG^TGTGTGTG;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10009:17474-TCAGGACT^ACAGGACA;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10011:10287-GAGAAGAG^AGGACATG;0^0
   2 @NS500451:154:HWKTMBGXX:1:11101:10012:4340-TGAGAGTG^ACTGTGAC;0^0

``` 
 
   3. If there is, how can you fix it? If not, how do you know there’s not?
 
``` 
    I would fix the unique issue by adding a line number toward the end of the sequence identifier. Making the sequence identifiers unique. Add  “counter” onto ID line.
 
```     
 
   4. What is the maximum expected value in a quality score?
    
```    
     gunzip -c mouse_C1.concordant_uniq.sam.gz | grep -v '^@' | cut -f 1,10,11| gsed -E 's/(.+)\t(.+)\t(.+)/@\1\n\2\n\+\n\3/' | gsed -n '4~4p'| grep 'E'| head

    6EEEEEEEEEEEEEEEEEEAE6E6EEEEE/EEEEEEEEEEE/EEEE6EEEEEEEAEEEEEE<EEAE/AEEE
    /E/E<<E//A6A<EA6EE/EA<EEEEEEAEEEEAEA/EE/EEEEEEEEEEEEEEEEEEEEEEEEEEEEEEE
    6<EEEEEEEEEEEEEEEEEEEEEEEEEAEEEEEAEEEEEEEEEEEEEEEE<EEEEEEEEEEEEEEEEEEEE
    EEEEE/EEEEEEEEEEEEEEEAEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEAEA
    6AEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEAEEEEEAEEEEEEEEEEEEEEEEE
    EEEEEEEEEEEEA<EEEEEEEEEEEEEEEEEEEEEEEAEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEE
    EEEAEEEEEEEEEEEEEEEAEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEE66
    EEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEE<
    6AEEEEAEEEAEEEEAEEEEEEEEEAEEA<EAEAAEAAAEEAEEEEEEEAEEAEE<E/66AEA/EE/EEAA
    E<EEAEEEEEEE/EEEEE<EEEEEEEEEEEEEEEEEEEEEEE<EEEEEEEAE/EEEEEEEEEEEEEEEEEE
   
 
     We know its Sanger or Phred 33 due to the '6' and 'E' values.
    
     We don't know if its sanger that has I, highest quality score 73 -33 = 40  or illumina that has J at the highest qulaity score 74 - 33 = 41. 
 ``` 
 
   5. What ASCII character encodes that value for your FASTQ file?
    
 ```  
   It could be either I (73) for Sanger and J (74) for Illumina
 ``` 
