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
```

## 6. Using blastn to predict mitochondrial markers
```bash
blastn -query MoMitochondrion.fasta -subject T29_final.fasta -evalue 1e-50 -max_target_seqs 20000 -outfmt '6 qseqid sseqid slen length qstart qend sstart send btop' -out MoMitochondrion.T29.BLAST

awk '$4/$3 > 0.9 {print $2 ",mitochondrion"}' MoMitochondrion.T29.BLAST > T29_mitochondrion.csv
```
```bash
blastn -query B71v2sh_masked.fasta -subject T29_Final.fasta -evalue 1e-50 -max_target_seqs 20000 -outfmt '6 qseqid sseqid qstart qend sstart send btop' -out B71v2sh.T29.BLAST

awk '$4/$3 > 0.9 {print $2 ",mitochondrion"}' B71v2sh.T29.BLAST > T29_B71.csv
```

## 7. Gene Prediction using SNAP, Augustus, and MAKER
SNAP
```bash
fathom genome.ann genome.dna -gene-stats
fathom genome.ann genome.dna -categorize 1000
fathom uni.ann uni.dna -gene-stats
fathom uni.ann uni.dna -export 1000 -plus
forge export.ann export.dna
hmm-assembler.pl Moryzae . > Moryzae.hmm
snap-hmm Moryzae.hmm T29_final.fasta T29-snap.zff
```
Augustus
```bash
```
MAKER
```bash
```

