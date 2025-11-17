# Laboratorios de Big Data

## Metadata

<table>
    <tbody>
        <tr>
            <td>Código del curso</td>
            <td>ST0263</td>
        </tr>
        <tr>
            <td>Nombre del curso</td>
            <td>Sistemas Distribuidos</td>
        </tr>
        <tr>
            <td>Estudiante</td>
            <td>
                Luis Miguel Torres Villegas (<tt>lmtorresv[at]eafit.edu.co</tt>)
            </td>
        </tr>
        <tr>
            <td>Profesor</td>
            <td>Edwin Nelson Montoya Múnera (<tt>emontoya[at]eafit.edu.co</tt>)
        </tr>
    </tbody>
</table>



## Breve descripción de la actividad

El propósito de estos laboratorios es desarrollar competencias básicas en el
procesamiento de datos masivos con plataformas de computación distribuida.

### Configurando el clúster

La primera parte se trata de configurar un clúster de **Elastic MapReduce
(EMR)** usando la consola de AWS. Esta se borra pasadas las cuatro horas que nos
provee el servicio de AWS Academy. Para no tener inconvenientes con esto,
creamos un bucket de **AWS S3** para guardar los resultados del trabajo (como si
fuera un datalake). Probaremos que podemos correr Spark desde JupyterHub
y Zeppelin.

### Cargando los datos

La segunda parte se trata de conectarse al nodo principal del clúster EMR para
realizar operaciones comunes. Por ejemplo, copiar archivos o directorios que
están en S3 al **Hadoop Distributed File System (HDFS)**. Estas operaciones las
haremos tanto por una terminal, a través de una conexión SSH, como por la
interfaz gráfica que nos proporciona **Hue** (que incluimos al crear el
clúster).

### Catalogación y consulta de datos

En la tercera parte, configuramos el servicio de catalogación de datos **AWS
Glue** para que lea nuestros datos guardados en S3 e infiera automáticamente el
tipo de dato. Una vez hecho esto, pasamos a usar el motor de consultas SQL **AWS
Athena** para realizar diferentes consultas sobre los datos, como si fuera una
base de datos relacional.

### Cuadernos interactivos

En la cuarta parte, entramos a **JupyterHub** y a **Google Colab** para realizar
consultas usando el clúster EMR ya creado con **Apache Spark**. Realizaremos
procesamiento básico de datos del COVID-19 en Colombia en ambos entornos. Al
final, quedaremos con datos en tanto S3 como en **Google Drive** (por lo de
Colab).
