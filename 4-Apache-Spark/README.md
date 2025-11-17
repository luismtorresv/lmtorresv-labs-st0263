# Laboratorio 4: Usando Apache Spark

## Resumen tabular de ejecución de cuadernos

| Cuaderno | Ambiente | Fuente de datos | Evidencia |
|:--|:--|---|:--|
| Data_processing_using_PySpark | EMR | AWS S3 | [emr-s3.ipynb][1] |
| Data_processing_using_PySpark | Colab | Google Drive | [colab-gdrive.ipynb][2] |
| Data_processing_using_PySpark | Colab | AWS S3 | [colab-s3.ipynb][3] |
| spark_colab_ejercicios | EMR | AWS S3 | [emr-s3.ipynb][4] |
| spark_colab_ejercicios | Colab | Google Drive| [colab-gdrive.ipynb][5] |
| wordcount-spark | EMR | AWS S3 | [emr-s3.ipynb][6] |

[1]: <notebooks/Data_processing_using_PySpark/emr-s3.ipynb>
[2]: <notebooks/Data_processing_using_PySpark/colab-gdrive.ipynb>
[3]: <notebooks/Data_processing_using_PySpark/colab-s3.ipynb>
[4]: <notebooks/spark_colab_ejercicios/emr-s3.ipynb>
[5]: <notebooks/spark_colab_ejercicios/colab-gdrive.ipynb>
[6]: <notebooks/wordcount-spark/emr-s3.ipynb>

> [!TIP]
>
> Para ver los archivos de salida generados durante la ejecución de los cuadernos
> en el ambiente de Google Colab, puede acceder
> [al directorio `spark-lab/out` en Google Drive](https://drive.google.com/drive/folders/1bGS_NmMgFfD_NDDIoAr3Q5r20dPeqsoZ?usp=sharing).

## Configuración de Google Colab

### Cargando los datasets

![Cargando los datasets.](screenshots/01-drive-archivos.png)

### Cargando los cuadernos

![Cargando los cuadernos.](screenshots/02-drive-cuadernos.png)

### Dando acceso

![Dando acceso](screenshots/03-drive-acceso.png)

### Verificación

![Verificación](screenshots/04-drive-montado.png)

## Problemas que se presentaron

### `spark_colab_ejercicios`

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
