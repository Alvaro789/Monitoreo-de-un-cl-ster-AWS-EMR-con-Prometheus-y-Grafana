# Monitoreo de un clúster AWS EMR con Prometheus y Grafana

El objetivo de esta práctica es:

1. Configurar un clúster AWS EMR.
2. Exponer métricas del clúster utilizando JMX Exporter.
3. Recopilar y almacenar métricas utilizando Prometheus.
4. Visualizar las métricas en Grafana.

---

## 📌 Parte 1: Configuración del clúster AWS EMR

### 1.1 Crear un clúster EMR

Accede a la consola de AWS y crea un clúster EMR con las siguientes características:

- **Nombre del clúster**: `EMR-Monitoring-iabd10`
- **Aplicaciones**: Hadoop, Spark y Hive
- **Configuraciones**: Las mismas que las utilizadas en otras prácticas previas con AWS EMR
- **Número de instancias**: 1 nodo maestro y 2 nodos core
- **Clave SSH**: Utilizar la clave proporcionada por defecto en el laboratorio (`ls`)
- **Bucket**: Elegir uno previamente creado
- **Tiempo de vida**: Configurar auto-terminación en 4 horas (opcional)

### 1.2 Conectarse al nodo maestro

Conéctate por SSH al nodo maestro utilizando la clave:
ssh -i labsuser.pem hadoop@ec2-54-173-192-73.compute-1.amazonaws.com
## 📌 Parte 2: Configuración de JMX Exporter
### 2.1 Instalamos el JMX Exporter:
wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.16.1/jmx_prometheus_javaagent-0.16.1.jar
### 2.2 Creamos el archivo de configurcion confgi.yml
Comando: nano config.yml

Y agregamos el siguiente contenido:

 lowercaseOutputName: true
 
 lowercaseOutputLabelNames: true
 
 rules:
 
  - pattern: ".*"
### 2.3 Configuramos el namenode para asi poder usar JMX Exporter

o Edita el archivo de configuración del NameNode:

sudo nano /etc/hadoop/conf/hadoop-env.sh

o Agrega la siguiente línea al final del archivo:

export HADOOP_NAMENODE_OPTS="-javaagent:/home/hadoop/jmx_prometheus_javaagent0.16.1.jar=12345:/home/hadoop/config.yml $HADOOP_NAMENODE_OPTS"
### 2.4 Reiniciamos el namenode
Con el comando:
sudo systemctl restart hadoop-hdfs-namenode

## 📌 Parte 3: Despliegue de Prometheus y Grafana
### 3.1 Creamos una instancia EC2 para prometheus y grafana
### 3.2 Instalar Prometheus

Conéctate a la instancia EC2 y sigue los pasos para instalar Prometheus:

wget https://github.com/prometheus/prometheus/releases/download/v2.30.3/prometheus-2.30.3.linux-amd64.tar.gz

tar -xzf prometheus-2.30.3.linux-amd64.tar.gz

cd prometheus-2.30.3.linux-amd64

./prometheus --config.file=prometheus.yml




### 3.3 Configurar Prometheus

Edita el archivo `prometheus.yml` para agregar el clúster EMR como objetivo. Añade la siguiente configuración bajo la sección `scrape_configs`:

scrape_configs:

  - job_name: 'emr-namenode'
    
    static_configs:
    
      - targets: ['3.80.99.53:7070']
### 3.4 Instalar Grafana

sudo apt-get install -y software-properties-common wget

wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

sudo apt-get update

sudo apt-get install grafana

sudo systemctl start grafana-server

sudo systemctl enable grafana-server

### 3.5 Configurar Grafana:
En grafana en un nuevo dashboard tenemos que agregar prometheus como fuente de datos 

http://localhost:9090.

## 📌Parte 4: Visualización de métricas en Grafana
### 4.1. Crear un dashboard en Grafana
Crea un nuevo dashboard y agrega paneles para monitorear métricas 
como:

▪ Uso de CPU y RAM.

▪ Espacio utilizado en HDFS.

▪ Estado del NameNode.

### En el pdf esta documentada todos los pasos realizados y con fotos de cada comando utilizado para la practica






