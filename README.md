
# Power BI – Analyse des sinistres et des primes (Projet portfolio)

Ce dépôt fournit un **projet Power BI prêt à l’emploi** pour la BI en assurance IARD (sinistres & primes).
Il inclut un **modèle en étoile**, des **mesures DAX** et un **jeu de données synthétique**. Idéal pour votre
portfolio GitHub et pour démontrer : modélisation de données, DAX, et storytelling analytique.

## 🚀 Objectifs pédagogiques
- Construire un **modèle en étoile** performant pour l’analytique (faits + dimensions).
- Définir des **mesures DAX** clés (Loss Ratio, YoY, gravité moyenne, etc.).
- Mettre en place une page de **dashboard** claire (KPI cards + visualisations).
- Raconter une **histoire métier** (où se concentrent les ouvertures, quelles lignes de produits dégradent le ratio, etc.).

## 📁 Structure du dépôt
```
powerbi-insurance-analytics/
├─ data/                  # CSV à importer dans Power BI
│  ├─ dim_date.csv
│  ├─ dim_region.csv
│  ├─ dim_product.csv
│  ├─ dim_customer.csv
│  ├─ dim_policy.csv
│  ├─ fact_premiums.csv
│  └─ fact_claims.csv
├─ measures/
│  └─ ClaimsMeasures.dax  # Mesures DAX à copier/coller
├─ powerquery/
│  └─ queries.pq          # (Optionnel) Template M + Paramètre dossier
├─ README.md              # README en anglais
├─ README_FR.md           # (CE FICHIER) README en français
└─ layout_guide.png       # Guide visuel de mise en page Power BI
```

## 🧱 Modèle de données (Star Schema)
**Tables de faits**
- `fact_premiums` : primes facturées mensuellement par police
- `fact_claims`   : sinistres (montants payés, encourus, statut)

**Dimensions**
- `dim_date`, `dim_product`, `dim_region`, `dim_customer`, `dim_policy`

**Relations clés (recommandées)**
- `dim_date[DateKey]` → `fact_premiums[DateKey]`, `fact_claims[DateKey]` (1-*)
- `dim_product[ProductID]` → `dim_policy[ProductID]` → `fact_*[PolicyID]`
- `dim_region[RegionID]` → `dim_customer[RegionID]` → `dim_policy[CustomerID]`
- Marquer `dim_date[FullDate]` comme **Date table**.

## 🧮 Mesures DAX (exemples fournis dans `measures/ClaimsMeasures.dax`)
- `Total Premium`, `Total Paid Claims`, `Total Incurred Claims`
- `Loss Ratio`, `Paid Loss Ratio`
- `Claim Count`, `Open Claim Count`, `Average Claim Severity ($)`
- YoY : `Premium LY`, `Incurred LY`, `Loss Ratio Δ YoY (pp)`
- Par produit/LOB et région : `Auto/Home/Pet Loss Ratio`, `Top Regions by Loss Ratio`

## 📊 Mise en page recommandée
Voir **`layout_guide.png`** pour un exemple de disposition :  
- **KPI cards** (haut de page) : Total Premium, Loss Ratio, Claim Count, Open Claim Count
- **Tendance** : Loss Ratio par mois (avec YoY)
- **Comparatif produit** : Premium vs Incurred par LOB (colonnes)
- **Opérations** : Sinistres ouverts par région (carte ou barres)
- **Distribution** : Gravité (histogramme PaidAmount)
- **Ranking** : Top régions par Loss Ratio (table)

## ⚙️ Étapes pour reconstruire le `.pbix`
1. Ouvrir Power BI Desktop → **Get Data → Text/CSV** → importer tous les CSV de `/data`.
2. Créer les relations comme ci-dessus (section *Modèle de données*).
3. **Marquer `dim_date[FullDate]`** comme **table de dates**.
4. Créer une table de mesures et coller le contenu de `measures/ClaimsMeasures.dax`.
5. Construire les visuels selon **layout_guide.png**. Sauvegarder : `Insurance_Analytics.pbix`.
6. (Optionnel) Utiliser `powerquery/queries.pq` et un **Paramètre** `Parameter_DataFolder`
   pour rendre le chemin de données configurable.

## 🧪 Questions d’analyse à traiter dans le README du projet
- Quelle **LOB** (Auto, Home, Pet) dégrade le plus le **Loss Ratio** en tendance ?
- Les **sinistres ouverts** sont-ils concentrés sur certaines **régions/provinces** ?
- La **gravité** varie-t-elle selon l’**ancienneté client** ou l’**âge** ?

## 🔁 Évolution & rafraîchissement
- Remplacer les CSV par vos sources (Snowflake, SQL Server, Azure, etc.).
- Garder une **clé temps** cohérente (`DateKey`) et des **ids** stables pour les relations.
- Ajouter de la **RLS** (ex. par `RegionID`) si besoin.

## 📦 Publication GitHub (exemple)
```bash
# depuis le dossier parent du projet
git init
git add powerbi-insurance-analytics
git commit -m "Power BI Insurance Analytics – FR README + layout guide"
git branch -M main
git remote add origin https://github.com/<user>/powerbi-insurance-analytics.git
git push -u origin main
```

## 📜 Licence
Sous licence **MIT** (voir `LICENSE`). Vous pouvez forker et utiliser librement.
