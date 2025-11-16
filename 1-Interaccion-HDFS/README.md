# Laboratorio 1: Interacción con el HDFS de Hadoop

## Preparación

### Cargar los datasets al bucket S3

![Carga de datasets a S3.](screenshots/02-carga-datasets.png)

## Conexión vía SSH

Me conecté al nodo principal mediante SSH:

![Conexión por SSH](screenshots/01-ssh.png)

> [!NOTE]
>
> La clave privada `vockey.pem` está en la página de inicio del laboratorio de
> AWS.

```
$ nvim ~/.ssh/vockey.pem
$ chmod 400 ~/.ssh/vockey.pem
$ ssh -i ~/.ssh/vockey.pem hadoop@ec2-3-236-106-227.compute-1.amazonaws.com
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
Last login: Sun Nov 16 02:30:19 2025

EEEEEEEEEEEEEEEEEEEE MMMMMMMM           MMMMMMMM RRRRRRRRRRRRRRR
E::::::::::::::::::E M:::::::M         M:::::::M R::::::::::::::R
EE:::::EEEEEEEEE:::E M::::::::M       M::::::::M R:::::RRRRRR:::::R
  E::::E       EEEEE M:::::::::M     M:::::::::M RR::::R      R::::R
  E::::E             M::::::M:::M   M:::M::::::M   R:::R      R::::R
  E:::::EEEEEEEEEE   M:::::M M:::M M:::M M:::::M   R:::RRRRRR:::::R
  E::::::::::::::E   M:::::M  M:::M:::M  M:::::M   R:::::::::::RR
  E:::::EEEEEEEEEE   M:::::M   M:::::M   M:::::M   R:::RRRRRR::::R
  E::::E             M:::::M    M:::M    M:::::M   R:::R      R::::R
  E::::E       EEEEE M:::::M     MMM     M:::::M   R:::R      R::::R
EE:::::EEEEEEEE::::E M:::::M             M:::::M   R:::R      R::::R
E::::::::::::::::::E M:::::M             M:::::M RR::::R      R::::R
EEEEEEEEEEEEEEEEEEEE MMMMMMM             MMMMMMM RRRRRRR      RRRRRR

[hadoop@ip-172-31-74-184 ~]$
```

## Comandos para listar archivos

### Listar la raíz

```
[hadoop@ip-172-31-74-184 st0263]$ hdfs dfs -ls /
Found 4 items
drwxr-xr-x   - hdfs hdfsadmingroup          0 2025-11-16 01:01 /apps
drwxrwxrwt   - hdfs hdfsadmingroup          0 2025-11-16 01:02 /tmp
drwxr-xr-x   - hdfs hdfsadmingroup          0 2025-11-16 01:01 /user
drwxr-xr-x   - hdfs hdfsadmingroup          0 2025-11-16 01:01 /var
```

### Listar los directorios de usuarios

```
[hadoop@ip-172-31-74-184 st0263]$ hdfs dfs -ls /user
Found 9 items
drwxrwxrwx   - hadoop   hdfsadmingroup          0 2025-11-16 02:56 /user/hadoop
drwxr-xr-x   - mapred   mapred                  0 2025-11-16 01:01 /user/history
drwxrwxrwx   - hdfs     hdfsadmingroup          0 2025-11-16 01:01 /user/hive
drwxrwxrwx   - hue      hue                     0 2025-11-16 01:01 /user/hue
drwxrwxrwx   - livy     livy                    0 2025-11-16 01:17 /user/livy
drwxrwxrwx   - oozie    oozie                   0 2025-11-16 01:01 /user/oozie
drwxrwxrwx   - root     hdfsadmingroup          0 2025-11-16 01:01 /user/root
drwxrwxrwx   - spark    spark                   0 2025-11-16 01:01 /user/spark
drwxrwxrwx   - zeppelin hdfsadmingroup          0 2025-11-16 01:01 /user/zeppelin
```

### Listar el directorio del usuario `hadoop`

```
[hadoop@ip-172-31-74-184 st0263]$ hdfs dfs -ls /user/hadoop
Found 1 items
drwxr-xr-x   - hadoop hdfsadmingroup          0 2025-11-16 02:56 /user/hadoop/datasets
```

### Listar el directorio no existente (todavía) de `datasets`

```
[hadoop@ip-172-31-74-184 st0263]$ hdfs dfs -ls /user/hadoop/datasets
ls: `/user/hadoop/datasets': No such file or directory
[hadoop@ip-172-31-74-184 st0263]$
```

## Crear directorio de datasets

```
[hadoop@ip-172-31-74-184 st0263]$ hdfs dfs -mkdir /user/hadoop/datasets
[hadoop@ip-172-31-74-184 st0263]$
```

## Clonar el repositorio

```
[hadoop@ip-172-31-79-60 ~]$ git clone https://github.com/luismtorresv/st0263.git
Cloning into 'st0263'...
remote: Enumerating objects: 400, done.
remote: Counting objects: 100% (119/119), done.
remote: Compressing objects: 100% (61/61), done.
remote: Total 400 (delta 71), reused 99 (delta 58), pack-reused 281 (from 1)
Receiving objects: 100% (400/400), 37.56 MiB | 38.35 MiB/s, done.
Resolving deltas: 100% (171/171), done.
```

> [!NOTE]
>
> Creé un fork del repositorio. Por eso usa mi cuenta.
>
> Además, moví `datasets` a la raíz (junto con otros cambios).
