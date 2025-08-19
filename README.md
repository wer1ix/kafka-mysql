# CDC con Kafka, MySQL y Debezium

Este proyecto implementa un flujo de **Change Data Capture (CDC)** utilizando:
- **MySQL** como base de datos transaccional  
- **Kafka** como sistema de mensajería  
- **Debezium** como conector para capturar y transmitir cambios  

## 🚀 Requisitos previos
- Docker  
- Docker Compose  
- (Opcional) GitHub CLI si deseas subirlo a GitHub directamente  

## 📂 Estructura del proyecto
```
.
├── docker-compose.yml             # Definición de servicios
├── db_movies_neflix_transact.sql  # Script SQL inicial de la base de datos
├── register-mysql.json            # Define el conector de Debezium para capturar y transmitir sus cambios en tiempo real.
└── README.md
```

## 🛠️ Servicios definidos
- **MySQL (`mysql-container`)**  
  - Puerto: `3307` (mapeado a `3306` interno)  
  - Inicializa con el script `db_movies_neflix_transact.sql`  
- **Kafka (`kafka`)**  
  - Puerto: `9092`  
  - Broker configurado como único nodo  
- **Debezium (`debezium`)**  
  - Puerto: `8083`  
  - Conector para capturar cambios de MySQL y enviarlos a Kafka  

Todos los servicios se conectan en la red `cdc-network`.

## ▶️ Cómo iniciar
1. Levanta los servicios:
   ```bash
   docker-compose up -d
   ```

2. Verifica que estén corriendo:
   ```bash
   docker-compose ps
   ```

3. Logs de un servicio específico:
   ```bash
   docker logs -f mysql-container
   docker logs -f kafka
   docker logs -f debezium
   ```

## 🔍 Consumir mensajes de Kafka
Ejemplo para leer desde el topic `netflix.db_movies_netflix_transact.Movie`:

```bash
docker exec -it kafka   /opt/bitnami/kafka/bin/kafka-console-consumer.sh   --bootstrap-server kafka:9092   --topic netflix.db_movies_netflix_transact.Movie   --from-beginning
```

Para filtrar por un valor específico (ejemplo `"John Doe"`):

```bash
... | grep "John Doe"
```

## 🧹 Detener y limpiar
```bash
docker-compose down -v
```
Esto eliminará contenedores, red y volúmenes.

## 📖 Notas
- El archivo SQL debe existir con el nombre correcto (`db_movies_neflix_transact.sql`).  
- Si cambias la estructura de la DB, recuerda recrear los volúmenes para que Debezium detecte los cambios.  

---
✍️ Autora: **Ilse Zubieta**  
📅 Fecha: 19 Agosto 2025
