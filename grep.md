For each of the following questions, provide the single command you used to generate the answers. Be sure to use triple backticks 
(\`\`\`) to set off code. You can find more about GitHub Flavored Markdown here: https://help.github.com/articles/basic-writing-and-formatting-syntax/

You should already have the file s_1_seq.fastq. If not, you can find a copy in this repository.

Name:

1.	Count the number of raw reads (answer: 250,000).
```
grep -c "^@HWI" s_1_seq.fastq
```
2.	Count the number of reads with barcode CGATA (answer: 19,501).
```
grep -c "^CGATA"  s_1_seq.fastq
```
3.	Capture all FASTQ records for ACCAT into a file called sample_01.fq (you should get a file with 18,352 records and 73,408 lines).

```
grep -A 2 -B 1 "^ACCAT" s_1_seq.fastq | grep -v "^--" > sample_01.fq
```

4.	Determine the count of all barcodes in the file. You should get the following counts (plus a bunch more, not shown here to save space):


```
286 CTAGT
7900 TCAGA
10659 ACTGC
10931 TGACC
11536 GAGAT
11871 CTGAA
14409 CGGCG
14508 TGGTT
18226 GAAGC
18352 ACCAT
18375 TCGAG
19501 CGATA
23012 AATTT
26336 GCATT
31136 CTAGG
```

```
cat s_1_seq.fastq | grep -A 1 "^@" | grep "^[A-Z]" | cut -c 1-5 | sort | uniq -c | sort –n
```

Some hints:
1.	Use head when building a command, cat once the command is working
2.	Look at the ```-n``` option for the head command, the ```-l``` option for ```wc```
3.	The ```^``` character means “must occur at the beginning of line” in a grep search
4.	Look at the grep options: ```-c```, ```-v```, ```-A```, ```-B```
Read the ```man``` pages for ```sort``` and ```uniq``` to learn how to combine them
