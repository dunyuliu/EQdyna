# EQdyna  

A parallel Finite Element software to model earthquake spontaneous dynamic ruptures. The software is designed to use high-performance computing. It is written in FORTRAN 90 and MPI.

## Features

EQdyna is based on Finite Element Method of solid mechanics and features 
1) 3D under-integrated hexahedral elements stabilized by hourglass control; 
2) explicit central difference time integration for efficient and accurate modeling of wave propagation; 
3) the traction-at-split-node technique to model earthquake faults or fractures;
4) the perfectly matched layer absorbing boundary;
5) the coarse-grained Q model for attenuation of seismic waves;
6) various forms of friction laws including slip-weakening, time-weakening, rate- and state- friction with the aging law, slip law and dynamic weakening;
7) 3D MPI. 

### Initializing environments and compiling on ADA, Texas A&M University, HPC.

```
Module load intel/2018a
make # run makefile to complie EQdyna. An excutable file eqdyna-hyb will be generated. 
```

### Batch file preparation on ADA

Here is an example of the batch file, runeqdyna.txt to submit jobs on ADA. The key word is #BSUB and contexts after # is commented. 

```
#BSUB -J job.name

## send stderr and stdout to the same file 
#BSUB -o mpitest.%J

## login shell to avoid copying env from login session
## also helps the module function work in batch jobs
#BSUB -L /bin/bash

## 30 minutes of walltime ([HH:]MM)
#BSUB -W 04:00

#BSUB -n 72 -M 2700    # 72: total number of cores allocated to the job
#BSUB -R rusage[mem=2700] span[ptile=12] 
# mem: the memory allocated to each core
# span[ptile]: the number of cores used on each computing node

# load intel toolchain
ml intel/2018a
ml
export OMP_NUM_THREADS=1

mpirun -np 72 ./eqdyna-hyb # excuting EQdyna
```

## Job submission on ADA
After submission, the job will be quened and excuated when resources are avalible.
```
Bsub <runeqdyna.txt
```

### Postporcessing

1) run rtp3DMPI.m to plot rupture time contour.
It coverts results frt.txt* to cplot.txt.  
2) run timeanalysis.m to analyze computational times used by various modules.
It processes timeinfo* files.

### History
Version 4.0-201608-  
Version 4.1.1 - 20161004 - Testing on TPV8 - Significant simplification of the system that include 
  1) consolidation of variable declarations; 
  2) movement of all controallable parameters in globalvar.f90.
Version 4.1.2 - 20161004 - Testing on TPV27v2, a plastic benchmark problem - Modifications include 
  1) formma and fromkd in driver.f90 are disabled; 
  2) parameters controlling the plastic yielding are added in globalvar.f90.
Version 4.1.3 - 20161005 - Testing on TPV104 - Modifications include
  1) the rate- and state- friction with aging law and slip law are added;
  2) time expense analysis is added.

### Ajustable parameters for Version 4.1.3
## in globalvar.f90
 C_elastic: 
          0: plastic model; stress tensors should be assigned to every element in the volume in meshgen.f90
          1: elastic model; normal and shear stresses should be assigned to on-fault split nodes;
 C_Nuclea:        
          0: disabled;
          1: allow artificial nucleation;
 friclaw: 
          1: slip weakening;
          2: time weakening;
          3: rate- and state- friction with the aging law;
          4: rate- and state- friction with strong rate-weakening;
 C_Q:
          0: attenuation disabled;
          1: allow frequency-dependent/-independet attenuation of seismi waves; currently only works with uniform element                 size (rat == 1.0) and elastic models;
 C_hg:    
          0：visocus hourglass control;
          1: KF78 hourglass control; recommended;
 nPML:  
          6: default value; number of layers in the Perfetly Matched Layers absorbing boundaries;
 rat: 
          1.025: bufferring ratio
 dx:
          element size;
 
## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
