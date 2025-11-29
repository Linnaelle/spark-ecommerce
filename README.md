### Projet E-commerce Big Data

Ce projet met en œuvre l'architecture d'une plateforme e-commerce moderne pour l'entreprise **ShopNow+**, inspirée par des systèmes à fort trafic comme Amazon. L'objectif principal est d'établir un flux de données complet, depuis les interactions utilisateur jusqu'à l'analyse métier en temps réel.

### Objectifs du Projet

* **Concevoir** une architecture résiliente et évolutive pour gérer un volume important d'événements e-commerce.
* **Implémenter** le flux de données complet : Front → Back → Kafka → HDFS → Spark.
* **Générer** des indicateurs clés (chiffre d'affaires, TOP produits, alertes stock) pour la direction de ShopNow+.

### Architecture Technique

| Brique | Rôle | Technologies |
| :--- | :--- | :--- |
| **Front** | Interface utilisateur (catalogue, panier, commande) en React
| **Back** | Logique métier, API, gestion des transactions (produits, stocks, commandes, paiements) en Flask
| **Kafka** | Bus d'événements temps réel pour transporter les actions (ajout au panier, commande payée, maj stock)
| **HDFS** | Stockage historique complet et durable de tous les événements et commandes pour l'analyse Big Data
| **Spark** | Moteur d'analyse pour calculer les indicateurs business et détecter les tendances (ventes, ruptures de stock)

### Flux de Données Principal

1.  **Interaction Client** : L'utilisateur navigue sur le **Front** (ex: `produit_consulté`).
2.  **Logique et Événement** : Le **Back** gère l'action (ex: `commande_validée`) et publie un événement vers **Kafka**.
3.  **Stockage** : Les événements sont ingérés et stockés de manière brute et transformée dans **HDFS**.
4.  **Analyse** : **Spark** lit les données depuis Kafka (streaming) et HDFS (batch) pour calculer les indicateurs métiers.

### Contenu du Dépôt

* `src/front`: Code source de l'application web (Front).
* `src/back`: Implémentation de l'API REST et de la logique métier (Back).
* `src/kafka`: Scripts de configuration des Topics et des producteurs/consommateurs.
* `src/spark`: Jobs d'analyse (calcul CA, détection stock) en streaming et en batch.
* `docs/architecture.png`: Schéma d'architecture global.
* `docs/journal_technique.pdf`: Documentation détaillée des étapes de mise en œuvre.
* `docs/rapport_client.pdf`: Synthèse métier et valeur pour ShopNow+.

### Arborescence

spark-ecommerce/
│
├── docs/
│   ├── architecture.md          # Schéma d'architecture technique
│   ├── setup.md                 # Instructions d'installation
│   └── api.md                   # Documentation API
│
├── src/
│   ├── back/
│   │   ├── app.py
│   │   ├── config.py
│   │   ├── requirements.txt
│   │   └── kafka_producer.py
│   │
│   ├── front/
│   │   ├── package.json
│   │   ├── src/
│   │   │   ├── App.jsx
│   │   │   ├── components/
│   │   │   │   ├── CommandForm.jsx
│   │   │   │   └── KPIDashboard.jsx
│   │   │   └── index.js
│   │   └── public/
│   │
│   ├── kafka/
│   │   ├── docker-compose.yml
│   │   └── topics.sh
│   │
│   └── spark/
│       ├── requirements.txt
│       ├── streaming_job.py
│       └── batch_job.py
│
├── .gitignore
├── README.md
└── docker-compose.yml