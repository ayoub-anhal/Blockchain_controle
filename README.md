# TP Blockchain – Guide du Notebook

Ce dépôt contient un notebook Jupyter (`TP_controle.ipynb`) dédié à la conception, à l'implémentation et à l'analyse d'une blockchain pédagogique. Le notebook mêle théorie, code Python commenté et démonstrations contrôlées afin d'offrir une progression complète : des fondations conceptuelles jusqu'aux scénarios de décentralisation simulée.

---

## Table des matières

1. [Organisation](#organisation)
2. [Structure du code et bonnes pratiques](#structure-du-code-et-bonnes-pratiques)
3. [Démonstrations contrôlées](#démonstrations-contrôlées)
4. [API REST simulée](#api-rest-simulée)
5. [Fonctions d'analytics](#fonctions-danalytics)
6. [Simulation de décentralisation](#simulation-de-décentralisation)
7. [Installation et exécution](#installation-et-exécution)
8. [Références essentielles](#références-essentielles)
9. [Licence et contribution](#licence-et-contribution)

---

## Organisation

### Objectifs pédagogiques

- Comprendre l'anatomie d'un bloc et les mécanismes fondamentaux d'une blockchain (chaînage des hachages, preuve de travail, validation).
- Construire pas à pas une implémentation Python fortement typée et réutilisable de la blockchain étudiée.
- Explorer une API REST simulée pour interagir avec la chaîne (lecture, transactions, minage, analyse de balances).
- Étudier la décentralisation par la simulation de plusieurs nœuds, de la diffusion naïve et de la résolution de conflits.

### Hypothèses et cadre d'étude

- Les participants maîtrisent les bases de Python (classes, dataclasses, annotations de type) et de la programmation orientée objet.
- Le réseau est simulé localement : aucune communication réelle sur le réseau n'est réalisée, ce qui facilite la reproductibilité et l'analyse.
- Les transactions sont simplifiées (pas de signatures cryptographiques) afin de se concentrer sur le mécanisme de consensus et le flux des blocs.

### Limites du prototype

- Le temps de minage est volontairement borné par une difficulté faible (2 à 3 zéros initiaux) pour garantir des exécutions rapides et éviter toute attente excessive.
- Les problématiques de sécurité avancées (attaques Sybil, 51 %, coûts économiques) sont évoquées mais non implémentées.
- L'API REST simulée n'est pas exposée via un serveur réel ; elle sert d'outil pédagogique pour illustrer les appels et les réponses JSON canoniques.

### Progression du notebook

Le notebook suit une structure pédagogique progressive organisée en quatre étapes par section majeure :

1. **Théorie** – Introduction conceptuelle à la blockchain, principes fondamentaux, vocabulaire, diagrammes ASCII illustratifs.
2. **Implémentation** – Construction progressive de la classe `Block`, de la chaîne (`Blockchain`), puis ajout des transactions et du minage via des fonctions utilitaires.
3. **Démonstration** – Exécution de scénarios commentés avec métriques de minage, bilans des comptes, journaux d'événements.
4. **Analyse** – Interprétation des résultats, mise en évidence des limites, propositions d'améliorations.

Cette structure est répétée à chaque section majeure pour ancrer les connaissances.

---

## Structure du code et bonnes pratiques

Le notebook est conçu comme un support où chaque cellule de code est accompagnée de commentaires et de docstrings. Les choix techniques principaux sont :

### Architecture modulaire

- **Dataclasses** pour modéliser les entités (`Block`, `Transaction`, `Node`) avec sérialisation JSON canonique (`json.dumps(..., sort_keys=True)`).
- **Annotations de type** systématiques et fonctions utilitaires dédiées (`hash_block`, `load_chain_from_json`, `compute_balance`).
- **Organisation modulaire** par sections : bloc de configuration initiale, classes principales, utilitaires d'analytics, API pédagogique.

### Documentation et maintenabilité

- **Docstrings normalisées** (format reStructuredText) pour chaque fonction exposée afin de faciliter l'autocomplétion et la génération de documentation.
- **Gestion des timestamps** avec le format RFC 3339 pour garantir l'interopérabilité et la clarté des journaux d'événements.
- **Commentaires explicatifs** intégrés dans chaque section pour guider la lecture et la compréhension du code.

---

## Démonstrations contrôlées

Toutes les démonstrations reposent sur un minage à difficulté 2 ou 3 pour garantir des temps d'exécution inférieurs à quelques secondes. Chaque scénario consigne :

- Le nombre moyen d'itérations nécessaires pour trouver un nonce valide.
- Le hash obtenu et son respect de la difficulté imposée.
- Les transactions minées et la récompense associée.
- Une analyse qualitative des résultats (impact sur les balances, sécurité du réseau).

### Scénarios démontrés

- Création et validation d'un bloc genesis
- Ajout de transactions et minage de blocs consécutifs
- Vérification de l'intégrité de la chaîne
- Calcul des balances et détection d'anomalies
- Analyse des performances de minage

---

## API REST simulée

Une API pédagogique est définie pour illustrer la manière dont une blockchain s'ouvre sur l'extérieur. Les endpoints sont décrits dans le notebook avec exemples d'appels et de réponses JSON.

### Endpoints disponibles

| Méthode | Endpoint           | Description                                                    |
|---------|--------------------|----------------------------------------------------------------|
| `GET`   | `/get_chain`       | Récupère l'état complet de la blockchain (liste de blocs).     |
| `POST`  | `/add_transaction` | Ajoute une transaction validée dans la file d'attente.         |
| `GET`   | `/mine_block`      | Lance le minage d'un nouveau bloc avec les transactions en attente. |
| `GET`   | `/get_balance`     | Calcule la balance d'une adresse à partir de la chaîne valide. |

### Format des réponses

Chaque réponse suit un format JSON canonique, avec champs horodatés en RFC 3339 et codes d'état simulés pour faciliter le débogage.

---

## Fonctions d'analytics

Le notebook intègre un module d'analytics pour mesurer la vitalité de la chaîne.

### Indicateurs disponibles

- Nombre total de transactions et volume agrégé.
- Liste des adresses actives et leur fréquence d'apparition.
- Itérations moyennes de minage par bloc et temps moyen associé.
- Rapport synthétique généré en Markdown, prêt à être exporté.

Ces indicateurs permettent d'entamer une réflexion sur les performances et la résilience du protocole implémenté.

---

## Simulation de décentralisation

La deuxième partie du notebook met en scène plusieurs nœuds simulés, chacun conservant sa propre copie de la blockchain et communiquant via des méthodes Python.

### Fonctionnalités implémentées

- **Gestion des pairs** : enregistrement, synchronisation initiale, diffusion naïve des nouveaux blocs.
- **Résolution de conflits** : adoption de la chaîne la plus longue et vote simplifié lorsque deux chaînes valides de même longueur coexistent.
- **Scénarios avancés** : partition réseau, bloc malveillant rejeté, tentative de double dépense et discussion sur les protections nécessaires.
- **Tableaux d'analyse** : état de santé du réseau, taux de propagation, fréquence des forks, taux de consensus.

Ces démonstrations soulignent les défis de la décentralisation et les compromis inhérents aux protocoles de consensus.

---

## Installation et exécution

### Prérequis

- Python 3.8 ou supérieur
- Jupyter Lab ou Jupyter Notebook
- Bibliothèques standard Python (hashlib, json, datetime, typing)

### Installation

```bash
# Cloner le dépôt
git clone [url-du-depot]
cd tp-blockchain

# Installer Jupyter si nécessaire
pip install jupyterlab

# Lancer Jupyter Lab
jupyter lab
```

### Exécution du notebook

1. Ouvrir le fichier `TP_controle.ipynb` dans Jupyter Lab ou VS Code.
2. Exécuter les cellules séquentiellement pour suivre la progression pédagogique.
3. Chaque section contient des instructions spécifiques pour reproduire les expériences et interpréter les résultats.

### Environnements compatibles

- Jupyter Lab
- Jupyter Notebook
- VS Code avec extension Jupyter
- Google Colab (avec adaptations mineures)

---

## Références essentielles

### Documentation fondamentale

- Satoshi Nakamoto. *Bitcoin: A Peer-to-Peer Electronic Cash System*. [bitcoin.org](https://bitcoin.org/bitcoin.pdf)
- Andreas M. Antonopoulos & Olaoluwa Osuntokun Harding. *Mastering Bitcoin*. [GitHub](https://github.com/bitcoinbook/bitcoinbook)

### Standards et spécifications

- NIST. *FIPS 180-4 Secure Hash Standard (SHA-256)*. [NIST CSRC](https://csrc.nist.gov/publications/detail/fips/180/4/final)
- *RFC 3339: Date and Time on the Internet*. [rfc-editor.org](https://www.rfc-editor.org/rfc/rfc3339)

### Recherche académique

- Juan A. Garay, Aggelos Kiayias, Nikos Leonardos. *The Bitcoin Backbone Protocol: Analysis and Applications*. [SpringerLink](https://link.springer.com/chapter/10.1007/978-3-662-46803-6_10)

### Frameworks web

- Documentation FastAPI. [fastapi.tiangolo.com](https://fastapi.tiangolo.com/)
- Documentation Flask. [flask.palletsprojects.com](https://flask.palletsprojects.com/)

### Ressources complémentaires

- Princeton University. *Bitcoin and Cryptocurrency Technologies*. [collaborate.princeton.edu](https://collaborate.princeton.edu/en/public-offering/bitcoin-and-cryptocurrency-technologies)

---

## Licence et contribution

### Licence

Ce projet est fourni à des fins pédagogiques. Veuillez consulter le fichier LICENSE pour plus d'informations sur les conditions d'utilisation.

### Contribution

Les contributions sont les bienvenues. Pour contribuer :

1. Forker le projet
2. Créer une branche pour votre fonctionnalité (`git checkout -b feature/amelioration`)
3. Committer vos changements (`git commit -m 'Ajout d'une amélioration'`)
4. Pousser vers la branche (`git push origin feature/amelioration`)
5. Ouvrir une Pull Request

### Contact et support

Pour toute question ou suggestion, veuillez ouvrir une issue sur le dépôt GitHub.

---

**Note importante** : Ce projet est destiné à un usage pédagogique uniquement. Il ne doit pas être utilisé en production sans une révision approfondie de la sécurité et l'ajout de fonctionnalités cryptographiques robustes.