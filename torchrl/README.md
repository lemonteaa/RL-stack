# Torchrl

(Assuming CPU version)

To build (run in this folder):

`docker build -t my-torchrl-cpu -f cpu/Dockerfile .`

To run:

`docker run -it --rm -p 8888:8888 my-torchrl-cpu`

See token in the terminal output to login to Jupyterhub.

## Remarks

Just some quick key points, will re-write more systematically:

- `torchrl` currently only compatible with legacy `gym` (See [Incompatibility between TorchRL and Gymnasium 1.0: Auto-Reset Feature Breaks Modularity and Data Integrity](https://github.com/pytorch/rl/discussions/2483))
- Current base image (`quay.io/jupyter/base-notebook`) uses `python 3.12`, and legacy `gym` is too old with broken dependencies when installing extras such as `"gym[mujoco]"` or `"gym[box2d]"`(?). Solution: Manual installations of individual extra packages.
- When using CPU + `mujoco`, need to add environment variable to configure it to use to `osmesa` backend
- For our current base package, need to start as root and add env `GRANT_SUDO=yes` to be able to have passwordless sudo (See: https://github.com/jupyter/docker-stacks/issues/949 and https://jupyter-docker-stacks.readthedocs.io/en/latest/using/recipes.html#using-sudo-within-a-container)
- Install system packages for the `osmesa` backend (need to run `apt-get` etc in non-interactive mode as some packages ask for configuration during install (!))
- Install core `torch` separately to be able to use the CPU version index for those packages only (`pip` doesn't seem to support fine-grained disambiguation of the index URL to use according to doc)

## TODO

- GPU variant?
- Support box 2D also: system packages: `build-essential`, pip packages: `"box2d-py==2.3.5" swig pygame`
