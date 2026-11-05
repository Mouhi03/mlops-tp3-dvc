# Orchestrateur ou integration continue ? (TP06)

| Critere | Prefect (orchestrateur) | GitHub Actions (CI) |
|---|---|---|
| Ce qui declenche l'execution | Cron (lundi 6h) ou Quick run manuel depuis l'interface | `git push` ou `workflow_dispatch` |
| Ou s'execute le traitement | Sur le poste (serveur local + worker `pool-tp`) | Sur une VM GitHub `ubuntu-latest` |
| Qui surveille, et ou | L'equipe via le dashboard http://127.0.0.1:4200 (Runs, Deployments, Work Pools) | GitHub via l'onglet Actions du depot |
| Ce qui se passe en cas d'echec | Run en etat `Failed`, tache `valider` identifiee, notification journalisee ; pas de retry sur `valider` (mais 2 retries sur le pipeline) | Etape rouge, commit marque en rouge sur GitHub, artefact `rapports` conserve |
| Type de tache adapte | Traitements recurrents / planifies (reentrainement, batch) | Verification du code a chaque modification (tests, lint, build) |
| Ce qui est versionne dans Git | `flow_mlops.py`, `planifier.py`, `prefect.yaml` (le code du workflow) | `.github/workflows/ci.yml`, `verifier_qualite.py`, `requirements-ci.txt` |

## Conclusion (5 lignes)

La chaine d'integration continue (GitHub Actions) valide le code a chaque modification
en rejouant le pipeline sur une VM Linux isolee. L'orchestrateur (Prefect) execute et
surveille des traitements recurrents (reentrainement hebdomadaire) avec une interface
d'observabilite et une regle de qualite bloquante. Les deux sont complementaires :
la CI protege la qualite du code, l'orchestrateur garantit la qualite du modele en production.