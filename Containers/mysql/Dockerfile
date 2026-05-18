FROM python:3.11-slim

LABEL Author="Ensembl HAVANA"
LABEL Version="0.1.0"
LABEL Description="nf-PostXref shared pipeline runtime with MySQL client support."

# Shared runtime for nf-PostXref.
#
# Usage:
#     docker run container python3
#     docker run container mysql
#
# Installed packages:
#     - default-mysql-client
#     - tabix
#     - bgzip
#     - mysql-connector-python
#     - openpyxl

RUN apt-get update && apt-get install -y --no-install-recommends \
        default-mysql-client \
        tabix \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir \
        "mysql-connector-python>=8.0.0" \
        "openpyxl>=3.1.0"

ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

# Tests to verify installation
RUN echo "Testing Python package imports..." && \
    python3 -c "import mysql.connector, openpyxl; print('All imports OK')" && \
    echo "Checking installed tools..." && \
    command -v mysql || { echo "mysql not found!"; exit 1; } && \
    command -v tabix || { echo "tabix not found!"; exit 1; } && \
    command -v bgzip || { echo "bgzip not found!"; exit 1; } && \
    tabix --version 2>&1 | head -1

# Default command when using 'docker run'
CMD ["python3"]
