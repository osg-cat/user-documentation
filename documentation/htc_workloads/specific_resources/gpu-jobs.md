---
ospool:
  path: htc_workloads/specific_resource/gpu-jobs.md
---

Using GPUs on the OSPool 
====================================



The Open Science Pool has an increasing number of GPUs available to 
run jobs. 

# Requesting GPUs

To request a GPU for your HTCondor job, you can use the 
HTCondor *request_gpus* attribute in your submit file (along 
with the usual *request_cpus*, *request_memory*, and *request_disk*
attributes). For example:

    request_gpus = 1
    request_cpus = 1
    request_memory = 4 GB
    request_disk = 2 GB

Currently, a job can only use 1 GPU at the time.

## Specific GPU Requests

HTCondor records different GPU attributes that can be used to select 
specific types of GPU devices. A few attributes that may be useful: 

* `GPUs_Capability`: this is NOT the GPU library, but rather a measure of the GPU's "Compute Capability"
* `GPUs_DriverVersion`: maximum version of the GPU libraries that can be supported
* `GPUs_GlobalMemoryMb`: amount of GPU memory available on the GPU device

Any of the attributes above can be used in the submit file's `requirements` line to 
select a specific kind of GPU. For 
example, to request a GPU with more than 8GB of GPU memory, one could use: 

    requirements = (GPUs_GlobalMemoryMb >= 8192)
    
If you want a certain type or family of GPUs, we usually recommend using the GPU's 
'Compute Capability', known as the `GPUs_Capability` by HTCondor. An A100 GPU has a 
Compute Capability of 8.0, so if you wanted to run on an A100 GPU specifically, 
the submit file requirement would be: 

    requirements = (GPUs_Capability == 8.0)

**Note that the more requirements you include, the fewer resources will be available 
to you! It's always better to set the minimal possible requirements (ideally, none!) 
in order to access the greatest amount of computing capacity.**

# Available GPUs

## Capacity

There are multiple OSPool contributors providing GPUs on a regular basis to the OSPool. 
Some of these contributors will make their GPUs available only when there is demand 
in the job queue, so after initial small-scale job testing, we strongly recommend 
submitting a signficant batch of test jobs to explore 
how much throughput you can get in the system as a whole. 

Also, **some members of the OSPool will only run GPU jobs if the job 
uses a container** (see note about software below). 

## GPU Types

Because the composition of the OSPool can change from day to day, 
we do not know exactly what specific GPUs are
available at any given time. Based on previous GPU job executions, 
you might land on one of the following types of GPUs:

* GeForce GTX 1080 Ti (GPUs\_Capability: 6.1)
* V100 (GPUs\_Capability: 7.0)
* GeForce GTX 2080 Ti (GPUs\_Capability: 7.5)
* Quadro RTX 6000 (GPUs\_Capability: 7.5)
* A100 (GPUs\_Capability: 8.0)
* A40 (GPUs\_Capability: 8.6)
* GeForce RTX 3090 (GPUs\_Capability: 8.6)

# Software and Data Considerations

## Software for GPUs

For GPU-enabled machine learning libraries, we recommend using 
containers to set up your software for jobs: 

  * [Using Containers on the OSPool](../../../htc_workloads/using_software/available-containers-list/)
  * [Sample TensorFlow GPU Container Image Definition](https://github.com/opensciencegrid/osgvo-tensorflow-gpu/blob/master/Dockerfile)
  * [TensorFlow Example Job](../../../software_examples/machine_learning/tutorial-tensorflow-containers/)

## Data Needs for GPU Jobs

As with any kind of job submission, check your data sizes (per job) before submitting 
jobs and choose the appropriate file transfer method for your data. 

See our [Data Staging and Transfer guide](../../../htc_workloads/managing_data/osgconnect-storage/) for
details and contact the Research Computing Facilitation team with questions.
