# Minimap2 Container

A Docker container providing minimap2 and samtools for Ensembl Havana bioinformatics workflows.

## Contents

This container includes:
- **minimap2**: A versatile sequence alignment program for aligning DNA or mRNA sequences
- **samtools**: Tools for manipulating SAM/BAM/CRAM alignment files

## Base Image

Built on `condaforge/mambaforge:latest` with packages installed from community channels (conda-forge and bioconda).

## Building the Container

To build the Docker image:

```bash
docker build -t minimap2:latest .
```

Or with a custom tag:

```bash
docker build -t ensembl-havana/minimap2:1.0.0 .
```

## Usage

### Running minimap2

Basic alignment:
```bash
docker run --rm -v $(pwd):/data minimap2:latest "minimap2 -ax splice /data/reference.fa /data/reads.fq > /data/output.sam"
```

### Running samtools

View BAM file:
```bash
docker run --rm -v $(pwd):/data minimap2:latest "samtools view /data/alignment.bam"
```

Sort and index:
```bash
docker run --rm -v $(pwd):/data minimap2:latest "samtools sort /data/input.bam -o /data/sorted.bam && samtools index /data/sorted.bam"
```

## Notes

- The container uses flexible channel priority to resolve dependencies across conda-forge and bioconda
- The container is optimized for size with post-installation cleanup

## Version

- **Container Version**: 1.0.0
- **Author**: Ryan_Merritt
- **Organization**: Ensembl Havana

## Support

For issues or questions, please contact the Ensembl Havana team.
