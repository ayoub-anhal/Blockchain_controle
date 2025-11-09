# Guide de démarrage — Blockchain_controle

Ce document détaille toutes les étapes nécessaires pour préparer l’environnement, installer les dépendances et exécuter le projet (notebook ainsi qu’API REST FastAPI).

---

## 1. Pré-requis système

- Système Linux (les commandes ci-dessous sont données pour Ubuntu/Debian).
- Accès à Internet pour installer les dépendances.
- Droits `sudo` pour installer des paquets système si nécessaire.

---

## 2. Vérifier ou installer Python

Le projet cible **Python 3.10+** (testé en 3.12). Sur Ubuntu, la commande `python` n’est pas forcément disponible par défaut ; utilisez `python3`.

1. Vérifier la version :
   ```bash
   python3 --version
   ```
   Vous devez obtenir une sortie du type `Python 3.x.y`.  
   > Si vous voyez un message d’erreur indiquant que `python3` est introuvable, installez-le.

2. Installer Python 3 et les outils associés :
   ```bash
   sudo apt update
   sudo apt install -y python3 python3-pip python3-venv python-is-python3
   ```
   - `python-is-python3` crée un alias `python` → `python3` (optionnel mais pratique).
   - `python3-pip` fournit le gestionnaire de paquets `pip`.

---

## 3. Cloner ou se placer dans le projet

```bash
cd /home/ayoubannhal/Documents/GitHub/Blockchain_controle
```

---

## 4. (Optionnel) Créer un environnement virtuel

Recommandé pour isoler les dépendances.

```bash
python3 -m venv .venv
source .venv/bin/activate   # (à lancer dans chaque nouveau terminal)
python --version            # Doit refléter l’environnement (.venv)
```

Pour quitter l’environnement : `deactivate`.

---

## 5. Installer les dépendances Python

`requirements.txt` liste les bibliothèques nécessaires :

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

Packages installés :
- `fastapi` : framework API REST.
- `uvicorn[standard]` : serveur ASGI pour exécuter FastAPI.
- `pydantic` : validation de données (schemas API).

### Dépendances additionnelles utiles
- Pour exécuter le notebook localement : `pip install notebook` ou `pip install jupyterlab`.
- Pour tester l’API depuis le notebook : `pip install httpx` (inclus implicitement par `uvicorn[standard]`, mais vous pouvez l’installer explicitement si nécessaire).

---

## 6. Ouvrir et exécuter le notebook

1. Lancer Jupyter Notebook ou Lab :
   ```bash
   jupyter notebook    # ou jupyter lab
   ```
2. Depuis l’interface web, ouvrir `TP_controle.ipynb`.
3. Exécuter les cellules dans l’ordre (Kernel > Restart & Run All).
   - Les sections 1.1 à 1.5 couvrent la construction et la validation de la blockchain.
   - La section 1.6 expose l’API FastAPI directement dans le notebook via `TestClient`.
   - Les sections suivantes simulent la décentralisation.

> Si une cellule FastAPI échoue en raison de dépendances manquantes, assurez-vous que l’environnement virtuel est activé et que `pip install -r requirements.txt` a bien été exécuté.

---

## 7. Lancer l’API REST FastAPI avec Uvicorn

Le notebook expose un objet `app` (FastAPI). Pour le lancer hors notebook :

1. Créer un script (ex. `main_api.py`) avec :
   ```python
   from TP_controle import app  # adapter si vous déplacez le code dans un module
   ```
   (Dans le notebook, vous pouvez aussi copier la cellule FastAPI dans un fichier Python.)

2. Puis exécuter :
   ```bash
   uvicorn main_api:app --reload --host 0.0.0.0 --port 8000
   ```

3. Tester les endpoints :
   ```bash
   curl http://127.0.0.1:8000/health
   curl http://127.0.0.1:8000/chain
   curl -X POST http://127.0.0.1:8000/transactions -H "Content-Type: application/json" \
        -d '{"sender": "alice", "receiver": "bob", "amount": 5}'
   curl -X POST http://127.0.0.1:8000/mine -H "Content-Type: application/json" \
        -d '{"miner_address": "miner_cli"}'
   curl http://127.0.0.1:8000/balance/alice
   ```

4. Documentation interactive : `http://127.0.0.1:8000/docs`.

---

## 8. Dépannage courant

- **`ModuleNotFoundError: No module named 'fastapi'`**  
  → Vérifiez que l’environnement virtuel est actif et relancez `pip install -r requirements.txt`.

- **`command not found: uvicorn`**  
  → Uvicorn n’est pas installé ou l’environnement n’est pas activé.

- **Ports occupés (`Address already in use`)**  
  → Arrêtez les instances précédentes (`Ctrl+C`) ou changez de port : `uvicorn main_api:app --port 8001`.

- **Kernel Jupyter n’utilise pas le bon Python**  
  → Dans Jupyter, Menu Kernel > Change Kernel > sélectionnez l’environnement `.venv`.

---

## 9. Étapes récapitulatives rapides

```bash
sudo apt update
sudo apt install -y python3 python3-pip python3-venv python-is-python3
cd /home/ayoubannhal/Documents/GitHub/Blockchain_controle
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
jupyter notebook  # pour exécuter le notebook
# ou
uvicorn main_api:app --reload  # après avoir exporté l'objet app dans un module Python
```

Vous disposez maintenant d’un environnement fonctionnel pour explorer la blockchain pédagogique et l’API associée.

