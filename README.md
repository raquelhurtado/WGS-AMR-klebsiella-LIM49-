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
prokka --outdir <strain> --prefix <strain> --genus Salmonella --species enterica --strain <strain> <strain>.fna
```
To multiple samples:
```
for f in *.fna; do prokka --outdir "${f/.fna}" --prefix "${f/.fna}" --genus Salmonella --species enterica --strain "${f/.fna}" "$f"; done;
```
## Prediction of AMR muatation and genes
RGI

To multiple samples:
```
for f in *.fna; do PointFinder.py -i "$f" -o /path/to/output-dir/ -p /path/to/pointfinder_db/ -s salmonella -m blastn -m_p /usr/bin/blastn;done;
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
