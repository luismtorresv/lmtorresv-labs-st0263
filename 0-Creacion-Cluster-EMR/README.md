# Laboratorio 0: Creación de clúster EMR

## Creación de bucket en S3

Como el clúster EMR guarda los cuadernos de Spark, toma datos de, y guarda sus
logs en un bucket de AWS S3, pues primero fui a crear ese bucket:

| Nombre | Región | URI |
|---|---|---|
| lmtorresv-datalake | us-east-1 | `s3://lmtorresv-datalake/` |

![Creación exitosa del bucket en AWS S3.](screenshots/01-creacion-bucket-s3.png)

Además activé el acceso público al bucket S3 (o, mejor dicho, desactivé el
bloqueo público que impone por defecto AWS):

![Activación del acceso público al bucket S3.](screenshots/02-acceso-publico-bucket-s3.png)

Ya tengo unos cuantos archivos creados — archivos que se necesitan luego al
crear el clúster EMR — que puedo ver listados si invoco AWS CLI sobre el
bucket S3:

```
$ aws s3 ls s3://lmtorresv-datalake/
                           PRE emr-cluster-logs/
2025-11-15 15:03:50        208 emr-cluster-config.json
2025-11-15 15:07:40        124 install-python-libs.sh
```

## Creación del clúster

### Lo esencial

Ahora sí fui a crear el clúster, apoyándome en recursos almacenados
en el bucket S3. Usé esta configuración:

- Versión de Amazon EMR: emr-7.10.0 (como en la guía)
- Paquete de aplicaciones: HCatalog, Hue, Livy, Zeppelin, Flink,
  Hadoop, JupyterEnterpriseGateway, Tez, Hive, JupyterHub, Spark
- Configuración del Catálogo de datos de AWS Glue:
  - [x] Usar para metadatos de la tabla de Hive
  - [x] Usar para metadatos de la tabla de Spark

> [!NOTE]
>
> Estas dos últimas configuraciones son necesarias para que luego al usar
> Hue podamos ver las tablas que maneja Hive. Si uno no las activa, luego
> sucede como en clase que no podíamos ver la tabla.

![Configuración básica del clúster.](screenshots/03-configuracion-esencial-cluster.png)

### Configuración de las instancias

Usé el tipo de instancia de EC2 `m4.large`, pues así lo sugería una guía de los
que hacen el laboratorio de AWS:

![Configuración del clúster.](screenshots/04-instancias.png)
