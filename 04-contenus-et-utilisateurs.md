# TP4 — Utilisateurs, contenus et recherche

## Mission

Réalisez quelques opérations d’administration éditoriale depuis le terminal.

## 1. Identifier les administrateurs

```bash
wp user list
```

Trouvez les comptes administrateurs.

**Défi :** utilisez les options de `wp user list` pour n’afficher que les informations utiles.

```bash
wp help user list
```

## 2. Créer un utilisateur de test

```bash
wp user create test.contributeur test.contributeur@example.test --role=contributor
wp user list
wp user get test.contributeur
```

## 3. Modifier l’utilisateur

```bash
wp user update test.contributeur --user_email=test2.contributeur@example.test
```

**Bonus :** trouvez comment changer son rôle.

## 4. Travailler avec les articles

```bash
wp post list --post_type=post
wp post list --post_type=post --format=ids
```

**Question :** pourquoi le format `ids` est-il utile dans un script ?

## 5. Générer du contenu

```bash
wp post generate --count=5 --post_status=publish
```

Vérifiez ensuite le nombre d’articles.

## 6. Travailler avec les catégories

```bash
wp term list category
wp term create category "Atelier"
```

**Défi :** trouvez comment supprimer cette catégorie.

## 7. Utiliser une sortie comme argument

Observez :

```bash
wp post list --post_type=post --format=ids
```

Puis réfléchissez au mécanisme Bash :

```bash
$(commande)
```

**Attention :** ne lancez aucune suppression massive. L’objectif est de comprendre le mécanisme.

## Défi avancé

Construisez une commande qui récupère les IDs selon un critère puis les transmet à une seconde commande. Testez d’abord avec une commande non destructive.
