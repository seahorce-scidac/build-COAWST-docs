# build-COAWST-docs
```build-COAWST-docs``` contains information required to compile the Coupled-Ocean-Atmosphere-Wave-Sediment Transport (COAWST, https://github.com/DOI-USGS/COAWST) Modeling system on high performance computing (HPC) systems. The point of this repository is to simplify producing a COAWST executable because each HPC cluster has unique architecture. As a result, the ```netcdf``` and compiler paths often have to be set by the user, which can be time consuming to a non-expert. Current documentation is for perlmutter (https://docs.nersc.gov/systems/perlmutter/architecture/).

## Ways to compile 
COAWST can be compiled using python or bash. As we are currently interested in ROMS-only applications, our python build script is tailored to ROMS. The key files are ```build_coawst.sh``` and ```build_roms.py```. 

## Perlmutter examples
Examples for each method are found in ```build_bash_pm.md``` and ```build_python_pm.md```. Note that the Python build script is more user friendly, but requires removal of ```MCT``` related options in ```Linux-gfortran.mk``` to work, unless you want to take the time to install ```MCT```. MCT is related to the coupler. An example of how to do that is shown in the bash example. The first steps for both methods require you to login to perlmutter. It is recommended to put COAWST in your ```home``` directory because ```scratch``` is scrubbed frequently. The examples are done in ```scratch``` for demonstration purposes.  
