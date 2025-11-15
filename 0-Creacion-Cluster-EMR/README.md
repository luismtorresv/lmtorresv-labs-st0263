# Laboratorio 0: Creación de clúster EMR

## Creación de bucket en S3

Como el clúster EMR guarda los cuadernos de Spark, toma datos de, y guarda sus
logs en un bucket de AWS S3, pues primero fui a crear ese bucket:

| Nombre | Región | URI |
|---|---|---|
| lmtorresv-datalake | us-east-1 | `s3://lmtorresv-datalake/` |

![Creación exitosa del bucket en AWS S3.](screenshots/01-creacion-bucket-s3.png)
