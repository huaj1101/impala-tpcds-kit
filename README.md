# A TPC-DS like benchmark for Apache Impala

The official and latest TPC-DS tools and specification can be found at
[tpc.org](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp)

## Step 0: Environment Setup

Install the necessary packages:

```
sudo apt install openjdk-8-jdk-headless maven git gcc make flex bison byacc curl unzip patch
```

## Step 1: Generate Data

```
git clone https://github.com/huaj1101/impala-tpcds-kit.git
cd impala-tpcds-kit/tpcds-gen
make
hadoop jar target/tpcds-gen-1.0-SNAPSHOT.jar -d /tmp/tpc-ds/sf/ -p 100 -s 100
```

## Step 2: Load Data


Adjust the source/text and target/Parquet schema names and flat file paths in the sql files found in the `scripts/` directory.  See the comments at the top of each.

Create external text file tables:

```
impala-shell -f impala-external.sql
```

Create Parquet tables:

```
impala-shell -f impala-parquet.sql
```

Load Parquet tables and compute stats:

```
impala-shell -f impala-insert-parquet.sql
```

Create Kudu tables:

```
impala-shell -f impala-kudu.sql
```

Load Kudu tables and compute stats:

```
impala-shell -f impala-insert-kudu.sql
```

## Step 3: Run Queries

Sample queries from the 100GB scale factor can be found in the `queries/` directory.  The `query-templates/` directory contains the Apache Impala TPC-DS query templates which can be used with `dsqgen` (found in the official TPC-DS tools) to generate queries for other scale factors or to generate more queries with different substitution variables.
