# MySQL Container

A Docker container providing MySQL client tooling and Python runtime dependencies for nf-PostXref and Ensembl Havana workflows.

## Contents

This container includes:
- **default-mysql-client**: Command-line MySQL client for connecting to MySQL-compatible databases
- **tabix**: Tool for indexing and querying tab-delimited genome position files
- **bgzip**: Block compression utility provided with tabix
- **mysql-connector-python**: Python driver for connecting to MySQL databases
- **openpyxl**: Python library for reading and writing Excel CARS workbooks

## Base Image

Built on `python:3.11-slim` with Debian packages installed using `apt` and Python packages installed using `pip`.

## Building the Container

To build the Docker image:

```bash
docker build -t mysql:latest .
```

Or with a custom tag:

```bash
docker build -t ensembl-havana/mysql:0.1.0 .
```

## Usage

### Running Python

Start a Python session:
```bash
docker run --rm mysql:latest
```

Run a Python script from the current directory:
```bash
docker run --rm -v $(pwd):/data mysql:latest python3 /data/script.py
```

### Running MySQL Client

Connect to a MySQL database:
```bash
docker run --rm mysql:latest mysql -h mysql-host -P 3306 -u mysql-user -p database_name
```

### Running tabix

Query a compressed and indexed file:
```bash
docker run --rm -v $(pwd):/data mysql:latest tabix /data/variants.tsv.gz chr1:1000-2000
```

## Notes

- The container is intended as the shared runtime for nf-PostXref pipeline processes
- The image verifies Python imports and command-line tools at build time
- The container is optimized for size with post-installation cleanup

## Version

- **Container Version**: 0.1.0
- **Author**: Ensembl HAVANA
- **Organization**: Ensembl Havana

## Support

For issues or questions, please contact the Ensembl Havana team.
