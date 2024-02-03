# README file for CNV and SV analysis during Master's thesis
Author: Yini Miao

## CNV analysis
### Input
Combined and single ```.cns``` files of every individual (from CNVkit)<br />
<br />
Combine all ```.cns``` and add ```barcode``` column:<br />
```sh combine_all_cnv.sh```

### Diagram and heatmap (env:cnvkit2)
Diagram:<br />
```cnvkit.py diagram -s Transfusionsmedizin.cns --sample-sex male```<br />
```cnvkit.py diagram -s wismut.cns --sample-sex male```<br />

Heatmap:<br />
Get paths of ```.cns``` files of all samples: <br />
```find /mnt/volume/QFEND/data/xxx -name "*.call.cns" > wismut.txt``` <br />
```find /mnt/volume/QFEND/data/xxx -name "*.call.cns" > Transfusionsmedizin.txt```<br />
<br />
Generate heatmaps:<br />
```cnvkit.py heatmap `cat wismut.txt` -o wismut__heatmap.png --sample-sex male```<br />
```cnvkit.py heatmap `cat Transfusionsmedizin.txt` -o Transfusionsmedizin__heatmap.png --sample-sex male```<br />

### CNV filtering and CHIP- / mLOY-related (env:qfend)
```CNV_analysis.Rmd```<br />

Includes: CNV size filtering, PASS filtering, CHIP- or mLOY related CNV annotation, oncoplot, recurrent plot.

## SV analysis
More detailed one can be found in ```SV_analysis_README.Rmd``` and ```SV_analysis_README.html```
### Input
```manta.diploid_sv.pass.vcf``` files from Manta.<br />
### Filtering (env:bcftools)
code in ```filtering.sh```
### Summarize SV types per chromosome per sample (env:qfend)
code in ```summary.sh```<br />
output: ```/mnt/volume/QFEND/code/SV_analysis/manta/trans_wismut/pass/trans.summary.txt``` and ```/mnt/volume/QFEND/code/SV_analysis/manta/wismut_control/pass/wismut.summary.txt```<br />

<br />If want to generate for a single sample: ```SV_summary.R```
### SV filtering based on frequency (env:qfend)
Filtering Wismut SVs based on Trans SVs / Filtering Trans SVs based on Wismut SVs:<br />
input: ```wismut.summary.txt``` and ```trans.summary.txt``` <br />
code in ```SV_freq.R```<br />
output: ```wismut_freq0.5.txt``` and ```trans_freq0.5.txt```<br />

Have representative Wismut SVs based on Wismut SVs / Have representative Trans SVs based on Trans SVs:<br />


### SV bar plot
code in ```manta_visulization.R```

### SV and CNV circos plot, CHIP- and mLOY-related (env:qfend)
code in ```SV_CNV_circos.R```
