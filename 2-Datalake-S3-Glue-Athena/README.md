# Laboratorio 2: Datalake con motor de consulta SQL usando los servicios S3, Glue, Athena

## Ingesta de datos en AWS S3

Ya lo había creado y configurado con acceso público desde
[el laboratorio 0](../0-Creacion-Cluster-EMR/README.md#creación-de-bucket-en-s3).

Repito nuevamente los datos del bucket aquí:

| Nombre | Región | URI |
|---|---|---|
| lmtorresv-datalake | us-east-1 | `s3://lmtorresv-datalake/` |

![`datasets` en S3.](screenshots/01-s3-datasets.png)

Acá específicamente están los datos de la ONU:

![Datos de la ONU.](screenshots/02-s3-onu.png)

## Catalogación

### Creación de la base de datos

Creé la base de datos `onudb` en Glue:

![Base de datos en Glue.](screenshots/03-glue-db.png)

### Creación del crawler

Seleccioné la ubicación de los datos de la ONU en S3:

![Fuente de datos.](screenshots/04-glue-path-s3.png)

Verificación antes de crear el crawler con todos los detalles:

![Crawler de Glue.](screenshots/05-crawler-confirmation.png)

### Corriendo el crawler

Corrí el crawler manualmente:

![C](screenshots/06-crawler-run.png)

Log en AWS CloudWatch:

```
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] BENCHMARK : Running Start Crawl for Crawler onudb-crawler
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] BENCHMARK : Classification complete, writing results to database onudb
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] INFO : Crawler configured with Configuration
{
    "Version": 1,
    "CreatePartitionIndex": true
}
 and SchemaChangePolicy
{
    "UpdateBehavior": "UPDATE_IN_DATABASE",
    "DeleteBehavior": "DEPRECATE_IN_DATABASE"
}
. Note that values in the Configuration override values in the SchemaChangePolicy for S3 Targets.
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] INFO : Created table export in database onudb
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] INFO : Created table hdi in database onudb
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] BENCHMARK : Finished writing to Catalog
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] INFO : Run Summary For TABLE:
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] INFO : ADD: 2
[0f2f2a30-c09d-4485-ba3c-d37e8fe0148b] BENCHMARK : Crawler has finished running and is in state READY
```
