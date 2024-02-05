# README file for CNV and SV analysis during Master's thesis
Author: Yini Miao

## CNV analysis
### Input
Combined and single ```.cns``` files of every individual (from CNVkit)<br />
<br />
Combine all ```.cns``` and add ```barcode``` column:<br />
Code in ```/mnt/volume/QFEND/code/CNV_analysis/cnvkit/combine.sh``` <br />
```sh combine.sh -i xxx/data -o <output dict path, i.e. ../xxx >```

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
Code in ```/mnt/volume/QFEND/code/CNV_analysis/cnvkit/CNV.R```, more detailed visualized version: ```/mnt/volume/QFEND/code/CNV_analysis/cnvkit/CNV_analysis.Rmd``` and ```/mnt/volume/QFEND/code/CNV_analysis/cnvkit/CNV_analysis.html```<br />
Includes: CNV size filtering, PASS filtering, CHIP-related CNV annotation, oncoplot, recurrent plot. <br />
<br />Running code:<br />
```Rscript CNV.R -i /mnt/volume/QFEND/data/results_recall_wismut/cnvkit_amplicon/call_total.cns -w /mnt/volume/QFEND/data/results_recall_transfusion_tumor/cnvkit_amplicon/call_total.cns -r F -s T -c /mnt/volume/QFEND/code/maf_analysis/chip.csv -m /mnt/volume/QFEND/code/QFEND_sample_preparations_20220726092451/QFEND_sample_preparations_20230905105507.csv -g /mnt/volume/QFEND/code/CNV_analysis/gene.txt -o /mnt/volume/QFEND/code/results/CNV/cnvkit```

## SV analysis
More detailed one can be found in ```SV_analysis_README.Rmd``` and ```SV_analysis_README.html```
### Input
```manta.diploid_sv.pass.vcf``` files from Manta.<br />
### Filtering (env:bcftools)
code in ```filtering.sh```, change the path of data that needed to be filtered.<br \>
The filtered transfusion SVs are saved in ```/mnt/volume/QFEND/code/SV_analysis/manta/trans_wismut/pass/trans```. <br \>
The filtered wismut SVs are saved in ```/mnt/volume/QFEND/code/SV_analysis/manta/wismut_control/pass/tumor```.
### Summarize SV types per chromosome per sample (env:qfend)
code in ```summary.sh```<br />
output: ```/mnt/volume/QFEND/code/SV_analysis/manta/trans_wismut/pass/trans.summary.txt``` and ```/mnt/volume/QFEND/code/SV_analysis/manta/wismut_control/pass/wismut.summary.txt```<br />

<br />If want to generate for a single sample: ```SV_summary.R```
### SV filtering based on frequency (env:qfend)
Filtering Wismut SVs based on Trans SVs / Filtering Trans SVs based on Wismut SVs:<br />
input: ```wismut.summary.txt``` and ```trans.summary.txt``` <br />
code in ```SV_freq.R```<br />
output: ``/mnt/volume/QFEND/code/SV_analysis/manta/trans_freq0.5.txt``` and ```/mnt/volume/QFEND/code/SV_analysis/manta/wismut_freq0.5.txt```<br />

Have representative Wismut SVs based on Wismut SVs / Have representative Trans SVs based on Trans SVs:<br />
```SV_within_wismut_freq.R```<br />
output: ```/mnt/volume/QFEND/code/SV_analysis/manta/uni.sv.trans.txt``` and ```/mnt/volume/QFEND/code/SV_analysis/manta/uni.sv.wismut.txt```<br />

### SV bar plot
code in ```manta_visulization.R```

### SV and CNV circos plot, CHIP- and mLOY-related (env:qfend)
code in ```SV_CNV_circos.R```<br \>
```Rscript SV_CNV_circos.R```
