# NextflowTutorial

This is a nextflow tutorial to create a simple pipeline. Also I will add notes and thing that are useful for future use.

## Step 1

First I wil clone these github repository to the local server: 

```bash
  git clone https://github.com/sradiouy/NextflowNotes.git
```

## Step 2 

Every nextflow workflow requires two main files: 
  . main.nf
  . nextflow.config
  
The main.nf file contains the main nextflow script that calls the processes.
It doesn’t have to be named main.nf but that is standard practice (**highly recommended!**)

The nextflow.config file contains default parameters to use in the nextflow pipeline

```bash
  touch main.nf nextflow.config
```
## Step 3 

Modification of the main.nf to create a basic program.

Every nextflow main.nf need to have this line:
  #! /usr/bin/env nextflow

Then I will add a few lines to the main.nf to do really simple stuff:
```nextflow
  #! /usr/bin/env nextflow
  
  user="sradio"
  params.jobid ="bioinformatician"
  
  println "These is a pipeline created by $user who is a $params.jobid"
```

The output will be: 

>N E X T F L O W  ~  version 22.04.4
>
>Launching `main.nf` [trusting_sinoussi] DSL2 - revision: 3f4be19378
>
>These is a pipeline created by sradio who is a bioinformatician

Is important to note that the parameteres (user and jobid) are of two types: 

- Internal: need to be defined in the scirpt body (*user*)
- External: could have a default value in the body of the script, but the value could change when the pipeline is executed using the double hyphen (*jobid*). As an example in these case:

```bash
nextflow run main.nf --jobid "manager"
```

## Nextflow Concept

### Processes

In Nextflow, a process is a fundamental unit of computation that executes a user-defined script. Typically, a Nextflow process is defined using the process keyword, followed by a unique name for the process and a code block containing the script to be executed. This script can be written in any language, and it can perform a wide range of tasks, such as data analysis, data transformation, or data visualization. Once a process is defined, it can be executed within a Nextflow pipeline, which is a series of interconnected processes that work together to achieve a common goal. In this way, Nextflow enables users to create complex, reproducible, and scalable computational pipelines using a simple and intuitive syntax.

A process may contain any of the following definition blocks: directives, inputs, outputs, when clause, and the process script. The syntax is defined as follows:

```bash
process < name > {

  [ directives ]

  input:
    < process inputs >

  output:
    < process outputs >

  when:
    < condition >

  [script|shell|exec]:
    < user script to be executed >

}
```

## Extra Notes

### Docker 

I usually follow this instruction to install docker in ubuntu: https://docs.docker.com/engine/install/ubuntu/

