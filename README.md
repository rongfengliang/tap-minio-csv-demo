# singer minio csv tap  demo

> this project use virtualenv

## How to Running

* Start minio && postgres service

```code
docker-compose up -d
```

* Create Bucket && upload demo csv file

```code
Open http://localhost:9000

Create demo bucket

Create exports directory in demo bucket

Upload my_table.csv file in demo/exports
```

* Init minio csv tap virtualenv

```code
cd s3-tap
python3 -m venv venv
source venv/bin/activate
pip install tap-minio-csv
```

* Init postgres target virtualenv

```code
cd pg-target
python3 -m venv venv
source venv/bin/activate
pip install target_postgres

```

* config tap && target

s3-tap/tap-config.json

```code
{
    "start_date": "2017-11-02T00:00:00Z",
    "bucket": "demo",
    "aws_access_key_id":"dalongapp",
    "aws_secret_access_key":"dalongapp",
    "endpoint_url":"http://localhost:9000",
    "tables": "[{\"search_prefix\":\"exports\",\"search_pattern\":\"my_table.csv\",\"table_name\":\"my_table\",\"key_properties\":\"id\",\"delimiter\":\",\"},{\"search_prefix\":\"exports\",\"search_pattern\":\"my_table2.csv\",\"table_name\":\"my_table2\",\"key_properties\":\"id2\",\"delimiter\":\",\"}]"
}

```

pg-target/target.json

```code
{
    "host": "localhost",
    "port": 5432,
    "dbname": "postgres",
    "user": "postgres",
    "password": "dalong",
    "schema": "copy"
}
```

* Generage catalog 

```code
s3-tap/venv/bin/tap-minio-csv -c s3-tap/tap-config.json -d > catalog.json
```

* Add selected stream

```code
      "metadata": [
        {
          "breadcrumb": [],
          "metadata": {
            "table-key-properties": [
              "id"
            ],
+ "selected":true
          }
        }
```

* Run command

```code
 s3-tap/venv/bin/tap-minio-csv -c s3-tap/tap-config.json -p catalog.json | pg-target/venv/bin/target-postgres -c pg-target/target.json
```