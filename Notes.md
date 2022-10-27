# Notes

Lesson learned while using nextflow

## Running on Background

Is important to note that using the & to send to background doesn't work well. Also the nohup command dosent play well with Nexflow.

So what I do in my case is to running with the option -bg. The ensure that the process will keep running in the background whether the terminal was kill.

An example of a run will be:

> nextflow -bg run nf-core/chipseq -r 2.0.0 -params-file nf-params.json -profile docker

## Nextflow memory requirements

In some cases, the Nextflow Java virtual machines can start to request a large amount of memory. We recommend adding the following line to your environment to limit this (typically in ~/.bashrc or ~./bash_profile):

NXF_OPTS='-Xms1g -Xmx4g'
