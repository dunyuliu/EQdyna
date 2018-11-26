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

Here is an example of a batch file to submit jobs on ADA. The key word is BSUB and contexts after ## is commented.

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

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

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
