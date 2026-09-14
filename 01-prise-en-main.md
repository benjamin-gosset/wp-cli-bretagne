# TP1 — Prise en main et exploration

## Mission

Vous récupérez un site WordPress que vous ne connaissez pas.

Avant de modifier quoi que ce soit, dressez rapidement son état des lieux avec WP-CLI.

## Objectifs

- vérifier que WP-CLI fonctionne ;
- identifier la version de WordPress ;
- explorer thèmes et extensions ;
- explorer utilisateurs et contenus ;
- trouver de l’aide ;
- utiliser différents formats de sortie.

## 1. Vérifier l’environnement

```bash
wp core version
wp option get blogname
wp option get blogdescription
```

**Question :** quel est le nom du site ? Quelle est sa description ?

## 2. Inventorier les extensions

```bash
wp plugin list
```

Puis trouvez comment n’afficher que les extensions actives.

**Indice :**

```bash
wp help plugin list
```

**Défi :** affichez un tableau avec uniquement les colonnes utiles.

## 3. Inventorier les thèmes

```bash
wp theme list
```

Trouvez le thème actif.

**Question :** pourquoi distinguer thèmes installés et thème actif ?

## 4. Explorer les utilisateurs

```bash
wp user list
wp role list
wp user get <identifiant>
```

## 5. Explorer les contenus

```bash
wp post list --post_type=post
wp post list --post_type=page
wp term list category
wp term list post_tag
```

## 6. Utiliser l’aide intégrée

Trouvez comment afficher les articles avec leur ID et leur titre uniquement :

```bash
wp help post list
```

**Défi :** trouvez comment afficher le résultat en CSV.

## Validation

Vous savez répondre à ces questions sans regarder la fiche :

- quelle commande donne la version de WordPress ?
- quelle commande liste les extensions ?
- comment savoir quel thème est actif ?
- comment obtenir les utilisateurs ?
- où chercher la syntaxe d’une commande inconnue ?
