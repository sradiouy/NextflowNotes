# Running on Background

Is important to note that using the & to send to background doesn't work well. Also the nohup command dosent play well with Nexflow.

So what I do in my case is to running with the option -bg. The ensure that the process will keep running whether the terminal was kill, and also in the background.

An example of a run will be:

> nextflow -bg run nf-core/chipseq -r 2.0.0 -params-file nf-params.json -profile docker

