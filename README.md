# Monitoreo-de-un-cl-ster-AWS-EMR-con-Prometheus-y-Grafana
El objetivo de esta práctica es: 
1. Configurar un clúster AWS EMR.
2. Exponer métricas del clúster utilizando JMX Exporter.
3. Recopilar y almacenar métricas utilizando Prometheus.
4. Visualizar las métricas en Grafana.

## 📌 Parte 1: Configuración del clúster AWS EMR
  1.1 Crear un cluter EMR:
  Accede a la consola de AWS y crea un clúster EMR con las siguientes 
  características:
  ▪ Nombre del clúster: EMR-Monitoring-iabdXX.
  ▪ Aplicaciones: Hadoop, Spark y Hive.
  ▪ El resto de las configuraciones, las mismas que las adoptadas en 
  otras prácticas sobre AWS EMR
  ▪ Número de instancias: 1 nodo maestro y 2 nodos core.
  ▪ Clave SSH: ls por defecto en el laboratorio.

