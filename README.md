# Atelier WP-CLI — La Maison des Contributeurs

Support de la table WP-CLI de la journée de contribution (WordCamp Bretagne).
Un environnement de démonstration jetable qui se monte dans le navigateur avec WordPress Playground, des exercices progressifs (TP1 à TP5) et les fiches d'animation.

## Démarrer l'atelier (rien à installer)

Ouvrir ce lien dans le navigateur :

```
https://playground.wordpress.net/?blueprint-url=https://raw.githubusercontent.com/benjamin-gosset/wp-cli-bretagne/main/blueprint.json
```

Le site « La Maison des Contributeurs » se construit automatiquement. Ouvrir ensuite le **Terminal** depuis le menu de l'instance Playground et taper `wp` pour vérifier que la commande répond.

> Recharger l'onglet réinitialise tout : l'environnement est jetable, c'est normal.

## Ce que monte le blueprint

- des utilisateurs de rôles variés (administrateurs, éditeur, auteur, contributeur) ;
- des catégories, étiquettes, articles et pages ;
- deux menus (explorables en CLI ; non rendus en façade, le thème actif étant un thème de blocs) ;
- plusieurs extensions, actives et inactives ;
- un plugin « Atelier Incident » piloté par une option, pour le TP3 (diagnostic).

Compte administrateur : `admin`. Mots de passe et données sont destinés à cet atelier local — ne jamais les réutiliser ailleurs.

`wp db export` et `wp db query` ne fonctionnent pas sous Playground (base SQLite) : ces démonstrations se font sur le serveur.

## Par où commencer ?

| Vous… | Commencez par |
|---|---|
| découvrez WP-CLI | TP1 — Prise en main |
| utilisez déjà WordPress | TP2 — Maintenance |
| administrez des sites | TP3, TP4, TP6 |
| développez / automatisez | TP5, TP7 |

Chaque TP est indépendant : entrez par celui qui vous parle.

Règle d'or : **observer avant de modifier**.

## Ressources

- Site officiel : https://wp-cli.org/fr/
- Documentation (handbook) : https://make.wordpress.org/cli/handbook/
- WordPress Playground : https://wordpress.org/playground/

## Licence

Vérifier la licence de tout thème ou contenu redistribué ici. Les thèmes et extensions issus des répertoires WordPress.org sont sous licence GPL.