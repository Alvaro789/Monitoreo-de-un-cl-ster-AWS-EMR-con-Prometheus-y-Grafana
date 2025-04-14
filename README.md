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

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.30.3/prometheus-2.30.3.linux-amd64.tar.gz
tar -xzf prometheus-2.30.3.linux-amd64.tar.gz
cd prometheus-2.30.3.linux-amd64
./prometheus --config.file=prometheus.yml


### 3.3 Configurar Prometheus

Edita el archivo `prometheus.yml` para agregar el clúster EMR como objetivo. Añade la siguiente configuración bajo la sección `scrape_configs`:

```yaml
scrape_configs:
  - job_name: 'emr-namenode'
    static_configs:
      - targets: ['<ip-nodo-maestro>:12345']





