Decompress ```fst.tar.gz``` into your working directory. This fst directory contains a set of F<sub>ST</sub> files. F<sub>ST</sub> values are a measure of genetic differentiation, here between populations of Stickleback fish. You can find out about F<sub>ST</sub> [here](https://en.wikipedia.org/wiki/Fixation_index). 

Your goal is to prepare these F<sub>ST</sub> data files for plotting. We need to separate the data by chromosome. You can find the chromosome name (aka linkage group) in **column 5** and the F<sub>ST</sub> values in **column 9**. Your final output files should contain only the linkage group and F<sub>ST</sub> values.

1.	Move to the ```fst``` directory. 
2.	Use ```ls -1``` to determine a list of files to examine. 
3.	Use ```cut```/```grep```/```sort```/```uniq``` to determine a list of chromosomes to process, ignoring scaffolds. 
```
command: cut -f 5 batch_2.fst_*.tsv  | grep "^group."| sort | uniq | sort
output: 
groupI
groupII
groupIII
groupIV
groupIX
groupV
groupVI
groupVII
groupVIII
groupX
groupXI
groupXII
groupXIII
groupXIV
groupXIX
groupXV
groupXVI
groupXVII
groupXVIII
groupXX
groupXXI 


```
4.	Work out the command to extract the chromosomes and F<sub>ST</sub> values from one F<sub>ST</sub> file and redirect them into their own file. 
```
cut -f 5,9 batch_2.fst_1-2.tsv | grep "^group." > batch_2.fst_1-2C.tsv 
cut -f 5,9 batch_2.fst_1-3.tsv | grep "^group." > batch_2.fst_1-3C.tsv 
cut -f 5,9 batch_2.fst_2-3.tsv | grep "^group." > batch_2.fst_2-3C.tsv
```
5.	Write a shell script that uses two shell loops, one nested inside the other, to separate each F<sub>ST</sub> file into separate files, one for each chromosome. Name them properly, so given the input ```file batch_2.fst_1-2.tsv```, the file for chromosome 4 should be called ```batch_2.fst_1-2_groupIV.tsv```. 

>ICA6pt5.txt
```
Script started on Wed Jul  3 19:09:21 2019
[?1034hbash-3.2$ cat ICA5[K6pt5
#!/usr/bin/env bash


files='batch_2.fst_1-2.tsv
       batch_2.fst_1-3.tsv
       batch_2.fst_2-3.tsv' 

chrms='groupI
groupII
groupIII
groupIV
groupIX
groupV
groupVI
groupVII
groupVIII
groupX
groupXI
groupXII
groupXIII
groupXIV
groupXIX
groupXV
groupXVI
groupXVII
groupXVIII
groupXX
groupXXI'


for chrm in $chrms
do
	for file in $files 
	do 
	name=${file}_${chrm}
	cat $file | grep '	'$chrm'	'| cut -f 5,9 > $name.txt
done
done


bash-3.2$ ./ICA6pt5
bash-3.2$ exit
exit

Script done on Wed Jul  3 19:10:26 2019
```
6.	Add variables to make paths absolute. 

>ICA6p6.txt

```
Script started on Thu Jul  4 02:10:17 2019
[?1034hbash-3.2$ cat ICA6p6
#!/usr/bin/env bash


files='batch_2.fst_1-2.tsv
       batch_2.fst_1-3.tsv
       batch_2.fst_2-3.tsv'

chrms='groupI
groupII
groupIII
groupIV
groupIX
groupV
groupVI
groupVII
groupVIII
groupX
groupXI
groupXII
groupXIII
groupXIV
groupXIX
groupXV
groupXVI
groupXVII
groupXVIII
groupXX
groupXXI'


dir='/Users/gerardoperez/Desktop/shell/fst/files/'


for chrm in $chrms
do
	for file in $files 
	do 
	name=${dir}${file}_${chrm}
	cat $dir$file | grep '	'$chrm'	'| cut -f 5,9 > $name 
done
done


bash-3.2$ .lICA6p6[1PICA6p6/ICA6p6
bash-3.2$ exit
exit

Script done on Thu Jul  4 02:11:47 2019
```

Turn in:
1. Your script
2. A list of file names with the number of lines in each file
3. The first 4 and last 4 lines of ```batch_2.fst1-2_groupI.tsv```

Hints:
You will need to grep data for each chromosome out of each F<sub>ST</sub> file. Make sure your search pattern picks up ```groupI``` in the groupI file, but not, say ```group IX```. One way to do this is to include tabs on either side of the groupI pattern, like ```<tab>groupI<tab>```, of course, using actual tab characters.
