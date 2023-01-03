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

Now I will expand a little bit on each of the blocks:

### Inputs

The input block allows you to define the input channels of a process, similar to function arguments. A process may have at most one input block, and it must contain at least one input.

The input block follows the syntax shown below:

```nextflow
input:
  <input qualifier> <input name>
 ```
 
An input definition consists of a qualifier and a name. The input qualifier defines the type of data to be received. This information is used by Nextflow to apply the semantic rules associated with each qualifier, and handle it properly depending on the target execution platform (grid, cloud, etc).

When a process is invoked in a workflow block, it must be provided a channel for each channel in the process input block, similar to calling a function with specific arguments. The examples provided in the following sections demonstrate how a process is invoked with input channels.

The available input qualifiers are listed in the following table:

- val: Access the input value by name in the process script.

- path: Handle the input value as a path, staging the file properly in the execution context.

- env:Use the input value to set an environment variable in the process script.

- stdin: Forward the input value to the process stdin special file.

- tuple: Handle a group of input values having any of the above qualifiers.

- each: Execute the process for each element in the input collection.

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

##### afterScript & beforeScript

The afterScript directive allows you to execute a custom (Bash) snippet immediately after/before the main process has run. 

**When combined with the container directive, the afterScript/beforeScript will be executed outside the specified container. In other words, the afterScript/beforeScript is always executed in the host environment.**

##### conda

The conda directive allows for the definition of the process dependencies using the Conda package manager.

Nextflow *automatically sets up an environment* for the given package names listed by in the conda directive. For example:

```nextflow
process foo {
  conda 'bwa=0.7.15'

  '''
  your_command --here
  '''
}
```

Multiple packages can be specified separating them with a *blank space* eg. bwa=0.7.15 fastqc=0.11.5. The name of the channel from where a specific package needs to be downloaded can be specified using the usual Conda notation i.e. prefixing the package with the channel name as shown here *bioconda::bwa=0.7.15.*

##### container

The container directive allows you to execute the process script in a Docker container.

It requires the Docker daemon to be running in machine where the pipeline is executed, i.e. the local machine when using the local executor or the cluster nodes when the pipeline is deployed through a grid executor.

For example:

```nextflow
process runThisInDocker {
  container 'dockerbox:tag'

  """
  <your holy script here>
  """
}
```

Simply replace in the above script dockerbox:tag with the name of the Docker image you want to use.

##### cpus

The cpus directive allows you to define the number of (logical) CPU required by the process’ task.

##### errorStrategy

The errorStrategy directive allows you to define how an error condition is managed by the process. By default when an error status is returned by the executed script, the process stops immediately. This in turn forces the entire pipeline to terminate.

__List of available error strategies:__

- terminate: Terminates the execution as soon as an error condition is reported. Pending jobs are killed (default)

- finish: Initiates an orderly pipeline shutdown when an error condition is raised, waiting the completion of any submitted job.

- ignore: Ignores processes execution errors.

- retry:Re-submit for execution a process returning an error condition.

When setting the errorStrategy directive to ignore the process doesn’t stop on an error condition, it just reports a message notifying you of the error event.

For example:

```nextflow
process ignoreAnyError {
  errorStrategy 'ignore'

  script:
  <your command string here>
}
```

The retry error strategy allows you to re-submit for execution a process returning an error condition. For example:

```nextflow
process retryIfFail {
  errorStrategy 'retry'

  script:
  <your command string here>
}
```

The number of times a failing process is re-executed is defined by the *maxRetries* and *maxErrors* directives.

#### memory

The memory directive allows you to define how much memory the process is allowed to use. For example: memory '2 GB' 

#### module

Environment Modules is a package manager that allows you to dynamically configure your execution environment and easily switch between multiple versions of the same software tool. If it is available in your system you can use it with Nextflow in order to configure the processes execution environment in your pipeline.

In a process definition you can use the module directive to load a specific module version to be used in the process execution environment. For example:

```nextflow
process basicExample {
  module 'ncbi-blast/2.2.27'

  """
  blastp -query <etc..>
  """
}
```

You can repeat the module directive for each module you need to load. Alternatively multiple modules can be specified in a single module directive by separating all the module names by using a : (colon) character as shown below:

```nextflow
 process manyModules {

   module 'ncbi-blast/2.2.27:t_coffee/10.0:clustalw/2.1'

   """
   blastp -query <etc..>
   """
}
```

#### executor

The executor defines the underlying system where processes are executed. *By default a process uses the executor defined globally in the nextflow.config file.*

The executor directive allows you to configure what executor has to be used by the process, overriding the default configuration. 

The following values can be used (there are more, but I select the most useful for us):

- awsbatch: The process is executed using the AWS Batch service.

- azurebatch: The process is executed using the Azure Batch service.

- google-lifesciences: The process is executed using the Google Genomics Pipelines service.

- k8s: The process is executed using the Kubernetes cluster.

- local: The process is executed in the computer where Nextflow is launched.

- slurm: The process is executed using the SLURM job scheduler.




## Extra Notes

### Docker 

I usually follow this instruction to install docker in ubuntu: https://docs.docker.com/engine/install/ubuntu/

