# Laboratorio 4: Usando Apache Spark

## Ejecución de cuadernos

### Resumen tabular con enlaces a evidencias

| Cuaderno | Ambiente | Fuente de datos | Evidencia |
|:--|:--|---|:--|
| Data_processing_using_PySpark | EMR | AWS S3 | [emr-s3.ipynb][1] |
| Data_processing_using_PySpark | Colab | Google Drive | [colab-gdrive.ipynb][2] |
| Data_processing_using_PySpark | Colab | AWS S3 | [colab-s3.ipynb][3] |
| spark_colab_ejercicios | EMR | AWS S3 | [emr-s3.ipynb][4] |
| spark_colab_ejercicios | Colab | Google Drive| [colab-gdrive.ipynb][5] |
| wordcount-spark | EMR | AWS S3 | [emr-s3.ipynb][6] |
| wordcount-spark | Colab | Google Drive | [colab-gdrive.ipynb][7] |

[1]: <notebooks/Data_processing_using_PySpark/emr-s3.ipynb>
[2]: <notebooks/Data_processing_using_PySpark/colab-gdrive.ipynb>
[3]: <notebooks/Data_processing_using_PySpark/colab-s3.ipynb>
[4]: <notebooks/spark_colab_ejercicios/emr-s3.ipynb>
[5]: <notebooks/spark_colab_ejercicios/colab-gdrive.ipynb>
[6]: <notebooks/wordcount-spark/emr-s3.ipynb>
[7]: <notebooks/wordcount-spark/colab-gdrive.ipynb>

