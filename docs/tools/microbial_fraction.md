---
title: SingleM microbial_fraction
---
# singlem microbial_fraction
The SingleM `microbial_fraction` ('SMF') subcommand estimates the fraction of reads in a metagenome that are microbial, compared to everything else e.g. eukaryote- or phage-derived. Here we define 'microbial' as either bacterial or archaeal.

The main conceptual advantage of this method over other tools is that it does not require reference sequences of the non-microbial genomes that may be present (e.g. those of an animal host). Instead, it uses a SingleM taxonomic profile of the metagenome to "add up" the components of the community which are microbial. The remaining components are non-microbial e.g. host, diet, or phage. 

Roughly, the number of microbial bases is estimated by summing the genome sizes of each species/taxon after multiplying by their coverage in the taxonomic profile. The microbial fraction is then calculated as the ratio of microbial bases to the total metagenome size.

This tool can help prioritise samples for deeper sequencing, forecast how much sequencing is required for a given set of samples, and identify problematic steps in genome-resolved metagenomic pipelines.

The method is least reliable in simple communities consisting of a small number of species that are missing from the reference database. The main challenge in these cases is that the genome sizes of novel species are hard to estimate accurately. Multiplying a coverage from the taxonomic profile against an uncertain genome length equals an uncertain number of bases assigned as microbial.

To detect such situations SingleM `microbial_fraction` emits a warning when a sample's microbial_fraction estimate could under- or overestimate the read fraction. Specifically, the warning is emitted when the 3 highest abundance lineages not classified to the species level would change the estimated read fraction of the sample by >2% if their genome size is halved or doubled. Microbial fractions are also capped at 100% since values greater than this are impossible, but the original value can be recovered from the output if you calculate `bacterial_archaeal_bases / metagenome_size`.

Here is an example output from 'microbial_fraction':

| sample      | bacterial_archaeal_bases | metagenome_size | read_fraction | warning |
| ----------- | ------------------------ | --------------- | ------------- | ------- |
| KRGM_94     | 473147661                | 512995675       | 92.23         |         |

The `read_fraction` column is a percentage, i.e. 0.3 is 0.3%, nt 30%. The `warning` column is empty if no warning was emitted.

OPTIONS
=======

INPUT
=====

**-p**, **\--input-profile** *INPUT_PROFILE*

  Input taxonomic profile file [required]

READ INFORMATION [1+ ARGS REQUIRED]
=====================================

**-1**, **\--forward**, **\--reads**, **\--sequences** sequence_file [sequence_file \...]

  nucleotide read sequence(s) (forward or unpaired) to be searched.
    Can be FASTA or FASTQ format, GZIP-compressed or not. These must be
    the same ones that were used to generate the input profile.

**-2**, **\--reverse** sequence_file [sequence_file \...]

  reverse reads to be searched. Can be FASTA or FASTQ format,
    GZIP-compressed or not. These must be the same reads that were used
    to generate the input profile.

**\--input-metagenome-sizes** *INPUT_METAGENOME_SIZES*

  TSV file with \'sample\' and \'num_bases\' as a header, where sample
    matches the input profile name, and num_reads is the total number
    (forward+reverse) of bases in the metagenome that was analysed with
    \'pipe\'. These must be the same reads that were used to generate
    the input profile.

DATABASE
========

**\--taxon-genome-lengths-file** *TAXON_GENOME_LENGTHS_FILE*

  TSV file with \'rank\' and \'genome_size\' as headers [default: Use
    genome lengths from the default metapackage]

**\--metapackage** *METAPACKAGE*

  Metapackage containing genome lengths [default: Use genome lengths
    from the default metapackage]

OTHER OPTIONS
=============

**\--accept-missing-samples**

  If a sample is missing from the input-metagenome-sizes file, skip
    analysis of it without croaking.

**\--output-tsv** *OUTPUT_TSV*

  Output file [default: stdout]

**\--output-per-taxon-read-fractions** *OUTPUT_PER_TAXON_READ_FRACTIONS*

  Output a fraction for each taxon to this TSV [default: D o not
    output anything]

OTHER GENERAL OPTIONS
=====================

**\--debug**

  output debug information

**\--version**

  output version information and quit

**\--quiet**

  only output errors

**\--full-help**

  print longer help message

**\--full-help-roff**

  print longer help message in ROFF (manpage) format

AUTHORS
=======

>     Ben J. Woodcroft, Centre for Microbiome Research, School of Biomedical Sciences, Faculty of Health, Queensland University of Technology
>     Samuel Aroney, Centre for Microbiome Research, School of Biomedical Sciences, Faculty of Health, Queensland University of Technology
>     Raphael Eisenhofer, Centre for Evolutionary Hologenomics, University of Copenhagen, Denmark
>     Rossen Zhao, Centre for Microbiome Research, School of Biomedical Sciences, Faculty of Health, Queensland University of Technology
