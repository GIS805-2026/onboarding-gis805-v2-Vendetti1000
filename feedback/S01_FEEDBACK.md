# Rétroaction automatisée -- S01 (Diagnostic fondamental -- NexaMart kickoff)

_Générée le 2026-05-15T12:35:31+00:00 -- Run `20260515T122624Z-00a5a04f`_

Ce document est produit par un pipeline reproductible (vérification SQL déterministe + analyse LLM du brief et de la déclaration IA). Une revue humaine précède toujours sa publication. **À ce stade expérimental, aucune note ni étiquette de niveau n'est diffusée : l'objectif est purement formatif.**

> ⚠️ **Avertissement instructeur (à retirer avant publication) :** cette analyse a été générée avec `--skip-pull`. Le contenu correspond au commit local et **n'est peut-être pas la dernière version poussée par l'étudiant·e**.

---

## 1. Vérification automatique de la requête SQL

La requête extraite de votre brief s'exécute correctement et produit la forme attendue. Bon travail sur l'auto-validation.

<details><summary>Requête analysée — cliquez pour déplier</summary>

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

</details>

- Colonnes retournées : `category, region, year, quarter, total_revenue, total_units`
- Correspondance avec les colonnes attendues :
  - `category` → `category`
  - `region` → `region`
  - `quarter` → `quarter`
  - `revenue` → `total_revenue`
- Présence de NULLs dans des colonnes de groupement : `category` =0, `region` =0, `quarter` =0. Pensez à documenter le traitement de ces cas.

## 2. Rétroaction pédagogique sur le brief

> Le brief présente un schéma en étoile approprié et une requête de preuve, et il recommande de formaliser l'entrepôt. Il manque toutefois la traçabilité (commits, note IA) et des artefacts de reproductibilité et quelques précisions techniques (SCD, traitements de cas limites).

### Observations par dimension

**Model quality**
- Observation : Le brief décrit un modèle en étoile avec grain 'ligne de commande' et cinq dimensions (dim_product, dim_store, dim_date, dim_customer, dim_channel).
- Piste d'amélioration : Documenter explicitement le choix du grain (pourquoi 'ligne de commande' vs 'commande') et adresser le traitement des changements historiques (SCD) et des attributs non-additifs comme unit_price.

**Validation quality**
- Observation : Le document fournit une requête SQL d'agrégation sur raw_* et liste des contrôles attendus (absence de NULLs, doublons, intégrité référentielle).
- Piste d'amélioration : Fournir des requêtes check reproductibles (make check) qui traitent les NULLs, valident le grain et vérifient les cas-limites (unit_price non-additif, sommes pondérées).

**Executive justification**
- Observation : La réponse exécutive affirme que les données existent mais nécessitent un entrepôt dimensionnel et recommande de formaliser le modèle pour répondre à la question du CEO.
- Piste d'amélioration : Raccourcir en 150–300 mots, formuler une décision claire pour le board (ex. : approuver la construction du schéma en étoile v1) et indiquer l'impact attendu ou métrique de succès.

**Process trace**
- Observation : Aucun historique de commits, note IA ou journal de décision n'est fourni dans le brief.
- Piste d'amélioration : Ajouter un petit log de processus : au moins 3 commits significatifs et une note IA indiquant outils utilisés et validations humaines.

**Reproducibility**
- Observation : Le brief mentionne que les raw_* existent dans DuckDB mais ne fournit pas de scripts ni d'instructions pour reproduire la transformation.
- Piste d'amélioration : Inclure un README et un script 'check' (DuckDB) permettant à un collègue de cloner et d'exécuter les vérifications en moins de 5 minutes sans chemins codés en dur.

## 3. Déclaration d'utilisation de l'IA

> La déclaration est complète : elle nomme les outils avec modèle, précise les étapes d'utilisation et décrit des validations humaines concrètes. Les limites et risques sont évoqués ; continuez à garder des validations explicites (ex. versions d'extension ou captures de commandes) si vous en ajoutez d'autres.

**Sujets bien couverts dans votre déclaration :**

- outils utilisés (nom + version/modèle)
- à quelle étape l'IA a été utilisée
- comment la sortie a été validée par l'humain
- limites ou erreurs observées

## 4. Pistes d'action pour la prochaine itération

- Aucune correction technique nécessaire. Voir la section 2 pour des pistes d'approfondissement.

---

## 5. Traçabilité

- **Run ID :** `20260515T122624Z-00a5a04f`
- **Devoir :** `S01`
- **Étudiant·e :** `Vendetti1000`
- **Commit analysé :** `0bcbec6`
- **Audit (côté instructeur) :** `tools/instructor/feedback_pipeline/audit/20260515T122624Z-00a5a04f/Vendetti1000/`
- **Prompts (SHA-256) :**
  - `sql_extractor_system` : `90ee9e277de7a27f...`
  - `rubric_grader_system` : `505f32d1d8319d66...`
  - `ai_usage_grader_system` : `81cb7fdf89bda55a...`
- **Fournisseur (rubrique) :** `openai`
- **Fournisseur (IA-usage) :** `openai` (gpt-5-mini-2025-08-07)

_Ce feedback a été produit par un pipeline automatisé et **revu par l'équipe pédagogique avant publication**. Aucun chiffre ni étiquette de niveau n'est diffusé à ce stade expérimental : l'objectif est uniquement formatif. Ouvrez une issue dans ce dépôt pour toute question._
