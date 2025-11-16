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

## Cargar datos a la tabla `hdi`

Decidí cargarlos usando la CLI. Solo tuve que cambiar los parámetros al comando
`hdfs`, con respecto a como estaban en la guía, para que funcionara:

```shell
[hadoop@ip-172-31-79-60 ~]$ hdfs dfs -cp \
  /user/hadoop/datasets/onu/hdi-data.csv \
  /user/hive/warehouse/lmtorresv.db/hdi
```

Y ahora aparece listado el archivo, tanto en la CLI…

```shell
[hadoop@ip-172-31-79-60 ~]$ hdfs dfs -ls /user/hive/warehouse/lmtorresv.db/hdi
Found 1 items
-rw-r--r--   1 hadoop hdfsadmingroup       9235 2025-11-16 21:52 /user/hive/warehouse/lmtorresv.db/hdi/hdi-data.csv
```

Como en Hue:

![Datos cargados vistos desde Hue.](screenshots/04-datos-cargados.png)

Verifiqué que se reflejaran los cambios en el _Table Browser_:

![Datos cargados vistos desde Table Browser.](screenshots/05-nuevas-estadisticas-hdi.png)

Y comprobé que haya interpretado bien el header con una muestra:

![Muestra de datos en Table Browser.](screenshots/06-sample-hdi.png)

> [!NOTE]
>
> Voy a asumir que _no es necesario_ crear la tabla con los otros
> métodos (como tabla externa con HDFS y S3). Eso está muy
> bien, pero no creo que tenga sentido tumbar esta tabla
> solo para usar esos otros métodos

## Realizar consultas y cálculos sobre la tabla `hdi`

### `gni > 2000`

#### Consulta

```sql
SELECT
    country,
    gni
FROM
    hdi
WHERE
    gni > 2000;
```

#### Confirmación visual

![Consulta simple.](screenshots/07-consulta-simple.png)


#### Output

```
INFO  : Compiling command(queryId=hive_20251116224306_d266db8e-30de-45ba-8c51-14e9bba53475): SELECT
    country,
    gni
FROM
    hdi
WHERE
    gni > 2000
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Semantic Analysis Completed (retrial = false)
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:country, type:string, comment:null), FieldSchema(name:gni, type:int, comment:null)], properties:null)
INFO  : Completed compiling command(queryId=hive_20251116224306_d266db8e-30de-45ba-8c51-14e9bba53475); Time taken: 0.072 seconds
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Executing command(queryId=hive_20251116224306_d266db8e-30de-45ba-8c51-14e9bba53475): SELECT
    country,
    gni
FROM
    hdi
WHERE
    gni > 2000
INFO  : Completed executing command(queryId=hive_20251116224306_d266db8e-30de-45ba-8c51-14e9bba53475); Time taken: 0.0 seconds
INFO  : OK
INFO  : Concurrency mode is disabled, not creating a lock manager
```
