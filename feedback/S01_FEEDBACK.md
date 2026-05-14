# Rétroaction automatisée -- S01 (Diagnostic fondamental -- NexaMart kickoff)

_Générée le 2026-05-14T22:23:03+00:00 -- Run `20260514T221333Z-7d34bf6a`_

Ce document est produit par un pipeline reproductible (vérification SQL déterministe + analyse LLM du brief et de la déclaration IA). Une revue humaine précède toujours sa publication. **À ce stade expérimental, aucune note ni étiquette de niveau n'est diffusée : l'objectif est purement formatif.**

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

**Pistes :**
> Votre `db/nexamart.duckdb` est absente ou vide ; la requête a été exécutée contre une **base de référence cohorte** (seed instructeur). Les chiffres retournés ne correspondent donc pas à vos propres données : reconstruisez votre base avec `python src/run_pipeline.py` (ou `.\run.ps1 load`) pour valider vos calculs sur votre seed personnel.

## 2. Rétroaction pédagogique sur le brief

> Bon diagnostic conceptuel et un modèle en étoile clairement énoncé; la brief contient une requête d'agrégation et des vérifications d'intégrité décrites. Il manque toutefois des éléments opérationnels : DDL/SCD explicites, checks automatisés et traces de processus (commits/IA) pour rendre le livrable reproductible et industriel.

### Observations par dimension

**Model quality**
- Observation : La brief indique un modèle en étoile avec fait `fact_sales` au grain d'une ligne de commande et cinq dimensions (`dim_product`, `dim_store`, `dim_date`, `dim_customer`, `dim_channel`).
- Piste d'amélioration : Préciser le traitement des changements historiques (SCD Type 2 vs Type 1) et documenter explicitement les clés substituts et le DDL attendu pour `fact_sales` et chaque `dim_*`.

**Validation quality**
- Observation : Le brief fournit une requête SQL d'agrégation (GROUP BY category, region, year, quarter) et décrit des contrôles d'intégrité (absence de NULLs, doublons, intégrité référentielle).
- Piste d'amélioration : Fournir des requêtes `make check` concrètes (rowcounts attendus, checks NULL, détection des doublons sur le grain) et traiter explicitement les cas non-additifs (ex. unit_price).

**Executive justification**
- Observation : La section 'Réponse exécutive' explique que les données sont dans des systèmes OLTP et recommande de centraliser et transformer en entrepôt dimensionnel pour répondre à la question CEO.
- Piste d'amélioration : Formuler une recommandation décisionnelle plus directe (ex. : 'Approuvez la construction du schéma en étoile v1 et un sprint de 2 semaines pour les transformations prioritaires') et ajouter une estimation d'impact ou un KPI cible.

**Process trace**
- Observation : Aucun historique de commits, note IA ou journal de décision n'est fourni dans le brief.
- Piste d'amélioration : Ajouter un petit journal de décisions et l'historique git (≥3 commits significatifs) et déclarer l'usage d'outils IA/le rôle humain dans les vérifications.

**Reproducibility**
- Observation : Le brief mentionne que les tables `raw_*` existent dans DuckDB mais ne fournit ni README, ni scripts pour cloner et reproduire les transformations.
- Piste d'amélioration : Fournir un README minimal et un script `make run`/`make check` qui permet à un coéquipier de reproduire les checks sur un clone propre.

## 3. Déclaration d'utilisation de l'IA

> La déclaration couvre les outils (modèle et contexte), les étapes d'utilisation, la validation humaine et mentionne des limites. Bien que claire et suffisante pour l'évaluation, veillez à supprimer l'exemple générique (Copilot) si vous n'en avez pas réellement fait usage pour ce livrable.

**Sujets bien couverts dans votre déclaration :**

- outils utilisés (nom + version/modèle)
- à quelle étape l'IA a été utilisée
- comment la sortie a été validée par l'humain
- limites ou erreurs observées

## 4. Pistes d'action pour la prochaine itération

- Aucune correction technique nécessaire. Voir la section 2 pour des pistes d'approfondissement.

---

## 5. Traçabilité

- **Run ID :** `20260514T221333Z-7d34bf6a`
- **Devoir :** `S01`
- **Étudiant·e :** `Vendetti1000`
- **Commit analysé :** `a053d73`
- **Audit (côté instructeur) :** `tools/instructor/feedback_pipeline/audit/20260514T221333Z-7d34bf6a/Vendetti1000/`
- **Prompts (SHA-256) :**
  - `sql_extractor_system` : `90ee9e277de7a27f...`
  - `rubric_grader_system` : `505f32d1d8319d66...`
  - `ai_usage_grader_system` : `81cb7fdf89bda55a...`
- **Fournisseur (rubrique) :** `openai`
- **Fournisseur (IA-usage) :** `openai` (gpt-5-mini-2025-08-07)

_Ce feedback a été produit par un pipeline automatisé et **revu par l'équipe pédagogique avant publication**. Aucun chiffre ni étiquette de niveau n'est diffusé à ce stade expérimental : l'objectif est uniquement formatif. Ouvrez une issue dans ce dépôt pour toute question._
