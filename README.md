# WGS-AMR-klebsiella-LIM49-

Protocols used in Draft publication  "Piperacillin-Tazobactam Resistance bloodstream Infection by Klebisella pneumoniae developed during febrile neutropenia, is a new storm ahead of us?"

## Assembly genome
UNICYCLER
```
unicycler -1 SHORT1 -2 SHORT2 -o OUT -t 12
```
## Assembly evaluation
Checkm v
```
```

## Confirm MLST

MLST
```
mlst --list
mlst --legacy --scheme senterica *.fna > MLST_salmonella
```
## Annotation
Prokka
```
prokka --outdir <strain> --prefix <strain> --genus Klepsiella --species pneumoniae --strain <strain> <strain>.fna
```
To multiple samples:
```
for f in *.fna; do prokka --outdir "${f/.fna}" --prefix "${f/.fna}" --genus Klepsiella --species pneumoniae  --strain "${f/.fna}" "$f"; done;
```
## Prediction of AMR muatation and genes
RGI

```
rgi load --card_json /home/raquel/Documents/klepsiella/card.json --local

rgi main -i JAKSFX010000057.1_NEWregion.fa -o new-shv-rgi --input_type contig --alignment_tool DIAMOND --split_prodigal_jobs --local --clean
```

## Prediction of plasmids

Mob-suite
```
mob_recon -c -i <strain>.fna -p <strain> -o <strain>
```
To multiple samples:
```
for f in *.fna; do mob_recon -c -i "$f" -p "${f/.fna}" -o "${f/.fna}"; done;
```
## Prediction of ISs
Abricate
```
isescan.py --nthread 4 –seqfile UG14.fna –output UG14_IS
```

## Variant Calling and annotation
Snippy
```
snippy --cpus 6 --mincov 5 --outdir SE2 --ref ref.gbk --R1 SE2_R1.fastq.gz --R2 SE2_R2.fastq.gz
snippy-core --ref ref.gbk core mysnps1 mysnps2 mysnps3 mysnps4
```

##Mappind and count coverage
Bwa mem
```
bwa index JAKSFY010000013.1-shv5673.fasta 
bwa mem JAKSFY010000013.1-shv5673.fasta /home/raquel/Documents/klepsiella/5673_R1.fastq /home/raquel/Documents/klepsiella/5673_R2.fastq | samtools view -b | samtools sort > shv5672-JAKSFY13.bam
```
 
Bedtools
```
bedtools coverage -a regions.bed -b shv5673-JAKSFY13.bam -mean > mean_coverage.bed
```

## Phylogenetic tree

RaxML
```
raxmlHPC -s core.aln -n kpneumoniae_100tree -x 1979 -T 8 -m GTRGAMMA -f a -p 12345 -#1000 > kpneumoniae_log.txt
```

##BLast

BLast
```
blastn -query JAKSFY010000013.1.fasta -subject JAKSFZ035.fasta -out 5673_jaksfz35.tab -evalue 1e-10 -outfmt "6 qseqid sseqid pident qcovs length qlen slen qstart qend sstart send evalue bitscore stitle"
```
##Downloas ncbi sequences
```
datasets download genome accession --inputfile list.txt > outdir/all_accessions.zip
```
