# Laboratorio 3: Procesamiento SQL con Apache Hive

## Creación de la base de datos

![Creación de base de datos.](screenshots/01-creacion-db.png)

```
INFO  : Compiling command(queryId=hive_20251116212222_0a227532-d9e9-45b0-8d25-caa24192bf5b):
CREATE DATABASE lmtorresv
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Semantic Analysis Completed (retrial = false)
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20251116212222_0a227532-d9e9-45b0-8d25-caa24192bf5b); Time taken: 0.003 seconds
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Executing command(queryId=hive_20251116212222_0a227532-d9e9-45b0-8d25-caa24192bf5b):
CREATE DATABASE lmtorresv
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20251116212222_0a227532-d9e9-45b0-8d25-caa24192bf5b); Time taken: 0.129 seconds
INFO  : OK
INFO  : Concurrency mode is disabled, not creating a lock manager
```

## Creación de la tabla manejada `hdi`

### Query

```sql
USE lmtorresv;

CREATE TABLE HDI (
    id INT,
    country STRING,
    hdi FLOAT,
    lifeex INT,
    mysch INT,
    eysch INT,
    gni INT
)

ROW FORMAT DELIMITED FIELDS
TERMINATED BY ','
STORED AS TEXTFILE
TBLPROPERTIES ("skip.header.line.count"="1");
```

### Confirmación visual

Al terminar la consulta de Hive en Hue:

![Creación de table hdi](screenshots/02-creacion-tabla-hdi.png)

Desde el _Table Browser_ de Hue:

![hdi en Table Browser de Hue.](screenshots/03-hdi-en-table-browser.png)

### Output

```
INFO  : Compiling command(queryId=hive_20251116214257_af2173c3-3c22-46ec-9652-c1623888c8c4):

CREATE TABLE HDI (
    id INT,
    country STRING,
    hdi FLOAT,
    lifeex INT,
    mysch INT,
    eysch INT,
    gni INT
)

ROW FORMAT DELIMITED FIELDS
TERMINATED BY ','
STORED AS TEXTFILE
TBLPROPERTIES ("skip.header.line.count"="1")
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Semantic Analysis Completed (retrial = false)
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20251116214257_af2173c3-3c22-46ec-9652-c1623888c8c4); Time taken: 0.006 seconds
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Executing command(queryId=hive_20251116214257_af2173c3-3c22-46ec-9652-c1623888c8c4):

CREATE TABLE HDI (
    id INT,
    country STRING,
    hdi FLOAT,
    lifeex INT,
    mysch INT,
    eysch INT,
    gni INT
)

ROW FORMAT DELIMITED FIELDS
TERMINATED BY ','
STORED AS TEXTFILE
TBLPROPERTIES ("skip.header.line.count"="1")
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20251116214257_af2173c3-3c22-46ec-9652-c1623888c8c4); Time taken: 0.029 seconds
INFO  : OK
INFO  : Concurrency mode is disabled, not creating a lock manager
```
