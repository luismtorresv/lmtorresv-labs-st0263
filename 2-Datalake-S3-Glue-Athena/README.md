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
