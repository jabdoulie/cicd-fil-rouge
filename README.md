# TaskFlow — dépôt fil rouge CI/CD

[![CI](https://github.com/jabdoulie/cicd-fil-rouge/actions/workflows/ci.yml/badge.svg)](https://github.com/jabdoulie/cicd-fil-rouge/actions/workflows/ci.yml)

TaskFlow est une petite API de gestion de tâches écrite en Python avec FastAPI.
C'est le projet fil rouge du module CI/CD (Mastère DevOps M1, Sup de Vinci) :
pendant trois jours, vous allez construire autour d'elle un pipeline complet
qui teste, construit, sécurise et livre l'application.

## Lancer l'API en local

Prérequis : Python 3.10 ou plus récent.

```bash
python3 -m venv .venv
source .venv/bin/activate          # Windows : .venv\Scripts\activate
pip install -r requirements-dev.txt
uvicorn app.main:app --reload
```

L'API répond sur http://localhost:8000 et sa documentation interactive est sur
http://localhost:8000/docs.

## Vérifier le code

```bash
pytest           # tests automatiques
ruff check .     # lint
ruff format .    # mise en forme
```

## Lancer avec Docker

```bash
docker build -t taskflow .
docker run --rm -p 8000:8000 taskflow
```

## Endpoints

| Méthode | Chemin | Rôle |
| --- | --- | --- |
| GET | `/health` | État de l'API et version |
| GET | `/tasks` | Liste des tâches |
| GET | `/tasks/search?q=...` | Recherche dans les titres |
| POST | `/tasks` | Crée une tâche (`{"title": "..."}`) |
| GET | `/tasks/{id}` | Détail d'une tâche |
| PATCH | `/tasks/{id}/done` | Marque une tâche comme faite |
| DELETE | `/tasks/{id}` | Supprime une tâche (en-tête `X-API-Token` requis) |

## Configuration

| Variable | Rôle | Défaut |
| --- | --- | --- |
| `APP_VERSION` | Version affichée par `/health` | `0.1.0` |
| `DB_PATH` | Fichier SQLite | `taskflow.db` |
| `API_TOKEN` | Jeton exigé pour supprimer une tâche | vide (suppression désactivée) |
| `NOTIFY_WEBHOOK_URL` | Webhook appelé à chaque création de tâche | vide (désactivé) |

## Équipe

- Hassif 
- Abdoulie

## Gouvernance du dépôt

Nécissite une approbation
Pas de force push 
Pas de suppresion

## Pipeline CI

**Performances d'installation (pip) :**
- Durée sans cache : ~20 à 30 secondes.
- Durée avec cache : ~2 à 5 secondes.

**Rôle du job `CI OK` :**
Ce job agit comme un point de contrôle final unique. Il ne s'exécute que si tous les jobs précédents (lint et tests sur la matrice de versions) ont réussi. Ainsi, au lieu de configurer GitHub pour exiger la réussite de chaque job de la matrice individuellement, nous définissons uniquement `CI OK` comme check obligatoire, ce qui est beaucoup plus simple et robuste.
