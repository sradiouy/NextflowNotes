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
For example:

```nextflow
// Define the process
process square {

  // Define the input parameters
  input:
    val n from: 'numbers.txt'
    val log_level from: 'log_level.txt'

  // Define the output files
  output:
    file 'results.txt' into: result_files

  // Define a conditional execution clause
  when:
    log_level == 'verbose'

  // Define the process directives
  directives {
    memory '2.5 GB'
    cpus 4
    container 'python:3.7'
  }

  // Define the script to be executed
  script:
    // Compute the square of the input number
    n_square = n * n

    // Save the result to a file
    result_file = 'result_' + n + '.txt'
    writeFile file: result_file, text: n_square

    // Print the result to the console
    println n_square
}
```

Now I will expand a little bit en each of the blocks:

#### Directives

Directives are optional settings that affect the execution of the current process.

They must be entered at the top of the process body, before any other declaration blocks (input, output, etc), and have the following syntax:

```nextflow
name value [, value2 [,..]]
```
Some directives are generally available to all processes, while others depend on the executor currently defined.

##### accelerator

The accelerator directive allows you to specify the hardware accelerator requirement for the task execution e.g. GPU processor. For example:

```nextflow
process foo {
    accelerator 4, type: 'nvidia-tesla-k80'

    script:
    """
    your_gpu_enabled --command --line
    """
}
```

**The above examples will request 4 GPUs of type nvidia-tesla-k80.**

## Extra Notes

### Docker 

I usually follow this instruction to install docker in ubuntu: https://docs.docker.com/engine/install/ubuntu/

