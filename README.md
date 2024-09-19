# HIC Workflow

Snakemake workflow for QC and processing of HIC data. Results in on .cool file and TAD calls for the data. All commands used to QC, and process hic-data are contianed in the main Snakefile.

**Prep**:

Place raw HIC reads in a new directory data/raw

Ensure raw files are compressed (.fastq.gz) - snakemake workflow will error if files are unzipped .fastq

If certain samples should be merged (replicates) make sure to give them the same prefix (eg. rhesus_sample1, rhesus_sample2) and designate this prefix in the configure file

```
mkdir -p data/raw
ln -s /path/to/some/data/*.fastq.gz data/raw/
```

**Configure the file**: src/config.yml

Generate Arima HIC genome restriction site file using the [hicup](https://www.bioinformatics.babraham.ac.uk/projects/hicup/) command [hicup_digester](https://www.bioinformatics.babraham.ac.uk/projects/hicup/) with the flag `--arima` for compatability with the Arima HIC protocol.

All commands used to QC, and process hic-data are contianed in the main Snakefile. The pipeline uses conda for dependency management. Make sure you have installed a recent version of snakemake and conda.

```
example:
hicup_digester --re1 ^GATC,MboI --genome Mouse_mm10 --outdir hicup-digest/ *.fa &
```

**Execution**:

```
snakemake --use-conda -j20
snakemake --use-conda -j20 > $(date +"%y%m%d%H%M%S")_snakemake.out 2>&1

```

**Runtime**

The hicup pipeline is the most resource intensive step that can be expected to run for at least 24 hours for a sample with a sequencing depth of 500 million reads and 8 threads.
