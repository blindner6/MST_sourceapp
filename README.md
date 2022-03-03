# ::MST Source Apportioning Pipeline::

### Version 1, March 2022
### Gatech PACE User's Guide

## Description:

This pipeline is desgined to apportion fecal signal among multiple sources based on competitive read mapping of shotgun metagenomic data. The pipeline, scripts, and databases are all stored on the Kostas' Lab shared-p directory.

## Overview:

The pipeline accepts raw paired FASTQ files. It will run `MicrobeCensus` to estimate GEQ's before trimming with `Trimmomatic` and then competitively mapping reads to fecal source libraries (described here: https://www.sciencedirect.com/science/article/pii/S0043135421011878) with `MagicBLAST`. The resulting mapped reads are used to calculate sequencing depth for each of the source libraries before normalizing against GEQs. These results are reported to the user. 

The pipeline runs with the following steps:

0: Accessioning. Data importation, renaming, and sizing.
1: Census. GEQ determination.
2: Trimming. Quality control.
3: Mapping. Competitive read recruitment.
4: Seqdepth. Calculation of sequencing depth.

## Installation and Setup:

No installation is necessary for PACE. Users simply need access to Kostas' Lab `shared-p` (i.e., `shared3`) 

## Quickstart: 

The pipeline is launched with the following BASH script:

```/storage/coda1/p-ktk3/0/shared/rich_project_bio-konstantinidis/shared3/mst/01_pipeline/xx_launcher.sh```

Four inputs are required, in the following order:

Input1: Alphanumeric string for identifying the dataset. Must be unique and NOT contain special characters.

Input2: Absolute path to gzipped FASTQ file containing raw forward reads for the dataset.

Input3: Absolute path to gzipped FASTQ file containing raw reverse reads for the dataset.

Input4: Directory where the user would like the project created.

Example usage:

```bash /storage/coda1/p-ktk3/0/shared/rich_project_bio-konstantinidis/shared3/mst/01_pipeline/xx_launch sample path/to/reads.1.fastq.gz path/to/reads.2.fastq.gz path/to/output_dir```

This will create a directory in "path/to/output_dir" called "MST_sourceapp" where the provided FASTQ files will be copied and processed. Multiple samples can be added to the same project directory by running the launcher with new inputs. Do not re-use sample identifiers.

## Restarting Failed Steps:

Occasionally, the pipeline may fail. When you have identified the step which has failed, relaunch by providing the wrapper script with the following variables: First, sample ID or `sid`. This has been provided as "sample" in the example below. Second, the job ID or `jid`. This has been provided as two "2" in the example below and corresponds with the trimming step.

```qsub /storage/coda1/p-ktk3/0/shared/rich_project_bio-konstantinidis/shared3/mst/01_pipeline/xx_wrapper.pbs-v sid=sample,jid=2```

Doing so will prompt the pipeline to retry the failed step.

## Known Issues:

The pipeline uses several hard-coded paths for various software and thus cannot be packaged for distribution yet.
