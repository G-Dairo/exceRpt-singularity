# exceRpt without Docker

## exceRpt installation using singularity

exceRpt is a pipeline primarily designed for small RNA sequencing data. The steps below show how to handle its installation using singularity.


- Load singularity first

```
module avail singularity
module load singularity

```

- Pull the docker image and convert it to a singularity image using the code below:

```

singularity pull docker://rkitchen/excerpt

```
This will generate an `excerpt_latest.sh` file, a container that houses the exceRpt pipeline.

`Note:` exerRpt uses external aligners such as HISAT2 or STARS to handle the alignment step.


- Prepare the Genome and Data files for alignment

1. First prepare your input and output working directory

```

mkdir /home/InputSample/
mkdir /home/OutputSample/

```

2. I downloaded the reference genome (hg38) in FASTA format because I used the HISAT2 aligner 

```
wget http://hgdownload.cse.ucsc.edu/goldenpath/hg38/bigZips/hg38.fa.gz -P /home/InputSample/

```

3. Unzip the FASTA file

```
gunzip /home/InputSample/hg38.fa.gz

```

4. Download the dataset you want to align. I will download it in the InputSample directory.
I will be downloading the publicly available SRR026761.sra file and convert it to a fastq file simulaneously

```
module load sra-toolkit
fasterq-dump --version
fasterq-dump SRR026761 -O /home/InputSample/

```

- Build HISAT2 Index
HISAT2 requires the reference genome to be indexed before it can perform alignments.

```

hisat2-build /home/InputSample/hg38.fa /home/InputSample/hg38/genome

```

- Align the reads using HISAT2

```

hisat2 -x /home/InputSample/hg38/genome \
       -U /home/InputSample/SRR026761.fastq \
       -S /home/OuputSample/AnyName.sam

``` 




 


