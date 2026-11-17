# Data Collection Sénégal — projet de collecte de données

Projet de devoir centré sur cinq acteurs Apify et une cible de 50 observations nettoyées par acteur, avec notebook, tableau de bord, rapport, slides et preuves de soutenance.

## Structure

- `index.html` : tableau de bord principal des cinq acteurs.
- `scripts/scrapers_apify.py` : collecteur local avec les cinq configurations, nettoyage et exports.
- `notebooks/collecte_5_acteurs_senegal.ipynb` : notebook à soumettre.
- `data/apify/` : généré après exécution; CSV par acteur, exports combinés et bilan.
- `docs/rapport_projet.html` : rapport détaillé des cinq acteurs, méthode et limites.
- `docs/slides_presentation.html` : slides présentables au clavier.
- `evidence/` : liste de dix captures à fournir, manifeste de galerie et fichier où ajouter l’URL vidéo.
- `archive_open_data.html`, `data/archive_open_data/` et `assets/images/` : ancienne collecte, isolée du jeu Apify.
- `render.yaml` : configuration Blueprint pour créer le site statique Render.

> Ne pas annoncer 250 observations acquises avant d’avoir exécuté le notebook et vérifié 50 lignes nettoyées par acteur. Le tableau de bord indique explicitement si la collecte n’a pas encore été faite.

## Lancer localement

Le tableau de bord charge les fichiers par HTTP. Depuis la racine du projet :

```powershell
python -m http.server 8765
```

Ouvrir ensuite <http://localhost:8765/>. Ne pas ouvrir `index.html` directement avec `file://`.

Pour le notebook, installer Jupyter et pandas dans l’environnement Python choisi (ou installer `pandas` puis ouvrir le notebook dans VS Code). Garder le notebook dans `notebooks/` afin qu’il retrouve le dossier `data/`.

Installer les dépendances de collecte depuis la racine :

```powershell
python -m pip install -r requirements.txt
```

Le notebook demande `APIFY_TOKEN` en saisie masquée si la variable n’est pas déjà définie. Les appels Apify peuvent consommer le quota/crédit du compte. Aucun appel n’est lancé par le site web. Les résultats sont écrits sous `data/apify/`; après nettoyage, l’objectif de remise est 50 observations par acteur.

## Déployer le site statique sur Render

Render déploie ce site statique à partir d’un dépôt Git. Le dossier de travail actuel n’est pas encore relié à un dépôt; il faut donc :

1. Créer un dépôt GitHub public et y pousser le projet (la configuration se trouve dans `render.yaml`).
2. Dans Render, choisir **New → Blueprint**, connecter ce dépôt et appliquer le Blueprint. Il crée un site statique gratuit nommé `data-collection-senegal`.
3. Attendre la fin du premier déploiement, puis utiliser l’URL `https://…onrender.com` fournie par Render.
4. Pour forcer le téléchargement direct du notebook, ajouter dans les en-têtes HTTP du site Render la règle `/notebooks/*.ipynb` avec `Content-Disposition: attachment`.

Le site ne requiert ni base de données ni clé API. La collecte Apify s’exécute localement; après exécution et vérification, ajouter `data/apify/` au dépôt pour publier les résultats. Pour que les captures apparaissent en ligne, déposer les vraies images PNG dans `evidence/captures/`, compléter `evidence/captures/manifest.json`, puis pousser les changements afin que Render redéploie.

Un dépôt public rend ces données et captures accessibles à tous : vérifier la vie privée et les conditions des plateformes avant publication.

## Licence et attribution

Le code du projet est sous licence MIT (voir `LICENSE`). Cette licence ne couvre pas les contenus de tiers. Les données restent soumises aux conditions de leurs fournisseurs; les crédits et licences des images archivées sont consignés dans `data/archive_open_data/manifest_images.csv` et doivent être vérifiés individuellement.

Ne jamais publier de jeton Apify. Les dix captures de pages doivent être réelles et ajoutées manuellement selon `evidence/README.md`. Chaque entrée du manifeste utilise les champs `actor`, `file`, `title` et `source_url`; `file` doit être le nom du PNG placé dans `evidence/captures/`. Après avoir enregistré et partagé la vidéo (12 minutes maximum), ajoutez son URL à `evidence/url_video.txt`.
