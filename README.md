# DNS en haute disponibilité avec Bind9 et Ansible

Projet académique : installation et configuration d'un service DNS en haute disponibilité avec deux serveurs **Bind9 (Master / Slave)**, déployé de façon automatisée avec **Ansible**.

📄 Documentation : [rapport](docs/rapport-dns-ha.pdf) · [présentation](docs/presentation-dns-ha.pptx)

## Objectifs
- Mettre en place un service DNS Master/Slave avec transfert de zone
- Automatiser l'installation et la configuration avec Ansible
- Tester la résolution DNS via les deux serveurs
- Vérifier la continuité du service en cas de panne du Master

## Architecture
- **Serveur Master** : gère la zone et ses mises à jour
- **Serveur Slave** : reçoit les transferts de zone et répond aux requêtes si le Master est indisponible
- **Machine de contrôle Ansible** : déploie la configuration sur les deux serveurs par SSH

## Structure du dépôt
| Dossier / fichier | Contenu |
|---|---|
| `ansible/inventory` | Inventaire des serveurs (Master et Slave) |
| `ansible/playbook.yml` | Playbook de déploiement |
| `ansible/templates/` | Templates Jinja2 : options Bind9, configuration Master et Slave, zones directe et inverse |
| `docs/` | Rapport et présentation |
| `screenshots/` | Captures des tests |

## Automatisation avec Ansible
Le déploiement installe Bind9, configure le transfert de zone, déploie les fichiers de zone (directe et inverse) à partir des templates, puis redémarre le service.

```bash
ansible all -i inventory -m ping
ansible-playbook -i inventory playbook.yml
```

## Test de résilience
1. Vérifier la résolution via le Master puis via le Slave (`dig @<IP_du_serveur> <nom_de_domaine>`)
2. Arrêter le serveur Master
3. Vérifier que le Slave continue à répondre aux requêtes DNS

## Technologies
Bind9, Ansible, Jinja2, DNS (zones directe et inverse), SSH, Linux

---
Projet réalisé par **Mariem Ounissi**
