# LoRA Python Container

A Docker container providing Python 3.10.14 with essential bioinformatics libraries for Ensembl Havana workflows.

## Contents

This container includes:
- **Python 3.10.14**: Specific Python version for reproducible environments
- **numpy**: Numerical computing library
- **pandas**: Data manipulation and analysis
- **requests**: HTTP library for API interactions
- **fuzzywuzzy**: Fuzzy string matching
- **pysam**: Python interface for SAM/BAM/CRAM files
- **python-Levenshtein**: Fast string matching for fuzzywuzzy optimization

## Base Image

Built on `condaforge/mambaforge:latest` with packages installed from community channels (conda-forge and bioconda).

## Building the Container

To build the Docker image:

```bash
docker build -t python-bio:latest .
```

Or with a custom tag:

```bash
docker build -t ensembl-havana/lora-python:1.0.0 .
```

## Usage

### Interactive Python Session

Launch an interactive Python shell:
```bash
docker run -it python-bio:latest python
```

### Running a Python Script

Execute a script from your current directory:
```bash
docker run -v $(pwd):/work -w /work python-bio:latest "python your_script.py"
```

### Running with Data Volume

Mount a data directory and process files:
```bash
docker run --rm -v $(pwd)/data:/data python-bio:latest "python /data/analysis.py"
```

### Using Specific Libraries

Working with BAM files using pysam:
```bash
docker run --rm -v $(pwd):/data python-bio:latest "python -c \"import pysam; bam = pysam.AlignmentFile('/data/input.bam', 'rb'); print(f'Reads: {bam.count()}')\""
```

Data analysis with pandas:
```bash
docker run --rm -v $(pwd):/data python-bio:latest "python -c \"import pandas as pd; df = pd.read_csv('/data/input.csv'); print(df.head())\""
```

## Notes

- The container uses flexible channel priority to resolve dependencies across conda-forge and bioconda
- The container is optimized for size with post-installation cleanup
- All packages are verified during build with automated tests
- Python version is pinned to 3.10.14 for reproducibility

## Version

- **Container Version**: 1.0.0
- **Author**: Ryan_Merritt
- **Organization**: Ensembl Havana

## Support

For issues or questions, please contact the Ensembl Havana team.
