# Proyecto de DFIR
### ¿Qué es un DFIR?
De la abreviación en inglés Digital Forensics and Incident Response, que significa Análisis Forense Digital y Respuesta a Incidentes.

Es un campo de la seguridad informática especializado en la investigación, identificación y mitigación de incidentes de seguridad cibernética.

### Componentes de Análisis Forense Digital y Respuesta a Incidentes (DFIR)
El DFIR se compone de dos componentes principales:

- **Análisis Forense Digital:** Es la recopilación de información almacenada electrónicamente de modo que se pueda confiar en ella como prueba o respaldar una determinación de un hecho. 
- **Respuesta a incidentes:** Implica investigar y remediar un ciberataque para restaurar los sistemas corporativos a sus operaciones normales.

## Herramientas a utilizar para el proyecto:

## TheHive
Es una plataforma de gestión de incidentes de seguridad de código abierto, diseñada para ayudar a las organizaciones a manejar y responder a incidentes de seguridad de manera eficiente.

### Características principales
- ####  Gestión de casos
Permite la creación de casos de incidentes con todos los detalles necesarios, así como el seguimiento del progreso y la asignación de tareas a diferentes miembros de equipo.

- #### Análisis colaborativo
Los equipos pueden trabajar juntos en tiempo real en la investigación de incidentes, compartiendo información y actualizaciones de manera eficiente.

- #### Integración con Cortex:
TheHive puede integrarse con cortex, un motor de análisis que ejecuta automáticamente una serie de analizadores relacionados con el incidente(archivos, links, IPs, etc.).

### Beneficios de utilizar TheHive
- Eficiencia en la respuesta.
- Automatización.
- Visibilidad y control.
- Ahorro de costos.

### Guía de instalación y configuración
***Antes de continuar se debe aclarar que toda esta guía es para la instalación en Linux Ubuntu.***

