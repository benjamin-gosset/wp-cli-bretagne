# Aide-mémoire WP-CLI

## Aide

```bash
wp help
wp help <commande>
```

## Cœur

```bash
wp core version
wp core update
wp core update-db
```

## Plugins

```bash
wp plugin list
wp plugin install <slug>
wp plugin activate <slug>
wp plugin deactivate <slug>
wp plugin uninstall <slug>
wp plugin update --all
```

## Thèmes

```bash
wp theme list
wp theme install <slug> --activate
wp theme activate <slug>
wp theme deactivate <slug>
wp theme delete <slug>
```

## Utilisateurs

```bash
wp user list
wp user get <id-ou-login>
wp user create <login> <email> --role=<role>
wp user update <id-ou-login> ...
wp role list
```

## Contenus

```bash
wp post list
wp post get <id>
wp post create --post_title="Titre" --post_content="Contenu" --post_status=publish
wp post generate --count=10
```

## Taxonomies

```bash
wp term list category
wp term create category "Nom"
wp term list post_tag
```

## Options

```bash
wp option get <nom>
wp option update <nom> <valeur>
```

## Médias

```bash
wp media regenerate --yes
```

## Transients

```bash
wp transient delete --expired
```

## Base de données — serveur

```bash
wp db size
wp db size --tables --size_format=kb
wp db export sauvegarde.sql
wp db import sauvegarde.sql
wp db optimize
wp db query "..."
wp search-replace 'ancien' 'nouveau'
```

## Chaînage

```bash
commande1 && commande2
```

## Sortie d’une commande

```bash
$(commande)
```

Exemple :

```bash
wp post delete $(wp post list --post_type='product' --format=ids) --force
```

**Attention :** toujours tester avec une commande non destructive.

## Formats

Selon les commandes :

```bash
--format=table
--format=csv
--format=json
--format=ids
```

## Réflexe de sécurité

1. lire `wp help ...` ;
2. identifier précisément la cible ;
3. faire une commande de lecture ;
4. sauvegarder si nécessaire ;
5. exécuter ;
6. vérifier.
