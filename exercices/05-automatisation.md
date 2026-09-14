# TP5 — Automatiser

## Mission

Une tâche répétitive prend plusieurs minutes à la main. Transformez-la en procédure reproductible.

## 1. Enchaîner des commandes

```bash
wp core version && wp plugin list && wp theme list
```

**Question :** que se passe-t-il si une commande échoue ?

## 2. Construire un état des lieux

Construisez une suite de commandes qui affiche :

- la version de WordPress ;
- le thème actif ;
- les plugins actifs ;
- le nombre d’articles ;
- le nombre d’utilisateurs.

Vous pouvez utiliser `&&`.

## 3. Préparer une maintenance

Imaginez une routine :

1. vérifier WordPress ;
2. vérifier plugins et thèmes ;
3. nettoyer les transients expirés ;
4. produire un compte rendu.

Pour les transients :

```bash
wp transient delete --expired
```

## 4. Penser script

Une suite de commandes peut être placée dans un fichier Bash :

```bash
#!/bin/bash

wp core version
wp plugin list
wp theme list
```

**Question :** quels avantages à conserver la procédure dans un fichier ?

## 5. Décrire une procédure

Choisissez une tâche de maintenance et écrivez :

- déclencheur ;
- informations à vérifier ;
- commandes ;
- actions ;
- contrôles ;
- erreurs possibles ;
- ce qui pourrait être automatisé.

## Partie avancée — WP-Cron vs cron système

### Le piège de vocabulaire

Deux choses différentes portent le mot « cron » :

- **WP-Cron** : le planificateur *interne* de WordPress. Il exécute des *hooks PHP* enregistrés par le cœur ou les extensions (publication différée, vérification des mises à jour…). WP-CLI l'observe et le pilote avec `wp cron ...`.
- **Le cron système** : le vrai planificateur temporel du serveur (crontab / cPanel). Il lance *n'importe quelle commande* à heure fixe — dont tes scripts bash.

`wp cron event schedule` programme un **hook WordPress**, pas un script bash. On ne lui confie donc pas une sauvegarde : ça, c'est le rôle du cron système.

### Étape 1 — Observer ce que WordPress planifie déjà

```bash
wp cron event list
wp cron test
```

On découvre souvent des dizaines d'évènements en attente.

### Étape 2 — Comprendre le défaut de WP-Cron

WP-Cron ne se déclenche **qu'à la visite d'une page**.

- Site sans trafic → les tâches prennent du retard (sauvegardes, e-mails, nettoyages qui ne partent pas).
- Site à fort trafic → WP-Cron se déclenche trop souvent et pèse sur les performances.

Ce n'est pas un vrai minuteur : c'est un « au prochain visiteur ».

### Étape 3 — Corriger : désactiver WP-Cron et le piloter par le système

Désactiver le déclenchement automatique :

```bash
wp config set DISABLE_WP_CRON true --raw
wp config get DISABLE_WP_CRON      # vérifier le résultat dans wp-config.php
```

Puis laisser le cron système déclencher WP-Cron à intervalle fixe et fiable. Exemple de ligne crontab (toutes les 15 minutes) :

```
*/15 * * * * cd /chemin/du/site && wp cron event run --due-now >/dev/null 2>&1
```

## Défi avancé

Imaginez un script de sauvegarde combinant :

- export de la base ;
- archivage des fichiers ;
- transfert distant ;
- planification CRON.

Vous n’avez pas besoin de réaliser le transfert dans Playground.

## À retenir

> L’intérêt de WP-CLI apparaît vraiment quand plusieurs commandes deviennent une **procédure reproductible**.
