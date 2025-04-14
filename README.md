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
 ### 1.2 Conectar la nodo maestro 
 - 


