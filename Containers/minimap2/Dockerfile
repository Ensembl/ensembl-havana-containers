FROM condaforge/mambaforge:latest
# Alternative: Use mambaforge (recommended - no Anaconda defaults channel)
# mambaforge comes pre-configured with conda-forge only, avoiding Anaconda licensing
#
# If you must use miniconda3:
# FROM continuumio/miniconda3:latest
# (but requires aggressive removal of defaults channel - see RUN section)

LABEL Author="Ryan_Merritt"
LABEL Version="1.0.0"
LABEL Description="Minimap2 container for Ensembl Havana"

# This container provides minimap2 and samtools for Ensembl Havana.
#
# Usage:
#     docker run container minimap2
#     docker run container samtools
#
# Installed packages:
#     - minimap2
#     - samtools

# Disable conda notices to prevent connections to repo.anaconda.com
RUN conda config --set notify_outdated_conda false && \
    conda config --system --set notify_outdated_conda false

# Note: mambaforge already comes with conda-forge configured and no 'defaults' channel
# Configure conda channels - ONLY community channels (no Anaconda)
# Add bioconda with higher priority than conda-forge for bioinformatics tools
RUN conda config --add channels bioconda && \
    # Use flexible priority to allow dependency resolution across channels
    # (strict priority can cause conflicts with base image packages)
    conda config --set channel_priority flexible

# Note: mambaforge already uses libmamba solver by default, no need to set it

# Verify channels (should only show bioconda and conda-forge)
RUN conda config --show channels

# Update conda using classic solver to avoid libxml2 dependency issues
RUN CONDA_NOTIFY_OUTDATED_CONDA=false conda update -n base --solver=classic -c conda-forge -c bioconda conda -y

# Install packages
# Option 1: Install packages directly
RUN CONDA_NOTIFY_OUTDATED_CONDA=false conda install -y \
    minimap2 \
    samtools

# Clean up to reduce image size
RUN conda clean --all -y && \
    rm -rf /var/lib/apt/lists/*

# Ensure conda is in PATH
ENV PATH="/opt/conda/bin:${PATH}"

# Disable conda notices to prevent Anaconda connections
ENV CONDA_NOTIFY_OUTDATED_CONDA=false

# If using a specific conda environment, activate it:
# ENV PATH="/opt/conda/envs/ENVIRONMENT_NAME/bin:${PATH}"

# Add any other environment variables here
# ENV VARIABLE_NAME=VALUE

# Tests to verify installation
RUN echo "Testing conda installation..." && \
    conda --version && \
    # Test installed packages - using 'command -v' is foolproof for all tools
    # (works regardless of whether tool supports --help/--version)
    echo "Checking installed tools..." && \
    command -v minimap2 || { echo "minimap2 not found!"; exit 1; } && \
    command -v samtools || { echo "samtools not found!"; exit 1; }

# Default command when using 'docker run'
ENTRYPOINT ["/bin/bash", "-c"]
CMD ["$@"]
