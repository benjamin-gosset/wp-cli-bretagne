# TP6 — Base de données et migration

**Démonstration animateur**

> Cette partie est réalisée sur un vrai serveur de démonstration. Les manipulations de base de données ne sont pas demandées aux participants dans WordPress Playground.

## Pourquoi sortir de Playground ?

L’environnement Playground sert aux exercices communs. Les commandes `wp db` sont montrées sur un serveur réel préparé par l’animateur.

## 1. Mesurer la base

```bash
wp db size
wp db size --tables --size_format=kb
```

**Question :** quelles tables occupent le plus d’espace ?

## 2. Exporter

```bash
wp db export sauvegarde.sql
```

Observez le fichier produit et sa taille.

## 3. Optimiser

```bash
wp db optimize
```

## 4. Exécuter une requête

```bash
wp db query "SHOW TABLES;"
```

L’animateur montre ensuite une requête de lecture adaptée à l’environnement.

> Ne jamais tester une requête destructive sans sauvegarde et sans avoir vérifié sa portée.

## 5. Rechercher / remplacer

```bash
wp search-replace 'ancienne-valeur' 'nouvelle-valeur'
```

**Questions :**

- pourquoi une recherche/remplacement SQL naïve peut-elle poser problème dans WordPress ?
- pourquoi faut-il être prudent avec les données sérialisées ?
- comment vérifier avant de modifier réellement ?

Commencez par :

```bash
wp help search-replace
```

## 6. Changement d’URL

```bash
wp option update home 'https://exemple.fr'
wp option update siteurl 'https://exemple.fr'
```

Une migration complète implique toutefois de considérer également les autres références à l’ancienne URL.

## Défi collectif

Scénario :

> « Déplacer `https://staging.exemple.test` vers `https://www.exemple.fr`. »

Remettez dans l’ordre :

1. sauvegarde ;
2. transfert ;
3. configuration ;
4. remplacement des URLs ;
5. vérification ;
6. nettoyage éventuel.

L’animateur réalise les commandes sur le serveur.
