# TP2 — Maintenance et administration

## Mission

Vous êtes chargé de la maintenance courante d’un site WordPress, sans utiliser le back-office.

## 1. Vérifier le cœur

```bash
wp core version
wp help core update
```

**Question :** quelle différence entre `wp core update` et `wp core update-db` ?

## 2. Installer et gérer une extension

Installez puis activez Classic Editor :

```bash
wp plugin install classic-editor
wp plugin activate classic-editor
```

Vérifiez :

```bash
wp plugin list
```

Puis désactivez et désinstallez :

```bash
wp plugin deactivate classic-editor
wp plugin uninstall classic-editor
```

**Réflexion :** pourquoi séparer installation, activation et désinstallation ?

## 3. Changer de thème

```bash
wp theme list
wp theme activate <nom-du-theme>
wp theme list
```

**Bonus :** trouvez comment supprimer un thème inutilisé.

## 4. Modifier une option

```bash
wp option get blogname
wp option update blogname "La Maison des Contributeurs — Atelier WP-CLI"
wp option get blogname
```

Remettez le titre initial si nécessaire.

## 5. Créer un article

```bash
wp post create   --post_title="Mon premier article depuis le terminal"   --post_content="Cet article a été créé avec WP-CLI."   --post_status=publish
```

Puis :

```bash
wp post list --post_type=post --orderby=date --order=desc
```

Trouvez son ID :

```bash
wp post get <ID>
```

## 6. Nettoyer

Supprimez votre article de test.

Avant cela :

```bash
wp help post delete
```

Puis vérifiez qu’il a disparu.

## Défi avancé

Installez et activez plusieurs extensions en une seule commande. Utilisez :

```bash
wp help plugin install
```

## À retenir

> **Observer → modifier → vérifier → nettoyer**
