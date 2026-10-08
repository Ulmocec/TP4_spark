# TP4 — De Cassandra à Spark

## Le sujet
On sort les données Vélib de Cassandra et on les traite avec PySpark. Simple.

## Les données
Stations Vélib de Paris (API Velib), récupérées au TP Cassandra.

- **Keyspace** : `velib_cluster`
- **Table** : `stations_velib`
- **Colonnes utiles** : `nom_station`, `vatiques_disponibles`, `capacite`, `derniere_mise_a_jour`

## La question métier
Quelles stations risquent la rupture de service (trop peu de vélos), et à quelle heure ?

## Ce qu'on fait
1. Connexion Spark → Cassandra, création du DataFrame
2. Sélection des colonnes + filtres (stations presque vides / bien remplies)
3. Colonnes calculées : `pct_dispo`, `niveau_risque` (VIDE / OK / PLEIN)
4. Agrégation par état de remplissage, puis par heure
5. Partitions, Lazy Evaluation, Spark UI

## Observations
- Les transformations ne lancent rien : c'est `show()` / `count()` qui déclenchent les Jobs.
- Partitions : 4 → `repartition(1)` → 1, le shuffle se voit dans la Spark UI (4 Tasks qui écrivent, 1 qui lit).
- Le `groupBy` fait un shuffle → `Exchange` visible dans le plan d'exécution.
- La Lazy Evaluation permet à Spark d'optimiser le plan global avant de calculer.

## Contenu
```
README.md
tp_cassandra_spark.ipynb
TP4_Cassandra_Spark_Donnees_Metier.md
screenshots/
```
