# MyGenomepwth
Analyses for ABT480/CS485G genome assembly

### 1. Analysis of sequence quality
The F1 and R1 sequence datasets were analyzed using FASTQC:
```bash
ssh -Y pwth228@pwth228.cs.uky.edu
cd MyGenome
fastqc &
```
Load F1 and R1 datasets into GUI interface.
Take screenshots of output files:

![ForwardFastQC.png](/data/ForwardFastQC.png)

## 2. Ran trimmomatic
```bash
java -jar ~/Assembly/trimmomatic.0.38.jar PE -threads 16 -phred33 -trimlog file.txt A26_1.fastq A26_2.fastq A26_1_paired.fastq A26_1_unpaired.fastq A26_2_paired.fastq A26_2_unpaired.fastq 
```

## 3. Count the number of forward reads remaining
```bash
grep -c "^@" A26_1_paired.fastq
```
