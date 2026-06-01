FROM condaforge/mambaforge:latest

LABEL Author="rmerritt"
LABEL Version="1.0.0"
LABEL Description="Python 3.10.14 environment with bioinformatics dependencies"

# This container provides a Python 3.10.14 environment with numpy, pandas, 
# requests, fuzzywuzzy, and pysam for bioinformatics workflows
#
# Usage:
#     docker run -it python-bio:latest python your_script.py
#
# Installed packages:
#     - python=3.10.14
#     - numpy
#     - pandas
#     - requests
#     - fuzzywuzzy
#     - pysam

# Disable conda notices to prevent connections to repo.anaconda.com
RUN conda config --set notify_outdated_conda false && \
    conda config --system --set notify_outdated_conda false

# Configure conda channels - ONLY community channels (no Anaconda)
# Add bioconda with higher priority than conda-forge for bioinformatics tools
RUN conda config --add channels bioconda && \
    # Use flexible priority to allow dependency resolution across channels
    conda config --set channel_priority flexible

# Verify channels (should only show bioconda and conda-forge)
RUN conda config --show channels

# Update conda using classic solver to avoid dependency issues
RUN CONDA_NOTIFY_OUTDATED_CONDA=false conda update -n base --solver=classic -c conda-forge -c bioconda conda -y

# Install Python and required packages
# Note: Using single = for version pins (conda syntax)
RUN CONDA_NOTIFY_OUTDATED_CONDA=false conda install -y \
    python=3.10.14 \
    numpy \
    pandas \
    requests \
    fuzzywuzzy \
    pysam

# Install python-Levenshtein for better fuzzywuzzy performance
RUN CONDA_NOTIFY_OUTDATED_CONDA=false conda install -y python-Levenshtein

# Clean up to reduce image size
RUN conda clean --all -y && \
    rm -rf /var/lib/apt/lists/*

# Ensure conda is in PATH
ENV PATH="/opt/conda/bin:${PATH}"

# Disable conda notices to prevent Anaconda connections
ENV CONDA_NOTIFY_OUTDATED_CONDA=false

# Tests to verify installation
RUN echo "Testing Python installation..." && \
    python --version && \
    python -c "import sys; assert sys.version_info[:3] == (3, 10, 14), f'Wrong Python version: {sys.version}'" && \
    echo "Checking installed packages..." && \
    python -c "import numpy; print(f'numpy {numpy.__version__}')" && \
    python -c "import pandas; print(f'pandas {pandas.__version__}')" && \
    python -c "import requests; print(f'requests {requests.__version__}')" && \
    python -c "import fuzzywuzzy; print(f'fuzzywuzzy {fuzzywuzzy.__version__}')" && \
    python -c "import pysam; print(f'pysam {pysam.__version__}')" && \
    echo "All packages installed successfully!"

# Default command when using 'docker run'
ENTRYPOINT ["/bin/bash", "-c"]
CMD ["python"]

# BUILD COMMAND:
# docker build -t python-bio:latest .
#
# RUN COMMAND:
# docker run -it python-bio:latest python
# docker run -v $(pwd):/work -w /work python-bio:latest "python your_script.py"
