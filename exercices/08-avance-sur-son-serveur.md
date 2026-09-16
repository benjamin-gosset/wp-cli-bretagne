# TP8 — WP-CLI avancé, sur votre propre serveur

**Pour qui :** vous connaissez déjà WP-CLI et vous disposez d'un WordPress à vous, avec un accès shell complet (SSH). Ce TP ne se fait pas dans Playground : il suppose un vrai terminal Unix (`|`, `$(...)`, `xargs`, cron).

> ⚠️ **Vous travaillez sur VOTRE site.** Contrairement à l'environnement d'atelier, ici rien n'est jetable. **Règle non négociable : une sauvegarde avant toute opération destructive, et `--dry-run` chaque fois qu'il est disponible.** Si vous n'êtes pas sûr, restez en lecture seule.

## Filet de sécurité (à faire en premier, toujours)

```bash
wp db export "backup-$(date +%Y%m%d-%H%M%S).sql"
```

Vérifiez que le fichier existe et n'est pas vide avant d'aller plus loin. C'est ce fichier qui vous permet de revenir en arrière avec `wp db import` si une manipulation tourne mal.

---

## Partie 1 — WP-CLI comme source de données pour le shell

L'idée : WP-CLI produit des données, le shell les exploite. Le couple `--format` + pipe ouvre tout.

```bash
# Formats exploitables
wp post list --post_type=post --format=csv --fields=ID,post_title,post_status
wp user list --format=json --fields=ID,user_login,roles

# Compter, filtrer, trier avec les outils Unix
wp plugin list --field=name | sort
wp plugin list --format=csv --fields=name,status | grep ',active'

# Extraire une valeur précise avec jq (si disponible)
wp option get siteurl
wp post list --format=json | jq '.[] | .post_title'
```

**À explorer :** comment obtenir la liste des extensions actives, une par ligne, prête à réutiliser ? (`--field` vs `--format=csv`.)

## Partie 2 — Agir en masse, avec filet

Capturer une liste d'IDs et la réinjecter — mais toujours observer avant d'agir.

```bash
# 1. Observer (lecture seule)
wp post list --post_type=revision --format=ids

# 2. Simuler quand c'est possible
wp search-replace 'https://ancien.example' 'https://nouveau.example' --dry-run

# 3. Agir seulement après sauvegarde, par lots pour les gros volumes
wp post list --post_type=revision --format=ids | xargs -r -n 50 wp post delete --force
```

`-r` (ne rien lancer si la liste est vide) et `-n 50` (lots de 50) évitent la commande géante fragile.

**À discuter :** pourquoi `--dry-run` sur `search-replace` est-il indispensable sur un site en production ? Que révèle-t-il exactement ?

## Partie 3 — Piloter plusieurs sites à distance

Depuis un seul poste, agir sur des sites distants sans s'y connecter en SSH manuellement.

```bash
# Cibler un site distant ponctuellement
wp --ssh=user@monserveur.fr/home/user/monsite core version
```

Pour ne pas retaper l'hôte à chaque fois, définir des **alias** dans un fichier `wp-cli.yml` (à la racine du projet) ou `~/.wp-cli/config.yml` :

```yaml
@production:
  ssh: user@monserveur.fr/home/user/monsite
@staging:
  ssh: user@monserveur.fr/home/user/staging
```

Puis :

```bash
wp @production core version
wp @staging plugin list
wp @all core version        # @all : la commande sur tous les alias définis
```

**À discuter :** quels risques à disposer d'un alias `@production` sous la main ? Comment éviter de lancer sur la prod ce qu'on voulait tester sur le staging ?

> Réserve : `--ssh` requiert un client SSH fonctionnel et une clé configurée. Le comportement de `@all` et la syntaxe des alias peuvent varier selon la version de WP-CLI — vérifiez avec `wp cli version` et la doc.

## Partie 4 — Industrialiser : script robuste + cron

Assembler tout ça dans une procédure reproductible, sûre et planifiable.

```bash
#!/usr/bin/env bash
set -euo pipefail

SITE="$HOME/monsite"
DEST="$HOME/sauvegardes"
STAMP="$(date +%Y%m%d-%H%M%S)"
mkdir -p "$DEST"
cd "$SITE"

echo "[1/4] Vérification du site"
wp core is-installed || { echo "WordPress introuvable"; exit 1; }

echo "[2/4] Sauvegarde (le filet avant tout)"
wp db export "$DEST/db-$STAMP.sql"
tar -czf "$DEST/content-$STAMP.tar.gz" wp-content

echo "[3/4] Entretien"
wp transient delete --expired
wp db optimize

echo "[4/4] Rapport"
echo "Version : $(wp core version)"
echo "Extensions actives : $(wp plugin list --status=active --field=name | wc -l)"
echo "Sauvegarde : $DEST/db-$STAMP.sql"
```

Rendre exécutable, tester à la main, puis planifier :

```bash
chmod +x ~/scripts/entretien.sh
~/scripts/entretien.sh
# crontab -e  → tous les jours à 3h :
# 0 3 * * * /bin/bash $HOME/scripts/entretien.sh >> $HOME/scripts/entretien.log 2>&1
```

**À discuter :** que fait `set -euo pipefail`, et pourquoi est-ce vital dans un script qui touche à une base de données ? Comment testeriez-vous ce script dans les conditions de cron (environnement épuré) avant de vous y fier ?

## Défi

Écrire votre propre procédure d'entretien, adaptée à un de vos sites réels. Elle doit :

- refuser de s'exécuter si WordPress n'est pas détecté ;
- sauvegarder avant toute autre action ;
- ne comporter aucune opération destructive sans simulation ou sauvegarde préalable ;
- produire une trace lisible et exploitable.

## Rappels de sécurité

- Sauvegarde **avant**, pas après.
- `--dry-run` par défaut ; le réel seulement après lecture du résultat simulé.
- Sur du destructif en masse, vérifier la liste d'IDs **avant** de la passer à la suppression.
- `wp db query` exécute du SQL brut : un `DELETE`/`UPDATE` sans `WHERE` correct est irréversible. Préférer les commandes WP-CLI dédiées quand elles existent.
