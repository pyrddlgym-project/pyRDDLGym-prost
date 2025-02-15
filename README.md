# pyRDDLGym-prost

[![Documentation Status](https://readthedocs.org/projects/pyrddlgym/badge/?version=latest)](https://pyrddlgym.readthedocs.io/en/latest/prost.html)
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

[Instructions](#instructions) | [Citing](#citing-pyrddlgym-prost)

Contains the Docker files for connecting the Monte-Carlo Tree Search planner [PROST](https://github.com/prost-planner/prost) with pyRDDLGym.

> [!NOTE]  
> PROST only works with finite state and action domains only.
> If you wish to do planning for continuous state and/or action spaces, check out the [gradient-based JAX planner](https://github.com/pyrddlgym-project/pyRDDLGym-jax) or the [deep reinforcement learning wrappers](https://github.com/pyrddlgym-project/pyRDDLGym-rl).
  
## Instructions

### Setting Up

First, you will need to download the necessary Docker files and run scripts packaged with this repository. You do not need to download or install PROST, as this is handled by the Dockerfile:

```shell
git clone https://github.com/pyrddlgym-project/pyRDDLGym-prost /path/to/dockerfiles
cd /path/to/dockerfiles/prost
```

In the above example, it is assumed that ``/path/to/dockerfiles`` is a valid path on the file system where the project files will be cloned into.

After executing the above commands, the folder should now contain:
- a ``Dockerfile`` with instructions for Docker to build the image
- ``prost.sh`` file that calls PROST from the command line
- ``rddlsim.py`` file that runs ``prost.sh`` from Python, and
- ``runprost.sh`` file that you can use to automate the build and run process (described below).

### Building the Image

To build the Docker image, you will need to install [Docker](https://docs.docker.com/get-docker/). Then, with Docker running, build the image as follows:

```shell
docker build -t prost .
```

### Running the Container

To run a container from the built image:

```shell
docker run --name <container name> --mount type=bind,source=<rddl dir>,target=/RDDL prost <rounds> "<prost args>"
```

where:
- ``<container name>`` is the name of the container you want to use
- ``<rddl dir>`` is the path of the directory containing the RDDL ``domain.rddl`` and ``instance.rddl`` files you wish to run
- ``<rounds>`` is the number of runs/episodes/trials of optimization
- ``<prost args>`` are the arguments to pass to PROST, whose syntax is described [here](https://github.com/pyrddlgym-project/pyRDDLGym-prost/blob/main/prost/PROST_Command_Line_Option_Notes_Thomas_Keller.txt)

After the container runs, you can then copy the files from the container to a directory ``<output dir>`` in your local filesystem for further analysis:

```shell
docker cp <container name>:/OUTPUTS/ <output dir>
```

### Automated Script

You do not need to run the two commands described in the previous section, since we have provided a script to automate the process:

```shell
bash runprost.sh <container name> <rddl dir> <rounds> <prost args> <output dir>
```

where the arguments are as described above.

## Citing pyRDDLGym-prost

PROST was written by Thomas Keller. If you use it in your research, please cite:

```
@inproceedings{keller2012prost,
  title={PROST: Probabilistic planning based on UCT},
  author={Keller, Thomas and Eyerich, Patrick},
  booktitle={Proceedings of the International Conference on Automated Planning and Scheduling},
  volume={22},
  pages={119--127},
  year={2012}
}
```

