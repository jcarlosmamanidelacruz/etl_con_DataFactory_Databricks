## ETL con Data Factory y Databricks

## Introducción

En este proyecto, exploraremos el proceso de Extracción, Transformación y Carga (ETL) de datos utilizando tecnologías en la nube de Azure. Nuestro objetivo es diseñar un flujo de trabajo eficiente que nos permita manipular datos desde su origen hasta su destino final, aprovechando las capacidades de Azure Data Factory, Databricks y Azure Database for PostgreSQL.

#### Tecnologías Utilizadas

- Azure Data Factory: Utilizaremos Azure Data Factory para orquestar y automatizar todo el flujo de trabajo ETL. Data Factory nos proporciona herramientas para definir, programar y supervisar las actividades de extracción, transformación y carga de datos.
- Databricks: Emplearemos Databricks para realizar las transformaciones complejas de los datos. Databricks nos ofrece un entorno de análisis unificado que combina Apache Spark con una interfaz colaborativa y amigable para el usuario, lo que facilita la manipulación y procesamiento de grandes volúmenes de datos.
- Azure Database for PostgreSQL: Utilizaremos Azure Database for PostgreSQL como nuestro destino final para los datos transformados. Esta base de datos relacional administrada en la nube nos proporciona un entorno seguro y altamente disponible para almacenar nuestros datos estructurados.

#### Objetivos del Proyecto

1. Configurar Azure Data Factory para gestionar el flujo de datos desde múltiples fuentes hacia nuestro almacén de datos en PostgreSQL.
2. Utilizar Databricks para realizar transformaciones complejas, como limpieza de datos, agregaciones y combinaciones.
3. Establecer conexiones seguras entre todas las tecnologías involucradas y garantizar la integridad y seguridad de los datos en todo momento.
4. Automatizar el proceso de ETL para garantizar la eficiencia y la consistencia en la carga de datos.

#### Fuente de Datos

Para este proyecto, estamos utilizando información de importaciones vehiculares descargadas desde Kaggle. Estos datos se presentan en archivos de .CSV que contienen detalles sobre los vehículos importados, incluyendo características como marca, modelo, año de fabricación, país de origen, etc.

## Creación de recursos

En este paso, creará un grupo de recursos.

 1.- En el menú de la izquierda, seleccione Grupo de recursos y luego Crear.

