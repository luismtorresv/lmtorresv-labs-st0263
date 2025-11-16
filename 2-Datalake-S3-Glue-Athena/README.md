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