> [!TIP]
>
> Para ver los archivos de salida generados durante la ejecución de los cuadernos
> en el ambiente de Google Colab, puede acceder
> [al directorio `spark-lab/out` en Google Drive](https://drive.google.com/drive/folders/1bGS_NmMgFfD_NDDIoAr3Q5r20dPeqsoZ?usp=sharing).

### Configuración de Google Colab

#### Cargando los datasets

![Cargando los datasets.](screenshots/01-drive-archivos.png)

#### Cargando los cuadernos

![Cargando los cuadernos.](screenshots/02-drive-cuadernos.png)

#### Dando acceso

![Dando acceso](screenshots/03-drive-acceso.png)

#### Verificación

![Verificación](screenshots/04-drive-montado.png)

### Problemas que se presentaron

#### `spark_colab_ejercicios`

No fue posible correr el PageRank con el paquete `graphframes`.

Le pregunté a ChatGPT, porque no tengo ni idea de cómo funciona el sistema de
paquetes en Colab o en JupyterHub, y me dijo que:

> ChatGPT said:GraphFrames does not load in Colab’s Spark 4.x runtime because
> GraphFrames’ latest release (0.8.3) is built for Spark 3.5, while Google Colab
> now bundles Spark 4.0.1.
>
> The JVM classloader rejects the JAR, leaving
> `org.graphframes.GraphFramePythonAPI` unavailable, which produces the exact
> error you see.
>
> There is no compatible GraphFrames build for Spark 4.x.
>
> To use GraphFrames in Colab, you must run Spark 3.5.x, not Spark 4.x.

No sé qué tanta raźón tenga, pero intenté varias cosas y ninguna permitió
correrlo con éxito.

### Ejecución de `wc-pyspark.py` en el nodo principal de EMR

#### Verificación de la tarea

```
[hadoop@ip-172-31-70-192 ~]$ cat wc-pyspark.py
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("WordCount").getOrCreate()
sc = spark.sparkContext

files = sc.textFile("s3://lmtorresv-datalake/datasets/gutenberg-small/*.txt")
wc = (
    files.flatMap(lambda line: line.split(" "))
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b)
)
wc.coalesce(1).saveAsTextFile("hdfs:///tmp/wcount1")
```

#### Log del proceso de la tarea

```
[hadoop@ip-172-31-70-192 ~]$ spark-submit --master yarn --deploy-mode cluster wc-pyspark.py
25/11/17 20:47:56 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
25/11/17 20:47:56 INFO DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at ip-172-31-70-192.ec2.internal/172.31.70.192:8032
25/11/17 20:47:57 INFO Configuration: resource-types.xml not found
25/11/17 20:47:57 INFO ResourceUtils: Unable to find 'resource-types.xml'.
25/11/17 20:47:57 INFO Client: Verifying our application has not requested more than the maximum memory capability of the cluster (12288 MB per container)
25/11/17 20:47:57 INFO Client: Will allocate AM container, with 2432 MB memory including 384 MB overhead
25/11/17 20:47:57 INFO Client: Setting up container launch context for our AM
25/11/17 20:47:57 INFO Client: Setting up the launch environment for our AM container
25/11/17 20:47:57 INFO Client: Preparing resources for our AM container
25/11/17 20:47:57 WARN Client: Neither spark.yarn.jars nor spark.yarn.archive is set, falling back to uploading libraries under SPARK_HOME.
25/11/17 20:47:58 INFO Client: Uploading resource file:/mnt/tmp/spark-ac05267c-cb38-4cc5-894a-3b89ae187688/__spark_libs__8977531645018525105.zip -> hdfs://ip-172-31-70-192.ec2.internal:8020/user/hadoop/.sparkStaging/application_1763396678263_0007/__spark_libs__8977531645018525105.zip
25/11/17 20:48:00 INFO Client: Uploading resource file:/etc/spark/conf.dist/hive-site.xml -> hdfs://ip-172-31-70-192.ec2.internal:8020/user/hadoop/.sparkStaging/application_1763396678263_0007/hive-site.xml
25/11/17 20:48:00 INFO Client: Uploading resource file:/etc/hudi/conf.dist/hudi-defaults.conf -> hdfs://ip-172-31-70-192.ec2.internal:8020/user/hadoop/.sparkStaging/application_1763396678263_0007/hudi-defaults.conf
25/11/17 20:48:00 INFO Client: Uploading resource file:/home/hadoop/wc-pyspark.py -> hdfs://ip-172-31-70-192.ec2.internal:8020/user/hadoop/.sparkStaging/application_1763396678263_0007/wc-pyspark.py
25/11/17 20:48:00 INFO Client: Uploading resource file:/usr/lib/spark/python/lib/pyspark.zip -> hdfs://ip-172-31-70-192.ec2.internal:8020/user/hadoop/.sparkStaging/application_1763396678263_0007/pyspark.zip
25/11/17 20:48:00 INFO Client: Uploading resource file:/usr/lib/spark/python/lib/py4j-0.10.9.7-src.zip -> hdfs://ip-172-31-70-192.ec2.internal:8020/user/hadoop/.sparkStaging/application_1763396678263_0007/py4j-0.10.9.7-src.zip
25/11/17 20:48:00 INFO Client: Uploading resource file:/mnt/tmp/spark-ac05267c-cb38-4cc5-894a-3b89ae187688/__spark_conf__15595027511123097589.zip -> hdfs://ip-172-31-70-192.ec2.internal:8020/user/hadoop/.sparkStaging/application_1763396678263_0007/__spark_conf__.zip
25/11/17 20:48:00 INFO SecurityManager: Changing view acls to: hadoop
25/11/17 20:48:00 INFO SecurityManager: Changing modify acls to: hadoop
25/11/17 20:48:00 INFO SecurityManager: Changing view acls groups to:
25/11/17 20:48:00 INFO SecurityManager: Changing modify acls groups to:
25/11/17 20:48:00 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: hadoop; groups with view permissions: EMPTY; users with modify permissions: hadoop; groups with modify permissions: EMPTY
25/11/17 20:48:00 INFO Client: Submitting application application_1763396678263_0007 to ResourceManager
25/11/17 20:48:00 INFO YarnClientImpl: Submitted application application_1763396678263_0007
25/11/17 20:48:01 INFO Client: Application report for application_1763396678263_0007 (state: ACCEPTED)
25/11/17 20:48:01 INFO Client:
         client token: N/A
         diagnostics: AM container is launched, waiting for AM container to Register with RM
         ApplicationMaster host: N/A
         ApplicationMaster RPC port: -1
         queue: root.default
         start time: 1763412480591
         final status: UNDEFINED
         tracking URL: http://ip-172-31-70-192.ec2.internal:20888/proxy/application_1763396678263_0007/
         user: hadoop
25/11/17 20:48:09 INFO Client: Application report for application_1763396678263_0007 (state: RUNNING)
25/11/17 20:48:09 INFO Client:
         client token: N/A
         diagnostics: N/A
         ApplicationMaster host: ip-172-31-76-66.ec2.internal
         ApplicationMaster RPC port: 38557
         queue: root.default
         start time: 1763412480591
         final status: UNDEFINED
         tracking URL: http://ip-172-31-70-192.ec2.internal:20888/proxy/application_1763396678263_0007/
         user: hadoop
25/11/17 20:48:21 INFO Client: Application report for application_1763396678263_0007 (state: FINISHED)
25/11/17 20:48:21 INFO Client:
         client token: N/A
         diagnostics: N/A
         ApplicationMaster host: ip-172-31-76-66.ec2.internal
         ApplicationMaster RPC port: 38557
         queue: root.default
         start time: 1763412480591
         final status: SUCCEEDED
         tracking URL: http://ip-172-31-70-192.ec2.internal:20888/proxy/application_1763396678263_0007/
         user: hadoop
25/11/17 20:48:21 INFO ShutdownHookManager: Shutdown hook called
25/11/17 20:48:21 INFO ShutdownHookManager: Deleting directory /mnt/tmp/spark-0e6b7d3d-49e1-47ff-84ae-a7be82f18dd2
25/11/17 20:48:21 INFO ShutdownHookManager: Deleting directory /mnt/tmp/spark-ac05267c-cb38-4cc5-894a-3b89ae187688
```

```
('', 27298)
('thoroughly', 15)
('themselves', 192)
('them.', 371)
('letter', 312)
('A.', 1456)
('ORIGINALS', 1)
('THEY', 1)
('sum', 59)
('singular', 18)
```

> ![NOTE]
>
> El resultado completo, con las 38 863 filas, está en
> [`output/part-00000`](output/part-00000).
