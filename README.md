# image_processing_JRA_internal

This repo is to share, test exemplary image processing steps and pipelines for use within ISIS on GMAP cloud services.

For the time being there are two directories:

* [scripts](./scripts) - collecting sample (shell) scripts and/or markdown commands explaining the workflow
* [notebooks](./notebooks) - jupyter-notebook examples, typically using Python libraries, with explanation


### Docker containers

A set of Docker containers is provided to optimize the use of shared workflows and to remove the overhead
of setting up the software stack.

> All the software installed in the provided (docker) containers is publicly available,
> and the container definitions (aka, _dockerfiles_) are also publicly available through 
> GMAP GitHub org (github.com/europlanet-gmap) `docker-*` repositories.

Detailed information about the use and different flavors for each set of docker images can
be found in their respective repositories in GMAP GitHub org, here below are the containers
we judge more useful for this community:

- `gmap/isis3:jupyterhub`: this container is the same one provided on our premises, [jupyter.europlanet-gmap.eu](https://jupyter.europlanet-gmap.eu)
  * provides ISIS3, Python GIS libraries, GDAL 3.3

### Contributors

Always include information about the environment each notebook/script is necessary to run.
For example, the software versions being used or environment variables if relevant.
In the case of Python-based workflows, it is relevant to provide a conda-environment file, 
pip-requirements (or Pipenv) file next to each notebook.
Or, if processing data on the command-line with ISIS, for example, make sure to include
information about the ISIS version in use (GDAL or whatever feels relevant).

Whenever a notebook or script demands more than one file, do create a directory
with the same name of the script/notebook and include it and the ancillary files
all together in the directory.

Repository structure:

- [`notebooks/`](notebooks/): 
- [`scripts/`](scripts/): (Shell) script examples, typically for batch/parallel jobs

The starting point is examples of command use and command concatenation.

The idea is to use on our Juptyer HUB - or , for users that want to install as documented TBA, locally isis and asp being called via jupyter (as a first MVP interface).

This taking advantage of the library TBA name to provide command line options.

For long jobs, a queing system will be included, TBA.


All files should have a self-explaining name. Markdwon individual files, also with self-explaining names, can be in the scripts directory or in the root.

## Resources

* [Berker, K. LPSC2015 ISIS @ messenger data workshop ](https://messenger.jhuapl.edu/Resources/Workshops-and-Meetings/MESSENGER_MDIS_WorkShop_LPSC2015_Final.pdf)