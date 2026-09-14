# TP7 — Manipuler un type de contenu personnalisé

## Mission

On vous confie un site qui gère des **Livres** (un type de contenu personnalisé, ou CPT).
Sans ouvrir le back-office : inventoriez ce type, remplissez-le, puis faites le ménage — le tout en ligne de commande.

## Prérequis

Le type de contenu `livre` et la taxonomie `genre` sont fournis par l'extension **Atelier CPT**, déjà présente sur l'environnement. Un CPT n'existe que s'il est enregistré par du code : WP-CLI ne « crée » pas un type persistant, il manipule un type existant.

## Étape 1 — Observer

```bash
wp post-type list
wp post-type get livre
wp taxonomy list
```

Question : comment savoir, sans le navigateur, quels types de contenu un site gère vraiment ?

## Étape 2 — Remplir

Créer quelques livres :

```bash
wp post create --post_type=livre --post_title='Le Petit Prince' --post_status=publish
wp post create --post_type=livre --post_title='Fondation' --post_status=publish
```

Puis en générer en masse :

```bash
wp post generate --post_type=livre --count=10
```

Vérifier :

```bash
wp post list --post_type=livre --fields=ID,post_title,post_status
```

## Étape 3 — Ranger

Créer un genre et l'affecter à un livre (récupérer un ID via l'étape 2) :

```bash
wp term create genre "Science-fiction"
wp post term set <ID_du_livre> genre science-fiction
```

## Étape 4 — Faire le ménage (suppression en masse)

Observer avant de supprimer, toujours :

```bash
wp post list --post_type=livre --format=ids
```

Puis supprimer d'un coup, en réutilisant la sortie de la commande précédente :

```bash
wp post delete $(wp post list --post_type=livre --format=ids) --force
```

Vérifier que la liste est vide :

```bash
wp post list --post_type=livre
```

## À discuter

- Que fait `--force` sur une suppression ? Que se passe-t-il sans lui pour un CPT ?
- Que renvoie `$(wp post list ... --format=ids)` si aucun livre n'existe — et que devient la commande de suppression dans ce cas ?
- Si la liste d'IDs est très longue, quelles limites peut-on rencontrer ?
- Pourquoi le type « Livre » disparaît-il de `wp post-type list` si on désactive l'extension Atelier CPT, alors que les livres restent en base ?

## Bonus (avancé)

- Explorer le contenu en PHP : `wp eval 'echo wp_count_posts("livre")->publish;'`
  (rappel : `wp eval` exécute du PHP arbitraire — outil de dev/debug, jamais à l'aveugle sur une prod).
- Exporter uniquement les livres : `wp export --post_type=livre`.

## Pour aller plus loin — traiter de gros volumes (avancé · shell)

> Optionnel, et ce n'est plus du WP-CLI mais du shell. À sauter si vous débutez ; à explorer côté serveur plutôt que dans Playground (où `xargs` peut être incomplet).

Passer **tous** les IDs d'un coup avec `$(…)` atteint une limite du shell sur des milliers d'éléments (« Argument list too long »). On découpe alors le travail en lots avec `xargs` :

```bash
wp post list --post_type=livre --format=ids | xargs -n 100 wp post delete --force
```

`-n 100` = 100 IDs maximum par appel. Ajouter `-r` évite de lancer la commande si la liste est vide.