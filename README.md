# MyGenomepwth
Analyses for ABT480/CS485G genome assembly

### 1. Analysis of sequence quality
The F1 and R1 sequence datasets were analyzed using FASTQC:
```bash
ssh -Y pwth228@pwth228.cs.uky.edu
cd MyGenome
fastqc &
```
Forward Sequence
![ForwardFastQC.png](/data/ForwardFastQC.png)
Reverse Sequence
![ReverseFastQc.png](/data/ReverseFastQC.png)

## 2. Ran trimmomatic
```bash
java -jar ~/Assembly/trimmomatic.0.38.jar PE -threads 16 -phred33 -trimlog file.txt A26_1.fastq A26_2.fastq A26_1_paired.fastq A26_1_unpaired.fastq A26_2_paired.fastq A26_2_unpaired.fastq 
```

## 3. Count the number of forward reads remaining
```bash
grep -c "^@" A26_1_paired.fastq
```

## 4. Assembling the genome using velvetoptimiser
Step size of 10
```bash
sbatch velvetoptimiser_noclean.sh A26 61 131 10
```
Step size of 2
```bash
sbatch velvetoptimiser_noclean.sh A26 71 111 2
```

## 5. Checking genome completeness using BUSCO
```bash
sbatch /project/farman_s24cs485g/pwth228/BuscoSingularity.sh T29
