# Setup Kafka + Spark

## Prérequis
- Docker + Docker Compose
- Node.js (front)
- Python 3.8+ (back + spark)

## Étape 1 : Lancer Kafka + Zookeeper

```bash
cd src/kafka
docker-compose up -d
```

Vérifiez que ça tourne :
- Zookeeper: http://localhost:2181
- Kafka: localhost:9092
- Kafka UI: http://localhost:8080

## Étape 2 : Backend + Producteur Kafka

```bash
cd src/back
pip install -r requirements.txt
python app.py
```

Le backend écoute sur `http://localhost:5000`

## Étape 3 : Job Spark Streaming

Dans un nouveau terminal :

```bash
cd src/spark
pip install -r requirements.txt
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.0 streaming_job.py
```

Spark écoute Kafka et envoie les KPI au backend toutes les 10 sec.

## Étape 4 : Frontend React

Dans un nouveau terminal :

```bash
cd src/front
npm install
npm start
```

L'app s'ouvre sur http://localhost:3000

---

## Test complet

1. **Backend** envoie une commande via POST `/commands`
2. **Producteur Kafka** met la commande dans le topic `commands`
3. **Spark Streaming** lit, agrège et calcule les KPI
4. **Frontend** affiche les KPI en temps réel

### Test manuel

```bash
curl -X POST http://localhost:5000/commands \
  -H "Content-Type: application/json" \
  -d '{"user_id":"user_1","products":["Laptop"],"total_price":999}'
```

Puis créez d'autres commandes via le formulaire. Observez les KPI mis à jour !

---

## Arrêter tout

```bash
docker-compose down  # Arrête Kafka
pkill -f streaming_job.py  # Arrête Spark
pkill -f app.py  # Arrête Flask
```