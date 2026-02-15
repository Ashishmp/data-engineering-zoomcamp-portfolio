🚕 NYC Taxi Data Pipeline: Zoomcamp Portfolio
      An end-to-end data engineering pipeline built to ingest and manage large-scale transportation data using modern dev-tooling.



🚀 Executive Summary
      This project automates the ingestion of over 1.3 million rows of NYC Yellow Taxi data into a containerized PostgreSQL database. It solves the common "Out of Memory"          (OOM) problem by utilizing data streaming and chunking patterns, ensuring the pipeline can run on standard consumer hardware.


🛠️ Tech Stack & Tools

      | Category         | Tool             | Purpose                                                        |
      | ---------------- | ---------------- | -------------------------------------------------------------- |
      | Language         | Python 3.x       | Core logic and data processing                                 |
      | Environment      | uv               | High-performance dependency and virtual environment management |
      | Containerization | Docker / Compose | Isolated DB (Postgres) and Management (pgAdmin)                |
      | Data Library     | Pandas           | Memory-efficient data handling and schema mapping              |
      | Database         | PostgreSQL       | Relational storage for structured trip data                    |
      | ORM / Driver     | SQLAlchemy       | Database abstraction and connection pooling                    |


Key Engineering Concepts
 
  1. Memory-Efficient Ingestion
   
    Instead of loading the massive .csv.gz file into RAM, the script uses Pandas Iterators.
    Chunking: Data is processed in batches of 100,000 rows.
    Speed: Utilizing psycopg2-binary for fast, direct communication with the Postgres engine.

  2. Infrastructure as Code (IaC)
   
    The entire environment is spun up with a single command using docker-compose.yaml.
    Persistence: Uses Docker Volumes to ensure data isn't lost when containers stop.
    Networking: Implements custom bridge networking for container-to-container communication (e.g., pgAdmin talking to Postgres via the service alias pgdatabase).

  3. Modern Tooling with uv

    Moved away from legacy pip and venv to uv, reducing environment setup time

  🚦 Getting Started
    
    Prerequisites

    Docker & Docker Compose
    uv (Python manager)

    Installation
    Clone the repository:


```
Bash
git clone https://github.com/Ashishmp/data-engineering-zoomcamp-portfolio.git
Start the database:

Bash
docker-compose up -d
Run the ingestion:

Bash
uv run python ingest_data.py \
  --pg-user=root \
  --pg-pass=root \
  --pg-host=localhost \
  --pg-port=5433 \
  --pg-db=ny_taxi \
  --target-table=yellow_taxi_trips

```

    🛠️ Troubleshooting Log
    
    During development, several real-world engineering hurdles were cleared:
    Port Conflicts: Resolved Port already allocated errors by mapping the host port to 5433 to avoid conflicts with local Postgres instances.
    Schema Integrity: Fixed TIMESTAMP issues where dates were being imported as TEXT by implementing explicit parse_dates in the Pandas reader.
    Dependency Resolution: Handled ModuleNotFoundError by utilizing uv add to maintain a strict pyproject.toml.
