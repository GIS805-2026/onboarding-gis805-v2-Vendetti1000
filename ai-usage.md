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

### 2026-05-14 — Séance S02
- **Modèle :** Claude Code (claude-sonnet-4-6) via extension VS Code
- **Prompt :**
  1. « On commence avec le schéma — est-ce que les tables ont des colonnes qui s'appellent ID ? »
  2. « D'après la théorie, c'est mieux de garder la clé naturelle ou bien créer une clé homemade ? »
  3. « On va garder les clés naturelles jusqu'à preuve du contraire — le système ne changera pas pour la durée du cours. »
  4. « Le grain est déjà déterminé par fact_sales non ? C'est une ligne, une transaction ? »
  5. « La question du CEO S01, c'était quoi ? J'ai l'impression qu'il manque des dimensions. »
  6. « Je crois que ce serait une bonne idée de mettre une dimension catégorie et une dimension région/géographique — on a ça déjà ou on doit les ajouter ? »
  7. « On va éviter le snowflake et on prévoit faire des tables dénormalisées dans chaque dim. »
  8. « Avec cette requête, on sera capable de répondre à la question du CEO S01 plus tard ? »
  9. « Le CEO est encore en mode exploratoire — restons en mode diagnostique. »
  10. « Tu peux me donner la question CEO du S03 juste pour voir comment s'y préparer ? »
- **Résultat :** Claude a confirmé la structure des clés dans les tables raw_*, expliqué le choix entre clés naturelles et clés de substitution, précisé que le grain est une ligne de commande et non une transaction, identifié les 6 dimensions du schéma (incluant dim_geography), généré le diagramme Mermaid en étoile, produit la requête SQL de preuve, et rédigé les sections du brief S02 (grain, réponse exécutive, décisions de modélisation, preuve, validation, risques, recommandations).
- **Validation :** L'étudiant a guidé activement les décisions : proposition du style Kimball, choix d'éviter le snowflake, questionnement sur ce qui est prouvé vs donné comme contexte, anticipation de S03 pour évaluer l'impact sur S02. Les sections du brief ont été approuvées et ajustées avant insertion. Le diagramme Mermaid a été validé visuellement via la preview VS Code.
- **Justification :** Claude a servi d'accélérateur et de validateur — l'étudiant maîtrise les principes (Kimball, grain, dénormalisation, dimensions conformes) et utilise l'IA pour confirmer ses intuitions, couvrir les angles morts et structurer la documentation. La terminologie technique avancée (SCD, fonctions de fenêtre, clés de substitution) est en cours d'apprentissage.

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
