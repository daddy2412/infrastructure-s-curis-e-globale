# Guide — Publier votre lab sur GitHub

## Fichiers préparés
```
portfolio-lab/
├── README.md                    ← page d'accueil du dépôt GitHub
├── index.html                   ← portfolio web (GitHub Pages)
├── docs/
│   └── technologies-labo.md     ← inventaire technique complet
└── assets/
    ├── architecture.png         ← schéma d'infrastructure
    └── grafana-dashboard.png    ← capture du dashboard
```

## Étape 1 — Créer le dépôt sur GitHub
1. Allez sur https://github.com/new
2. Nom du dépôt : `homelab-soc` (ou autre nom clair)
3. Visibilité : **Public** (pour que les recruteurs le voient)
4. Ne cochez PAS "Add a README" (vous en avez déjà un)
5. Cliquez **Create repository**

## Étape 2 — Envoyer les fichiers (deux options)

### Option A — Via l'interface web (le plus simple, sans ligne de commande)
1. Sur la page du nouveau dépôt, cliquez **uploading an existing file**
2. Glissez-déposez tous les fichiers et dossiers du dossier `portfolio-lab/`
3. Écrivez un message de commit, ex. : `Documentation initiale du lab SOC`
4. Cliquez **Commit changes**

### Option B — Via Git en ligne de commande
```bash
cd portfolio-lab
git init
git add .
git commit -m "Documentation initiale du lab SOC"
git branch -M main
git remote add origin https://github.com/VOTRE-USAGER/homelab-soc.git
git push -u origin main
```

## Étape 3 — Activer GitHub Pages (portfolio en ligne)
1. Dans le dépôt, allez dans **Settings** → **Pages**
2. Sous "Build and deployment", **Source** : sélectionnez `Deploy from a branch`
3. **Branch** : `main`, dossier `/ (root)`
4. Cliquez **Save**
5. Après 1–2 minutes, votre portfolio sera en ligne à :
   `https://VOTRE-USAGER.github.io/homelab-soc/`

## Étape 4 — Personnaliser avant publication
Dans `README.md` et `index.html`, remplacez :
- `votre-usager` / `VOTRE-USAGER` → votre nom d'utilisateur GitHub réel
- `(votre email / LinkedIn à ajouter)` → vos vraies coordonnées
- Le lien de la vidéo de démonstration, si vous en avez une

## Étape 5 — Ajouter à votre CV et vos candidatures
Une fois en ligne, ajoutez le lien dans :
- Votre CV (section "Projets" ou "Portfolio")
- Votre profil LinkedIn (section "Featured" / "En vedette")
- Vos lettres de motivation pour les postes en cybersécurité/infrastructure

## Prochaines améliorations possibles (optionnel)
- Ajouter une vidéo de démonstration (celle enregistrée avec OBS Studio) sur YouTube en non répertorié, puis lier le lien
- Ajouter des captures d'écran supplémentaires (OPNsense, Wazuh Dashboard, Security Onion)
- Rédiger un ou deux articles "post-mortem" décrivant un incident résolu (ex. reset de mot de passe root via GRUB) — ça montre votre capacité de dépannage réel
