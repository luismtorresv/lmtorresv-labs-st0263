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

### Configuración de redes

Acá están los grupos de seguridad de EC2 administrados por EMR:

![Redes.](screenshots/05-firewall.png)

### Acciones de arranque (y logs)

Cargué el archivo [`install-python-libs.sh`](install-python-libs.sh) al bucket
S3 y lo configuré como acción de arranque. El enlace en Markdown debería llevar
a los contenidos de ese archivo en el repositorio.

![Acciones de arranque](screenshots/06-acciones_arranque-logs.png)

Además, creé un directorio (o «prefijo») en el bucket de S3 para guardar los
registros del clúster. No es súper importante, pero preferí tener todo en un
mismo sitio a que me creara más buckets.

### Configuración del software

Como explican
[en la documentación de AWS](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-jupyterhub-s3.html),
para tener persistencia de los _notebooks_ se puede configurar el clúster de
JupyterHub en Amazon EMR para que los guarde en AWS S3. Este es el ejemplo que
dan en esa página:

```json
[
    {
        "Classification": "jupyter-s3-conf",
        "Properties": {
            "s3.persistence.enabled": "true",
            "s3.persistence.bucket": "MyJupyterBackups"
        }
    }
]
```

Yo lo adapté en [`emr-cluster-config.json`](emr-cluster-config.json) con el
nombre de mi bucket y lo cargué a S3:

![Configuración del software.](screenshots/07-configuracion-software.png)

## Par de claves y roles

No hay mucho qué decir.

![Par de claves.](screenshots/08-par-claves.png)
![Roles.](screenshots/09-roles.png)

## Creación exitosa

Alrededor de 21 minutos después, el clúster estaba esperando órdenes:

![Creación exitosa.](screenshots/10-cluster-creado.png)

Si uno lo quisiera clonar con AWS CLI, puede usar los comandos en
[`aws-cli-create-cluster-script.bash`](aws-cli-create-cluster-script.bash):

![Recrear con AWS CLI.](screenshots/11-clonar-cluster-aws_cli.png)

## Posconfiguración del clúster

### Activación de acceso público

![Activación de acceso público.](screenshots/12-cluster-publico.png)

### Apertura de puertos

Abrí los siguientes puertos en el grupo de seguridad del nodo maestro:

| Puerto  | Propósito |
|--:|--:|
| 22 | SSH |
| 8888 | Hue |
| 8890 |  Zeppelin |
| 9443 | JupyterHub |
| 9870 | HDFS Name Node |
| 14000 | No sé |

![Abriendo puertos.](screenshots/13-apertura-puertos.png)

## Hue

Entré a la URL de la interfaz gráfica para Hue en el enlace que proporciona la
consola de AWS: <http://ec2-44-221-67-165.compute-1.amazonaws.com:8888/>

Configuré el usuario `hadoop` con una contraseña que no voy a publicar acá
porque tampoco estamos tan a nuestras anchas ;)

> [!NOTE]
>
> Nota por curiosidad y poco relevante:
> AWS en español le dice «Tonalidad» a Hue.

![Hue al inicio.](screenshots/14-hue-inicio.png)
