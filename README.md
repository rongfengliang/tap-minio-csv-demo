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