[![Grupo-de-Recursos.png](https://i.postimg.cc/1Xw6hnCY/Grupo-de-Recursos.png)](https://postimg.cc/zHD3TB1n)

2.- Asignamos un nombre al grupo de recursos y seleccionamos la region.

3.- Revisamos y damos crear.

[![Grupo-de-Recursos-creacion.png](https://i.postimg.cc/fy8sb5r3/Grupo-de-Recursos-creacion.png)](https://postimg.cc/vxxkq7MM)

## Crear una cuenta de almacenamiento

1.- Para ello nos vamos al inicio y seleccionamos cuenta de almacenamiento y damos en crear.

[![cuenta-de-almacenamiento.png](https://i.postimg.cc/NGRqPGGy/cuenta-de-almacenamiento.png)](https://postimg.cc/6T6m8Xvw)

2.- Seleccionamos el grupo de recursos creado anteriormente y le damos un nombre de almacenamiento.

3.- En la opción redundancia seleccionamos Almacenamiento con redundancia local (LRS).

4.- En la pestaña avanzado habilitamos el espacio de nombres jerarquicos.

5.- Damos a la opción Revisar y crear.

[![revisar-y-crear.png](https://i.postimg.cc/jjQKnsWn/revisar-y-crear.png)](https://postimg.cc/VS623PWz)

6.- Creamos un contenedor con el nombre de input.

[![nuevo-contenedot.png](https://i.postimg.cc/6pRVJPSp/nuevo-contenedot.png)](https://postimg.cc/18mVDvsk)

7.- Subimos el archivo importaciones.csv el mismo que encontrán en este repositorio.

[![carga-csv.png](https://i.postimg.cc/0QWKwgrx/carga-csv.png)](https://postimg.cc/LYP8cbz0)

## Crear una factoría de datos

En este paso, creará una factoría de datos y abrirá la interfaz de usuario de Data Factory para crear una canalización en la factoría de datos.

2. Abra Microsoft Edge o Google Chrome. Actualmente, la interfaz de usuario de Data Factory solo se admite en los exploradores web Microsoft Edge y Google Chrome.

[![factaria-de-datos.png](https://i.postimg.cc/kgSwm6D7/factaria-de-datos.png)](https://postimg.cc/NKgR801n)

4. En el menú de la izquierda, seleccione Crear un recurso>Integración>Data Factory:

1. En la página Nueva factoría de datos, en Nombre, escriba jmamanidelacruzADFDatabricks.

El nombre de la instancia de Azure Data Factory debe ser único de forma global. Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos. (Por ejemplo, utilice SuNombreADFTutorialDataFactory).

[![factoria-de-datos-creacion.png](https://i.postimg.cc/rybm7gbR/factoria-de-datos-creacion.png)](https://postimg.cc/K4PxM7mm)

## Crear un servicio de Databricks

Para procesar nuestros datos con Databricks deberemos seguir los siguientes pasos:

1.- Desplegar un servicio de Azure Databricks
2.- Conceder accesos a travéz de Azure Active Directory
3.- Montar un Data Lake en Databricks
4.- Transformar los datos con Databricks
5.- Persistir los datos en una base de datos Postgresql de Azure

[![azure-Databricks.png](https://i.postimg.cc/XvL8rRjN/azure-Databricks.png)](https://postimg.cc/Y44gVyLT)

[![azure-Databricks-admin.png](https://i.postimg.cc/k41PvYcq/azure-Databricks-admin.png)](https://postimg.cc/ygZGVPtb)

## Crear un nuevo Cluster

1.- Nos dirigimos a la opción computo y damos a nuevo computo.

[![azure-Databricks-cluster.png](https://i.postimg.cc/3x5NGWN9/azure-Databricks-cluster.png)](https://postimg.cc/bsgqcyQ2)

Crear cluster

[![creacion-cluster.png](https://i.postimg.cc/9Mv8jzGR/creacion-cluster.png)](https://postimg.cc/YGzf6rDM)

2.- Nos dirigimos a Workspace y creamos un nuevo Notebook

[![nuevo-noteboobk.png](https://i.postimg.cc/K82PLw7M/nuevo-noteboobk.png)](https://postimg.cc/qz16Hm5J)

## Configuración de acceso para DataFactory

1.- Seleccionamos el recurso Azure Active Directory
2.- Nos dirigimos a la parte de App registrations
3.- Damos a New y le asignamos un nombre
4.- Seleccionamos el Single tenant
5.- En la opción Redirect URI, seleccionamos Web y damos a registrar

[![app-registration.png](https://i.postimg.cc/Kz7H2B5K/app-registration.png)](https://postimg.cc/2b6wdqMm)

## Dar acceso a nuestro DataLake

1.- Accedemos a nuestro DataLake y en la parte de Acces Control (IAM) le damos a Add Role Asignment 

2.- Buscamos el Rol de Storage Blob Data Contributor

3.- En la opción Members buscamos la opción app que acabamos de crear

4.- Finalmente damos a Review + Assign

[![add-role-assignmet.png](https://i.postimg.cc/xTXQMjyX/add-role-assignmet.png)](https://postimg.cc/7frjykR4)

[![asignar-role.png](https://i.postimg.cc/cL0LYgTd/asignar-role.png)](https://postimg.cc/ZCsmzq1Q)

## Creación de una base de datos Postgresql

1.- Buscamos el servicio Azure Database for PostgreSQL flexible servers

[![bd-postgresql.png](https://i.postimg.cc/ht5jWyV1/bd-postgresql.png)](https://postimg.cc/tsFyFzSs)

2.- Opción Create

[![bd-configuration01.png](https://i.postimg.cc/MH3yC05M/bd-configuration01.png)](https://postimg.cc/BPDLDKd4)

3.- Creamos un usuario Admin y su constraseña y Review + create

[![bd-configuration02.png](https://i.postimg.cc/prMfRXMx/bd-configuration02.png)](https://postimg.cc/gX4XKp0B)

4.- Configuramos la IP en el Firewall, para conectarnos al servidor desde una dirección IP

[![ip-adres.png](https://i.postimg.cc/7YfK9bPt/ip-adres.png)](https://postimg.cc/56W5N4C8)

5.- Nos conectamos a nuestro servidorer de Postgresql con el IDE de nuestra preferencia

[![Login-postgresql.png](https://i.postimg.cc/VLZkyZ8L/Login-postgresql.png)](https://postimg.cc/dLdYrB3z)

## Configuración de bases de datos importaciones_db

Cree la siguientes base de datos manualmente en su servidor Postgres, el archivo con el script lo encontrara en este repositorio con el nombre de create_tables.sql

importaciones_olap

Puede utilizar su cliente Postgres preferido o ejecutar comandos SQL directamente en la interfaz de línea de comandos.

Modelo ER

[![Modelo-ER.png](https://i.postimg.cc/nLmFDqP4/Modelo-ER.png)](https://postimg.cc/Z9TzkvZn)

## Configuración de Data Factory para la ejecución del notebook de Databricks

1.- Nos dirigimos a la sección de actividades y seleccionamos Notebook y lo arrastramos al lienzo de trabajo de DataFactory

[![databricks-DF.png](https://i.postimg.cc/KY4cky91/databricks-DF.png)](https://postimg.cc/NyqvSZvt)

2.- En la pestaña Azure Databricks creamos un linked service y seleccionamos el Notebook creado dentro de nuestro Databricks

[![DF-databricks.png](https://i.postimg.cc/QMMRd5Qp/DF-databricks.png)](https://postimg.cc/dhzWWhS1)

## Archivo Notebook Proceso para la base de importaciones

Este proceso consta de los siguientes pasos:

1. Conexión al almacenamiento de Azure Blob (Gen2)
Primero, establecemos la conexión al almacenamiento de Azure Blob (Gen2) utilizando las credenciales proporcionadas. Esto nos permite acceder a los archivos de datos ubicados en el almacenamiento.

2. Extracción de datos desde Azure Blob Storage
Una vez establecida la conexión, extraemos el archivo importaciones.csv desde el almacenamiento de Azure Blob (Gen2). Este archivo contiene los datos de importaciones que queremos procesar.

3. Limpieza de datos
Después de extraer los datos, realizamos una limpieza inicial para prepararlos para su procesamiento. Esto puede incluir la eliminación de filas duplicadas, el manejo de valores nulos y cualquier otra tarea necesaria para garantizar la calidad de los datos.

4. Procesamiento ETL
Aplicamos transformaciones adicionales a los datos según sea necesario para cumplir con los requisitos del negocio. Esto puede incluir la agregación de nuevas columnas, cálculos, filtrado de datos y otras operaciones para preparar los datos para su carga en la tabla final.

5. Ingestión en la tabla de importaciones
Finalmente, cargamos los datos procesados en la tabla importacion de nuestra base de datos. Esto asegura que los datos estén disponibles para su análisis y consulta por parte de los usuarios.

Este proceso de ETL garantiza que los datos de importaciones se procesen de manera efectiva y se carguen en la tabla final, lista para su uso en análisis y reporting.

[![cod01-databricks.png](https://i.postimg.cc/2jbRR4Gv/cod01-databricks.png)](https://postimg.cc/bdhF1SKN)

[![cod02-databricks.png](https://i.postimg.cc/3wS6zRDn/cod02-databricks.png)](https://postimg.cc/B8FMKJpK)

[![cod03-databricks.png](https://i.postimg.cc/J0bp74Gw/cod03-databricks.png)](https://postimg.cc/JHzNP8NK)

[![cod04-databricks.png](https://i.postimg.cc/TPzjyv8T/cod04-databricks.png)](https://postimg.cc/t1znffCc)
