# CHoRUS EHR Extract

`ehr-extract.ipynb` exports the [OMOP CDM](https://ohdsi.github.io/CommonDataModel/)
tables from the CHoRUS enclave Postgres database to zstd-compressed Parquet for
packaging into an RO-Crate. It does not transform the data.

## Setup

```sh
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env    # then fill it in
```

`.env` takes `POSTGRES_USER`, `POSTGRES_PASSWORD` and `POSTGRES_HOST`, plus
optional `POSTGRES_PORT` (default 5432) and `POSTGRES_DB`.

## Usage

Run **Configuration**, then whichever section you need:

- **Table inventory** — tables in `SCHEMA` with size and estimated row count.
  Worth running first to check free space.
- **Extract to Parquet** — writes every table to `OUTDIR`. Tables over `CHUNKSIZE`
  rows are split into numbered parts (`measurement.00000.parquet`, …).
  Already-exported tables are skipped, so an interrupted run resumes on re-run.
- **TSV samples** — optional; first `SAMPLE_ROWS` rows of each table to
  `OUTDIR/samples/`.

`SCHEMA` (`omopcdmv2`), `OUTDIR`, `CHUNKSIZE` and `SAMPLE_ROWS` are set in the
second cell.

## Publishing

Clear cell outputs before committing, since they can contain patient data:

```sh
jupyter nbconvert --clear-output --inplace ehr-extract.ipynb
```

Zenodo archives a specific tag, so cite a tagged release rather than the branch
tip.

## License

[MIT](LICENSE)
