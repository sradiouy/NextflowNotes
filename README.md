# NextflowTutorial

This is a nextflow tutorial to create a simple pipeline. Also I will add notes and thing that are useful for future use.

## Step 1

First I wil clone these github repository to the local server: 

  git clone https://github.com/sradiouy/NextflowTutorial.git

## Step 2 

Every nextflow workflow requires two main files: 
  . main.nf
  . nextflow.config
  
The main.nf file contains the main nextflow script that calls the processes.
It doesn’t have to be named main.nf but that is standard practice (**highly recommended!**)

The nextflow.config file contains default parameters to use in the nextflow pipeline

  touch main.nf nextflow.config

## Step 3 

Modification of the main.nf to create a basic program.

Every nextflow main.nf need to have this line:
  #! /usr/bin/env nextflow

Then I will add a few lines to the main.nf to do really simple stuff:
  #! /usr/bin/env nextflow
  
  user="sradio"
  params.jobid ="bioinformatician"
  
  println "These is a pipeline created by $user who is a  $params.jobid"

