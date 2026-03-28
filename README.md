# RihlaBot 🌍🇲🇦
### Planificateur de Voyage IA au Maroc

> *"Planifier un voyage au Maroc prend des heures. RihlaBot le fait en 30 secondes."*

**Projet IA — Licence MSDI — FST Mohammedia — **  
**Réalisé par : Camelea ARIRI **

---

## 🎯 Problème résolu

Planifier un voyage au Maroc est long et complexe : recherche des activités, estimation des budgets, consultation de la météo, choix des restaurants... RihlaBot automatise tout ce processus en une conversation de 30 secondes et génère un itinéraire complet jour par jour, avec météo et prix en Dirhams.

---

## 🏗️ Architecture

```
Utilisateur
    │
    ▼
Botpress (Interface conversation)
    │ webhook POST
    ▼
n8n (Pipeline automatisé)
    ├── Groq AI (Llama 3.3) → Parsing des préférences
    ├── OpenWeatherMap → Météo en temps réel
    ├── Groq AI (Llama 3.3) → Génération itinéraire
    └── Google Sheets → Sauvegarde des demandes
```

---

## 🛠️ Technologies utilisées

| Outil | Rôle | Gratuit |
|-------|------|---------|
| **Botpress Cloud** | Interface chatbot conversationnelle | ✅ |
| **n8n Cloud** | Automatisation du pipeline | ✅ |
| **Groq AI** (Llama 3.3-70b) | Génération de l'itinéraire | ✅ |
| **OpenWeatherMap** | Météo en temps réel | ✅ |
| **Google Sheets** | Sauvegarde des demandes | ✅ |

---

## 💬 Guide d'utilisation

1. Ouvrir le chatbot : [RihlaBot Webchat](https://cdn.botpress.cloud/webchat/v3.6/shareable.html?configUrl=https://files.bpcontent.cloud/2026/03/25/18/20260325185413-H52A4A2U.json)
2. Choisir le type de voyage : **Destination unique** ou **Circuit multi-destinations**
3. Répondre aux questions : ville, date, budget, style, nombre de voyageurs
4. Recevoir l'itinéraire complet avec météo et prix en DH

---

## 🔄 Workflow n8n — 9 nœuds

| # | Nœud | Rôle |
|---|------|------|
| 1 | **Webhook** | Reçoit les données de Botpress |
| 2 | **Set Input** | Normalise les champs reçus |
| 3 | **OpenAI Parsing** (Groq) | Extrait et valide les préférences |
| 4 | **Code Parse JSON** | Parse robuste avec gestion d'erreur |
| 5 | **Weather API** | Récupère la météo OpenWeatherMap |
| 6 | **Set Meteo** | Formate la météo en phrase lisible |
| 7 | **OpenAI Itinerary** (Groq) | Génère l'itinéraire jour par jour |
| 8 | **Google Sheets** | Sauvegarde la demande |
| 9 | **Send to Botpress** | Renvoie la réponse |

---

## 🤖 Prompts utilisés

### Prompt 1 — Extraction JSON (température 0)
```
SYSTEM: Tu es un assistant voyage Maroc. Extrait et retourne UNIQUEMENT ce JSON :
{"destination":"","dates":"","budget_dh":"","style":"","voyageurs":"","nb_nuits":"","trip_type":"","langue":"fr"}

USER: Données reçues : destination={destination}, dates={dates}, budget={budget_dh}...
```

### Prompt 2 — Génération itinéraire (température 0.7)
```
SYSTEM: Tu es RihlaBot, expert en tourisme marocain depuis 15 ans.
Tu connais : prix en DH 2025, riads, CTM/Supratours, souks, tajine, harira.

USER: Génère un itinéraire pour {destination}, départ {dates}, {nb_nuits} de séjour,
budget {budget_dh}, {voyageurs}, style {style}.
Format obligatoire par jour :
Jour X — Titre
🌅 Matin : activité (~XX DH)
☀️ Après-midi : activité (~XX DH)
🌙 Soir : restaurant (~XX DH/pers)
💡 Conseil : astuce culturelle locale
Commence par "Marhaba ! 🌟"
```

---

## ⚠️ Difficultés rencontrées

| Problème | Solution |
|----------|----------|
| Clé xAI invalide (modèle grok-2 déprécié) | Migration vers **Groq** (gratuit, llama-3.3-70b) |
| Google Sheets interprétait les valeurs comme formules | Option **"Let n8n format"** |
| Botpress envoyait le type de voyage au lieu de la ville | Correction du champ `destination: workflow.ville1` |
| URL webhook-test vs production | Utilisation de l'URL `/webhook/rihlabot` (production) |
| Variables Botpress non accessibles dans Main flow | Ajout de valeurs par défaut avec `||` dans le code |

---

## 🧠 Biais identifiés (Module 8 — Éthique IA)

| Biais | Description | Correction |
|-------|-------------|------------|
| **Géographique** | Villes connues (Marrakech, Fès) mieux couvertes qu'Ifrane ou Merzouga | Liste de 20 villes validées dans Botpress |
| **Budgétaire** | Estimations de prix basées sur 2025, peuvent être inexactes | Disclaimer ajouté dans le message final |
| **Linguistique** | Le bot ne comprend pas le darija | Langue forcée en français, détection partielle |

---

## 📁 Fichiers du projet

- `RihlaBot_n8n_workflow.json` — Workflow n8n complet (9 nœuds)
- `RihlaBot_2026_Mar_27.bpz` — Export bot Botpress
- `README.md` — Ce fichier

---

## 🚀 Installation

### n8n
1. Importer `RihlaBot_n8n_workflow.json` dans n8n
2. Remplacer les clés API : Groq, OpenWeatherMap, Google Sheets
3. Activer le workflow (Published)

### Botpress
1. Importer `RihlaBot_2026_Mar_27.bpz`
2. Mettre à jour l'URL webhook dans Execute Code
3. Publier le bot

---

*RihlaBot — FST Mohammedia — Botpress + n8n + Groq AI 🇲🇦*
