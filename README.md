# AlphaFold on HPC at Ibex

This repository contains scripts and instructions for running AlphaFold v2.3.2 on the HPC cluster at Ibex. The provided SLURM script is configured to run AlphaFold with the appropriate environment and resource settings.

## Prerequisites

1. Access to the HPC cluster at Ibex.
2. Modules required:
    - AlphaFold v2.3.2
    - CUDA 12.2
3. Ensure the necessary data directories are available at `/the/path/mentioned/in/the/script`.
4. This script is suitable for sequences up to 600 residues. 

## SLURM Script

The following SLURM script is configured to run AlphaFold on the Ibex cluster. This script has been tested and verified to produce the correct output.

### Script: `run_alphafold.sh`