Ingresar al link: [Guía de instalación](https://docs.strangebee.com/thehive/installation/step-by-step-installation-guide/)

### Dependencias
Antes de continuar con la instalación, asegúrese de que los siguientes programas ya estén instalados en su sistema:

- Abra una ventana de terminal.
Ejecute el siguiente comando para instalar las dependencias necesarias:

~~~ 
- apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl software-properties-common python3-pip lsb-release
~~~

Asegúrese de que todas las dependencias se instalen correctamente antes de continuar con el proceso de instalación de TheHive.

**Máquina Virtual Java#
Nota Importante:**

Para la seguridad y el soporte a largo plazo, es obligatorio usarlo Corretto Amazon builds, que son compilaciones OpenJDK proporcionadas y mantenidas por Amazon.

***Java versión 8 ya no es compatible.***
Abra una ventana de terminal.
Ejecute los siguientes comandos:
~~~
wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg
echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
sudo apt update
sudo apt install java-common java-11-amazon-corretto-jdk
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment
export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"
~~~
Verifique la instalación ejecutando:

~~~
java -version
~~~
Debería ver una salida similar a la siguiente:

~~~
openjdk version "11.0.12" 2022-07-19
OpenJDK Runtime Environment Corretto-11.0.12.7.1 (build 11.0.12+7-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.12.7.1 (build 11.0.12+7-LTS, mixed mode)
~~~

### Apache Cassandra
Apache Cassandra es un sistema de base de datos altamente escalable y robusto. TheHive es totalmente compatible con la última versión de lanzamiento estable de Apache Cassandra 4.0.x.

Actualización de Cassandra 3.x

La información proporcionada en esta guía se refiere específicamente a instalaciones nuevas. Si actualmente está utilizando Cassandra 3.x y está considerando una actualización, le recomendamos que se refiera a guía dedicada.

Instalación#

Añadir referencias al repositorio Apache Cassandra

Descargue las claves del repositorio de Apache Cassandra usando el siguiente comando:
~~~
wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg
~~~
Agregue el repositorio a su sistema agregando la siguiente línea al **/etc/apt/sources.list.d/cassandra.sources.list*** archivo. Es posible que este archivo no exista y es posible que deba crearlo.
~~~
echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list 
~~~
Instale el paquete

Una vez que se agregan las referencias del repositorio, actualice su índice de paquetes e instale Cassandra usando los siguientes comandos:
~~~
sudo apt update
sudo apt install cassandra
~~~

De forma predeterminada, los datos se almacenan en **/var/lib/cassandra.** Asegúrese de que se establezcan los permisos apropiados para este directorio para evitar cualquier problema con el almacenamiento y el acceso a los datos.

### Configuración
Puede configurar Cassandra modificando la configuración dentro del **/etc/cassandra/cassandra.yaml** archivo.

***NOTA: Si no desea cambiar nada, entonces deje el archivo de configuración tal y como está.***

1. Localice el Archivo de configuración de Cassandra:

Navegue al directorio que contiene el archivo de configuración de Cassandra **/etc/cassandra/.**

2. Editar el cassandra.yaml Archivo:

Abrir el cassandra.yaml archivo en un editor de texto con los permisos apropiados.

3. Configurar Nombre del clúster:

Establecer el cluster_name parámetro al nombre deseado. Este nombre ayuda a identificar el clúster de Cassandra.

4. Configurar Escuchar Dirección:

Establecer el listen_address parámetro a la dirección IP del nodo dentro del clúster. Esta dirección es utilizada por otros nodos dentro del clúster para comunicarse.

5. Configurar la dirección RPC:

Establecer el **rpc_address** parámetro de la dirección IP del nodo para permitir que los clientes se conecten al clúster de Cassandra.

6. Configurar Proveedor de Semillas:

Asegurar el **seed_provider** la sección está configurada correctamente. El seeds el parámetro debe contener la dirección IP(es) del nodo (s) semilla en el clúster.

7.Configurar Directorios:

Establezca los directorios para el almacenamiento de datos, los registros de confirmación, los cachés guardados y las sugerencias según sus requisitos. Asegúrese de que los directorios especificados existan y tengan los permisos apropiados.

8. Guardar los Cambios:

Después de realizar las configuraciones necesarias, guarde los cambios en el cassandra.yaml archivo.

~~~
/etc/cassandra/cassandra.yaml
# content from /etc/cassandra/cassandra.yaml
[..]
cluster_name: 'thp'
listen_address: 'xx.xx.xx.xx' # address for nodes
rpc_address: 'xx.xx.xx.xx' # address for clients
seed_provider:
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
    parameters:
        # Ex: "<ip1>,<ip2>,<ip3>"
        - seeds: 'xx.xx.xx.xx' # self for the first node
data_file_directories:
- '/var/lib/cassandra/data'
commitlog_directory: '/var/lib/cassandra/commitlog'
saved_caches_directory: '/var/lib/cassandra/saved_caches'
hints_directory: 
- '/var/lib/cassandra/hints'
[..]
~~~

### Inicie el servicio

Ejecute el siguiente comando para iniciar el servicio Cassandra:
~~~
sudo systemctl start cassandra
~~~
Asegúrese de que el Servicio se Reinicia Después de Reiniciar:

Habilite que el servicio Cassandra se reinicie automáticamente después de reiniciar el sistema:
~~~
sudo systemctl enable cassandra
~~~

Nota

Cassandra omite la escucha en el puerto 7000/tcp para la comunicación entre nodos y el puerto 9042/tcp para la comunicación con el cliente.

### Elasticsearch
Elasticsearch es un robusto motor de búsqueda e indexación de datos. TheHive lo utiliza para administrar los índices de datos de manera eficiente.

**Nota**

Desde la versión 5.3, TheHive es compatible con Elasticsearch 8.0 y 7.x. Las versiones anteriores de TheHive solo admiten Elasticsearch 7.x.

**Nota**

A partir de TheHive 5.3, para casos de uso avanzados, OpenSearch también es compatible.

**Instalación**

Añadir referencias al repositorio de Elasticsearch

Para agregar claves de repositorio de Elasticsearch, ejecute el siguiente comando:
~~~
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https
~~~

Agregue el repositorio a su sistema agregando la siguiente línea al archivo **/etc/apt/sources.list.d/elastic-7.x.list** Es posible que este archivo no exista y es posible que deba crearlo
~~~
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list 
~~~
Instale el paquete

Una vez que se agregan las referencias del repositorio, actualice el índice de su paquete e instale Elasticsearch utilizando los siguientes comandos:
~~~
sudo apt update
sudo apt install elasticsearch
~~~
 

**Configuración**
Puede configurar Elasticsearch modificando la configuración dentro del **/etc/elasticsearch/elasticsearch.yml** archivo.

**NOTA: Si no desea cambiar nada puede dejar el archivo de configuración tal y como está.**

1. Localice el Archivo de configuración de Elasticsearch:

Navegue hasta el directorio que contiene el archivo de configuración de Elasticsearch **/etc/elasticsearch/**.
2. Editar el archivo elasticsearch.yml:

Abrir el elasticsearch.yml archivo en un editor de texto con los permisos apropiados.
3. Configurar HTTP y Hosts de transporte:

Establecer el http.host y transport.host parámetros a 127.0.0.1 o la dirección IP deseada.
4. Configurar Nombre del clúster:

Establecer el cluster.name parámetro al nombre deseado. Este nombre ayuda a identificar el clúster de Elasticsearch.
5. Configurar el Tamaño de la Cola de Búsqueda del Grupo de Hilo:

Establecer el thread_pool.search.queue_size parámetro al valor deseado, como 100000.
6. Configurar rutas para Registros y Datos:

Establecer el path.logs y path.data parámetros a los directorios deseados, tales como **"/var/log/elasticsearch" y "/var/lib/elasticsearch"**, respectivamente.
7. Configurar X-Pack Security (Opcional):

Si no está utilizando la seguridad de X-Pack, asegúrese de que xpack.security.enabled está configurado para false.
8. Configurar Tipos Permitidos de Script (Opcional):

Si es necesario, establezca el script.allowed_types parámetro para especificar los tipos de script permitidos.
9. Guardar los Cambios:

Después de realizar las configuraciones necesarias, guarde los cambios en el elasticsearch.yml archivo.
10. Opciones JVM Personalizadas:

Crea el archivo /etc/elasticsearch/jvm.options.d/jvm.options si no existe.
11. Opciones JVM Personalizadas:

Interior jvm.options, agregue las opciones de JVM deseadas, como:

-Dlog4j2.formatMsgNoLookups=true
-Xms4g
-Xmx4g
Ajuste la configuración de la memoria (-Xms y -Xmx) según la memoria disponible.

~~~
/etc/elasticsearch/elasticsearch.yml
http.host: 127.0.0.1
transport.host: 127.0.0.1
cluster.name: hive
thread_pool.search.queue_size: 100000
path.logs: "/var/log/elasticsearch"
path.data: "/var/lib/elasticsearch"
xpack.security.enabled: false
script.allowed_types: "inline,stored"
~~~
Info

La creación del índice ocurre durante el inicio inicial de TheHive, que puede tardar algún tiempo en completarse.
Similar a los datos y archivos, los índices deben incluirse en la política de respaldo para garantizar su preservación.
Los índices se pueden eliminar y volver a crear según sea necesario.
 

**Iniciar el servicio**
NOTA: Iniciar Elasticsearch es importante para que THehive y Cortex funcionen.

Ejecute el siguiente comando para iniciar el servicio Elasticsearch:
~~~
sudo systemctl start elasticsearch
~~~
Asegúrese de que el Servicio se Reinicia Después de Reiniciar:

Habilite que el servicio Elasticsearch se reinicie automáticamente después de reiniciar el sistema:
~~~
sudo systemctl enable elasticsearch
~~~

La Instalación y Configuración de Live#
Esta sección proporciona instrucciones detalladas para instalar y configurar TheHive.

 **Instalación y configuración de TheHive**
Todos los paquetes requeridos están disponibles en nuestro repositorio de paquetes. Admitimos paquetes Debian y RPM, así como paquetes binarios en formato zip. Todos los paquetes están firmados usando nuestra clave GPG 562CBC1C con la huella digital 0CD5 AC59 DE5C 5A8E 0EE1 3849 3D99 BB18 562C BC1C.

Para los sistemas Debian, utilice los siguientes comandos:
~~~
wget -O- https://archives.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
~~~

Instale el paquete TheHive utilizando los siguientes comandos:
~~~
echo 'deb [arch=all signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.3 main' |sudo tee -a /etc/apt/sources.listd/strangebee.list
sudo apt-get update
sudo apt-get install -y thehive
~~~
 
**Configuración**

La configuración provista de paquetes binarios está diseñada para una instalación independiente, con todos los componentes alojados en el mismo servidor. En este punto, es crucial ajustar los siguientes parámetros según sea necesario:
~~~
/etc/thehive/application.conf
[..]
# Service configuration
application.baseUrl = "http://localhost:9000" # 
play.http.context = "/"                       # 
[..]
~~~
Las siguientes configuraciones son necesarias para el inicio exitoso de TheHive:

- Configuración de clave secreta
- Configuración de base de datos
- Configuración de almacenamiento de archivos
 
 **Configuración de clave secreta**

La clave secreta se genera y almacena automáticamente en **/etc/thehive/secret.conf** durante la instalación del paquete.


**Base de datos e índice**
De forma predeterminada, TheHive está configurado para conectarse a bases de datos locales de Cassandra y Elasticsearch.
~~~
/etc/thehive/application.conf
# Database and index configuration
# By default, TheHive is configured to connect to local Cassandra 4.x and a
# local Elasticsearch services without authentication.
db.janusgraph {
storage {
    backend = cql
    hostname = ["127.0.0.1"]
    # Cassandra authentication (if configured)
    # username = "thehive"
    # password = "password"
    cql {
    cluster-name = thp
    keyspace = thehive
    }
}
index.search {
    backend = elasticsearch
    hostname = ["127.0.0.1"]
    index-name = thehive
}
}
~~~

Almacenamiento de archivos#
La ubicación de almacenamiento de archivos predeterminada de TheHive es **/opt/thp/thehive/files**

Si decide almacenar archivos en el sistema de archivos local:

Asegúrese de que el usuario de hive tenga permisos en la carpeta de destino:

~~~
chown -R thehive:thehive /opt/thp/thehive/files
~~~
Valores predeterminados en el archivo de configuración

~~~
/etc/thehive/application.conf
# Attachment storage configuration
# By default, TheHive is configured to store files locally in the folder.
# The path can be updated and should belong to the user/group running thehive service. (by default: thehive:thehive)
storage {
provider = localfs
localfs.location = /opt/thp/thehive/files
}
~~~

#### Cortex y MISP

El archivo de configuración inicial empaquetado con el software contiene las siguientes líneas, que habilitan los módulos Cortex y MISP de forma predeterminada. Si no está utilizando ninguno de estos módulos, simplemente puede comentar la línea correspondiente y reiniciar el servicio.

~~~
/etc/thehive/application.conf
# Additional modules
#
# TheHive is strongly integrated with Cortex and MISP.
# Both modules are enabled by default. If not used, each one can be disabled by
# ommenting the configuration line.
scalligraph.modules += org.thp.thehive.connector.cortex.CortexModule
scalligraph.modules += org.thp.thehive.connector.misp.MispModule
 ~~~

#### Ejecutar
Para iniciar el servicio TheHive y permitirle ejecutarse en el arranque del sistema, ejecute los siguientes comandos en su terminal:

~~~
sudo systemctl start thehive
sudo systemctl enable thehive
~~~
Tenga en cuenta que el servicio puede tardar algún tiempo en comenzar inicialmente.

Después de que el servicio haya comenzado con éxito, inicie su navegador web y navegue hacia 
~~~
http://YOUR_SERVER_ADDRESS:9000/
~~~

Las credenciales de usuario de administrador predeterminadas son las siguientes:
~~~
Username: admin@thehive.local
Password: secret
~~~
Por razones de seguridad, se recomienda encarecidamente cambiar la contraseña predeterminada después de iniciar sesión.

## Cortex
Es una plataforma de análisis y respuesta a incidentes en ciberseguridad desarrollada por TheHive project. Está diseañada para realizar análisis automatizados de datos de seguridad mediante el uso de analizadores y respondedores.
### Analizadores
Permiten ejecutar análisis sobre observables y datos de seguridad. Estos analizadores pueden verificar direcciones IP, dominios, archivos y otros tipos de datos.
### Respondedores
Permiten tomar acción automáticas en respuesta a un incidente de seguridad.
### Instalación y configuración
Si estás usando uno de los apoyados sistemas operativos, utilice nuestro todo en uno script de instalación:

~~~
wget -q -O /tmp/install.sh https://archives.strangebee.com/scripts/install.sh ; sudo -v ; bash /tmp/install.sh
~~~

Este script ayuda con el proceso de instalación en un SO compatible ; el programa también se ejecuta con éxito si se cumplen las condiciones en términos de requisitos de hardware.

**NOTA:** Por mi parte yo lo instalé en un ubuntu con las características de 4GB de memoria RAM, 40GB de almacenamiento y 4 núcleos del procesador y me funcionó de manera normal.
## MISP
Es una plataforma de código abierto diseñada para  facilitar el intercambio de información, la gestión y el análisis de información sobre amenazas y malware en ciberseguridad. MISP es utilizado por organizaciones de seguridad para compartir datos sobre amenazas, indicadores de compromiso(IoC), tácticas, técnicas y procedimientos(TTPs), y otros datos relevantes para mejorar la detección y la respuesta frente a incidentes de seguridad.

### Configuración e instalación
A continuación se presenta la forma más fácil de instalar MISP.
**NOTA:**
Si bien existe la manera manual de instalar MISP no es nada recomendable ya que es un proceso muy tedioso.

Ejecuta este comando:

~~~
wget -O /tmp/INSTALL.sh https://raw.githubusercontent.com/MISP/MISP/2.4/INSTALL/INSTALL.sh
~~~

Luego ejecuta este otro comando:
~~~
bash /tmp/INSTALL.sh -A | tee log.txt
~~~

Se aclara que este proceso toma un cierto tiempo así que se debe de tener paciencia.

