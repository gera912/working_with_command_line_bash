## Part 1
Download and expand the ```genecounts.tar.gz``` archive and move to the samples directory.
1.	Execute a command that can identify the number of reads that mapped to gene ```ENSMUSG00000020848``` in the file ```Saline1.genecount```.
```
command:grep '^ENSMUSG00000020848' Saline1.genecount
Output: ENSMUSG00000020848	4

```
2.	Combine the command from question 1 with the ```ls -1``` command in a while loop to count the number of reads mapped to ```ENSMUSG00000020848``` in each file.
```
Command:ls -1 | while read line; do grep '^ENSMUSG00000020848' $line ; done 

Output:
ENSMUSG00000020848	7
ENSMUSG00000020848	14
ENSMUSG00000020848	5
ENSMUSG00000020848	4293
ENSMUSG00000020848	5145
ENSMUSG00000020848	4
ENSMUSG00000020848	3
ENSMUSG00000020848	5

```
3.	Modify the command in your loop to prefix each output with the file name.
```
Command:
ls -1 | while read line; do echo $line ; grep '^ENSMUSG00000020848' $line ; done 

Output:
Furamidine1.genecount
ENSMUSG00000020848	7
Furamidine2.genecount
ENSMUSG00000020848	14
Furamidine3.genecount
ENSMUSG00000020848	5
Heptamidine1.genecount
ENSMUSG00000020848	4293
Heptamidine2.genecount
ENSMUSG00000020848	5145
Saline1.genecount
ENSMUSG00000020848	4
Saline2.genecount
ENSMUSG00000020848	3
Saline3.genecount
ENSMUSG00000020848	5
```

## Part 2
1.	Create a script to do what you did in Part 1.

>ICA5pt1.txt

```
Script started on Mon Jul  1 11:53:20 2019
[?1034hbash-3.2$ cat ICA5pt1
#!/usr/bin/env bash

dir='/Users/gerardoperez/Desktop/shell/genecounts/'

ls -1 $dir | while read line
        do echo $dir$line ;grep '^ENSMUSG00000020848' $dir$line;

done



bash-3.2$ ./ICA%[K5pt1
/Users/gerardoperez/Desktop/shell/genecounts/Furamidine1.genecount
ENSMUSG00000020848	7
/Users/gerardoperez/Desktop/shell/genecounts/Furamidine2.genecount
ENSMUSG00000020848	14
/Users/gerardoperez/Desktop/shell/genecounts/Furamidine3.genecount
ENSMUSG00000020848	5
/Users/gerardoperez/Desktop/shell/genecounts/Heptamidine1.genecount
ENSMUSG00000020848	4293
/Users/gerardoperez/Desktop/shell/genecounts/Heptamidine2.genecount
ENSMUSG00000020848	5145
/Users/gerardoperez/Desktop/shell/genecounts/Saline1.genecount
ENSMUSG00000020848	4
/Users/gerardoperez/Desktop/shell/genecounts/Saline2.genecount
ENSMUSG00000020848	3
/Users/gerardoperez/Desktop/shell/genecounts/Saline3.genecount
ENSMUSG00000020848	5
bash-3.2$ exit
exit

Script done on Mon Jul  1 11:54:39 2019

```

## Part 3
Download and unarchive ```seqs.tar.gz``` into your working directory. This seqs directory contains a set of five FASTQ files, containing DNA sequences.
1.	Move to the ```seqs``` directory.
2.	Use ```ls ‐1``` combined with a shell loop to count the number of lines in each file.
```
command:ls -1 indv_*.fq |  while read line ; do wc -l $line; done
Output:  
    4000 indv_01.fq
    4000 indv_02.fq
    4000 indv_03.fq
    4000 indv_04.fq
    4000 indv_05.fq

```
3.	Use a shell loop to concatenate the files into a single file, use the ```>>``` operator to redirect the output.
```
command: ls -1 indv_*.fq |  while read line ; do cat $line >> ICA5pt2.txt ; done
```
4.	Count the lines in the final file, make sure they match the sum of the lines in the individual files.
```
commnad:  wc -l ICA5pt2.txt
output: 20000 ICA5pt2.txt

command: wc -l indv_01.fq indv_02.fq indv_03.fq indv_04.fq indv_05.fq
output:  
    4000 indv_01.fq
    4000 indv_02.fq
    4000 indv_03.fq
    4000 indv_04.fq
    4000 indv_05.fq
   20000 total

```

5.	Create a shell script to do all the above.

>ICA5pt2F.txt


```
Script started on Mon Jul  1 12:40:47 2019
[?1034hbash-3.2$ ICA5pt2F.txt[K[K[K[K[K[K[K[K[K[K[K[KICA5p2f.sh [1@c[1@a[1@t[1@ 
#!/usr/bin/env bash

dir='/Users/gerardoperez/Desktop/shell/seqs/'

rm -f ICA5pt2F.txt
ls -1 $dir | while read line
        do cat $dir$line  >> ICA5pt2F.txt
	wc -l $dir$line

done
wc -l ICA5pt2F.txt 


bash-3.2$ ./ICA5p2f.sh                         [K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K[K
    4000 /Users/gerardoperez/Desktop/shell/seqs/indv_01.fq
    4000 /Users/gerardoperez/Desktop/shell/seqs/indv_02.fq
    4000 /Users/gerardoperez/Desktop/shell/seqs/indv_03.fq
    4000 /Users/gerardoperez/Desktop/shell/seqs/indv_04.fq
    4000 /Users/gerardoperez/Desktop/shell/seqs/indv_05.fq
   20000 ICA5pt2F.txt
bash-3.2$ exit
exit

Script done on Mon Jul  1 12:43:37 2019
```

To turn in your work for this assignment, do the following:
1.	Execute the ```script``` command. It will appear as nothing has happened and the command should immediately return the prompt. This will cause script to begin recording all input and output on the shell. 
2.	Concatenate the contents of your script to standard output: ```$ cat myscript.sh ```
3.	And now execute your script: ```$ ./myscript.sh``` 
4.	Type ```exit``` to end script, creating a file called ```typescript``` containing your input/output.
5.	Append the ```.txt``` extension to your file.
6.	The script command records every keystroke so that by re-running it, you can exactly re-create what you did. However, this looks messy when viewed as a text file. Clean up your typescript file using your favorite text editor. **CHALLENGE** – write a script to clean up your typescript files.
7.	Turn your file in through GitHub. 

*Hints*: You have already used the ```>``` shell redirection operator. This causes output to be redirected into a file you specify, overwriting the contents of the file if it already exists. The ```>>``` shell redirection operator will append to the end of a file instead of overwriting it.
