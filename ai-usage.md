# Trace d'usage IA — GIS805

> Chaque interaction significative avec un outil IA doit être documentée ici.
> Ce fichier est **obligatoire** et évalué à chaque remise.

## Format par entrée

```
### YYYY-MM-DD — Séance SXX
- **Modèle :** (ChatGPT-4o, Claude, Copilot, etc.)
- **Prompt :** (copier-coller exact)
- **Résultat :** (résumé de ce que l'IA a produit)
- **Validation :** (comment vous avez vérifié/modifié le résultat)
- **Justification :** (pourquoi cette interaction était nécessaire)
```

---

### 2026-01-XX — Séance S00 *(exemple — supprimez cette entrée quand vous ajoutez les vôtres)*
- **Modèle :** GitHub Copilot Chat
- **Prompt :** « Qu'est-ce qui se trouve dans mon dépôt ? Explique-moi la structure du projet. »
- **Résultat :** Copilot a listé les dossiers principaux (sql/, answers/, data/, docs/) et expliqué le rôle de chacun dans le contexte d'un entrepôt dimensionnel.
- **Validation :** J'ai comparé la réponse avec le README.md et le contenu réel des dossiers — tout correspondait.
- **Justification :** Première prise de contact avec le dépôt ; je voulais comprendre l'organisation avant de lancer les commandes.

<!-- Ajoutez vos entrées ci-dessous -->

### 2026-05-11 — Séance S01
- **Modèle :** Claude Code (claude-sonnet-4-6) via extension VS Code
- **Prompt :**
  1. « Tu peux aller lire dans DuckDB ce qu'on a de disponible ? »
  2. « Quelles tables on a de disponible dans la DB ? »
  3. « Comment on peut valider que j'ai toutes les tables de dispo ? »
  4. « Le CEO me demande quelles catégories de produits déclinent et pourquoi — je dois répondre sous le format d'un executive brief. »
  5. « On ne peut pas répondre directement sans centraliser les données et les traiter. On a un système transactionnel qu'on doit convertir en système... (entrepôt de données ?) »
  6. « Je suggère style Kimball, en étoile, table de fait au centre — pas contre il manque des tables de dimension non ? »
  7. « Pour savoir quelles catégories déclinent on devra jumeler des tables de manière à pouvoir tracer un trendline (du genre moyenne mobile sur 3 mois). On doit aussi pouvoir relier les données par région. Pour répondre au "pourquoi" c'est difficile et ça nécessitera une analyse plus approfondie. »
  8. « Quel serait le code au premier niveau pour simplement rejoindre toutes les tables à la Kimball ? »
  9. « C'est quoi les risques/limites, je comprends pas ce concept. »
- **Résultat :** Claude a exploré la structure de la base NexaMart et identifié 24 tables `raw_*` disponibles dans DuckDB. Il a confirmé que les trois dimensions nécessaires pour répondre à la question du CEO sont présentes : `raw_dim_product` (catégorie), `raw_dim_store` (région) et `raw_dim_date` (trimestre/année), toutes joinables via `raw_fact_sales`. Claude a expliqué la distinction OLTP vs OLAP, identifié que les tables `raw_*` sont des données brutes et non des dimensions Kimball modélisées, produit la requête SQL de jointure de base, et expliqué le concept de risques/limites dans un executive brief. L'ensemble a permis de compléter les 7 sections du brief S01.
- **Validation :** Les concepts couverts étant fondamentaux et bien compris, les réponses de Claude ont été acceptées sans modification majeure. En parallèle, une exploration directe des fichiers CSV a été effectuée pour confirmer la structure des données disponibles.
- **Justification :** Claude a servi à valider la vue d'ensemble du dépôt, à couvrir les angles morts et à s'assurer que toutes les sections attendues pour S01 étaient complètes et bien formulées. Claude a également assisté à la rédaction de ce fichier `ai-usage.md` en structurant les prompts, résultats et validations de la session.
