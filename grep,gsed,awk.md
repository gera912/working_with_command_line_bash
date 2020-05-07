Download and decompress the following file from HPC:
```
trimmed_reads.fq.gz
```
1.	Determine the length distribution of the trimmed reads (you should be able to do parts i-iii using one line).
    1.	Extract the sequence
    ```
    gsed -n '2~4p' trimmed_reads.fq 
    ```
    2.	Measure its length
    ```
    gsed -n '2~4p' trimmed_reads.fq | awk '{print length($0)}'
     ```
    3.	Compute the distribution of lengths
    ```
    gsed -n '2~4p' trimmed_reads.fq | awk '{print length($0)}'| sort | uniq -c |sort
    
    gzcat trimmed_reads.fq.gz | grep -A 1 "^@unbarcoded" | grep -v "^@unbarcoded" | grep -v "^--" | awk '{print length($1)}' | sort -n | uniq -c
   
    ```
2.	Use your favorite plotting software to plot the distribution of read lengths.

command for column 1 :awk '{print $1}' sequence_counts.txt
command for column 2 awk '{print $2}' sequence_counts.txt

Download the following files from HPC:
```
mouseSaline1_fw.genecount
mouseSaline1_rv.genecount
```
3.	Determine the percentage of reads that mapped to a feature (gene).
    1.	Sum the number of reads that mapped to a feature. (one line command)
    ```
    command: grep '^ENSMUSG' mouseSaline1_fw.genecount | awk '{sum+=$2} END {print sum}'
    Output:894086
    
    command: grep '^ENSMUSG' mouseSaline1_rv.genecount | awk '{sum+=$2} END {print sum}'
    Output:14442057
    ```
    
    
   
    2.	Calculate the total number of reads. (one line command)
    command:
    
  ```
   command: awk '{sum+=$2} END {print sum}' mouseSaline1_fw.genecount
   output: 19177391
    
   command: awk '{sum+=$2} END {print sum}' mouseSaline1_rv.genecount
   output: 19177391
 ```
    
    
 3.	Divide by the number of mapping reads by the total number of reads. (feel free to use a calculator)
    ```  
    mouseSaline1_fw.genecount  894086/19177391 = 0.04662187885
    
    mouseSaline1_rv.genecount 14442057/19177391 = 0.75307725644
    ```
 4.	*CHALLENGE* – Condense as many parts as possible to a single line command.
 ```
     $ cat mouseSaline1_fw.genecount | awk '$1~"ENSMUS" {sum += $2} $1~"__" {count += $2} END {print sum;print count+sum;print      
     sum/(sum+count)}'
     894086
     19177391
    0.0466219
  

     $ cat mouseSaline1_rv.genecount | awk '$1~"ENSMUS" {sum += $2} $1~"__" {count += $2} END {print sum;print       
     count+sum;print sum/(sum+count)}'
     14442057
     19177391
     0.753077
```
    
4.	Which file had more reads mapping to features?
```
Reverse file: mouseSaline1_rv.genecount had more reads mapping to features
Must been reverse stranded kit
```
