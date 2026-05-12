# Board Brief — S01

## Question du CEO

> « Quelles catégories déclinent dans quelles régions et pourquoi ? »


## Réponse exécutive

Les données NexaMart existent mais sont réparties dans des systèmes transactionnels (OLTP) qui ne permettent pas de répondre directement à la question. Pour identifier les catégories en déclin par région et par trimestre, il faut centraliser et transformer ces données dans un entrepôt de données (data warehouse) structuré selon un modèle dimensionnel — ce qui est précisément l'objectif du projet NexaMart.


## Décisions de modélisation

L'approche retenue est un modèle en étoile (Kimball). La table de fait centrale est `fact_sales` au grain d'une ligne de commande. Cinq dimensions gravitent autour :

- `dim_product` — pour naviguer par catégorie et sous-catégorie
- `dim_store` — pour naviguer par région et province
- `dim_date` — pour naviguer par année, trimestre et mois
- `dim_customer` — pour naviguer par segment de fidélité
- `dim_channel` — pour naviguer par canal de vente

**Ce qui manque :** les tables sources (`raw_*`) existent dans DuckDB, mais les tables dimensionnelles modélisées (`dim_*`, `fact_*`) avec clés de substitution et grain validé ne sont pas encore construites. La transformation de `raw_fact_sales` → `fact_sales` et des `raw_dim_*` → `dim_*` est le travail des séances à venir.

Pour savoir quelles catégories déclinent, il faudra joindre les tables de manière à tracer une tendance dans le temps (par exemple, une moyenne mobile sur 3 mois). Il faudra également relier les données de ventes à la dimension géographique pour isoler les variations par région. Quant au **pourquoi**, la réponse est plus difficile à obtenir et nécessitera une analyse approfondie croisant d'autres sources de données.


## Preuve

```sql
SELECT
    p.category,
    s.region,
    d.year,
    d.quarter,
    SUM(f.line_total)  AS total_revenue,
    SUM(f.quantity)    AS total_units
FROM raw_fact_sales f
JOIN raw_dim_product p ON f.product_id  = p.product_id
JOIN raw_dim_store   s ON f.store_id    = s.store_id
JOIN raw_dim_date    d ON f.order_date  = d.date_key
GROUP BY p.category, s.region, d.year, d.quarter
ORDER BY p.category, s.region, d.year, d.quarter;
```


## Validation

La validation du modèle devra couvrir plusieurs niveaux d'intégrité. D'abord, vérifier l'**absence de valeurs nulles** sur les clés de jointure (`product_id`, `store_id`, `order_date`) dans `fact_sales` — un NULL empêcherait la ligne d'être reliée à sa dimension. Ensuite, s'assurer qu'il n'y a **aucun doublon** sur les clés primaires des dimensions (`product_id` dans `dim_product`, `store_id` dans `dim_store`, etc.) pour garantir l'unicité des enregistrements. Enfin, valider l'**intégrité référentielle** : chaque `store_id` présent dans `fact_sales` doit exister dans `dim_store`, chaque `product_id` dans `dim_product`, et chaque `order_date` dans `dim_date` — sans quoi les jointures produiraient des lignes orphelines et des agrégats incomplets.


## Risques / limites

Plusieurs limites encadrent cette analyse à ce stade. Premièrement, la donnée permet de constater qu'une catégorie baisse, mais pas d'expliquer **pourquoi** — la cause (mauvaise promotion, rupture de stock, pression concurrentielle) nécessitera des sources supplémentaires. Deuxièmement, les données brutes n'ont pas encore été validées — tant que l'intégrité référentielle et l'absence de nulls ne sont pas confirmées, les agrégats restent potentiellement non fiables. Troisièmement, **aucune mesure ni stratégie de moyenne mobile n'a encore été définie** — pour détecter une tendance, il faudra écrire le code et concevoir les transformations qui permettront de calculer une moyenne mobile et de distinguer un déclin réel d'une variation saisonnière normale. Finalement, les tables `raw_*` ne sont pas encore des dimensions Kimball propres — des clés orphelines ou des doublons pourraient fausser les résultats des jointures et des agrégats.


## Prochaine recommandation

La prochaine étape est de formaliser le modèle dimensionnel. Concrètement : transférer les données brutes vers un système analytique (OLAP), construire un schéma en étoile Kimball avec `fact_sales` au centre et les dimensions `dim_product`, `dim_store`, `dim_date`, `dim_customer` et `dim_channel` modélisées proprement. Une fois l'étoile en place, on pourra commencer à créer un entrepôt de données structuré et y greffer des comptoirs logiques (data marts) par domaine — ventes, opérations, marketing — pour répondre aux questions stratégiques de manière répétable et fiable.
