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

## Défi avancé

Imaginez un script de sauvegarde combinant :

- export de la base ;
- archivage des fichiers ;
- transfert distant ;
- planification CRON.

Vous n’avez pas besoin de réaliser le transfert dans Playground.

## À retenir

> L’intérêt de WP-CLI apparaît vraiment quand plusieurs commandes deviennent une **procédure reproductible**.
