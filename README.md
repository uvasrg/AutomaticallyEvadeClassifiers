   <p align="center"><font color="red"><b>
  For EvadeML-Zoo see: <a href="https://github.com/mzweilin/EvadeML-Zoo">https://github.com/mzweilin/EvadeML-Zoo</a>
   </b></font>
   </p>
  



=============
EvadeML v1.0
=============

------------
An Evolutionary Framework for Evading Machine Learning-based Malware Classifiers
------------

[Weilin Xu](https://github.com/mzweilin), [Yanjun Qi](http://www.cs.virginia.edu/yanjun/), and [David Evans](http://www.cs.virginia.edu/evans/)  
_University of Virginia_

# Installation

Several external libraries are required in the project.

* A modified version of pdfrw for parsing PDF at https://github.com/mzweilin/pdfrw
* Cuckoo Sandbox v1.2 as the oracle at https://github.com/cuckoosandbox/cuckoo/releases/tag/1.2
* Target classifier PDFrate-Mimicus at https://github.com/srndic/mimicus
* Target classifier Hidost at https://github.com/srndic/hidost

# Configuration

Copy the template and change to your own configuration.

```
cp project.conf.template project.conf
vim project.conf
```

# Running


First start the centralized detection agent with pre-defined malware signatures.
```
$ ./utils/detection_agent_server.py ./utils/36vms_sigs.pickle
```

Second, run a program to select several benign PDF files as external genome.
```
$ ./utils/generate_ext_genome.py [classifier_name] [benign_sample_folder] [file_number]
```

Now we can start the main program `./gp.py` with a long list of arguments. The helper script `./batch.py` should be helpful in large scale experiments.
```
./batch.py [classifier_name] [ext_genome_folder] [round_id]
```


# Add a new classifier to evade

Adding more target classifiers to the framework is trivial.

1. Add a wrapper in `./classifiers/` like `pdfrate_wrapper.py::pdfrate()`
2. Implement a fitness function in `./lib/fitness.py` like `fitness_pdfrate()`, and specify a switch in `gp.py`
3. Import the wrapper function in `./utils/detection_agent_server.py` like `pdfrate()`, and extend `query_classifier()` so that the main program could call the detector through `lib.detector.query_classifier()`.
