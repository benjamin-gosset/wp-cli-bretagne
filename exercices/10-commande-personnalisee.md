# TP9 — Créer sa propre commande WP-CLI

**Pour qui :** développeurs, à l'aise en PHP. Nécessite un accès shell (Playground seul ne suffit pas pour la mise au point). L'objectif n'est pas de tout couvrir, mais de **comprendre le mécanisme** : comment WP-CLI transforme une fonction PHP en commande.

> Rappel du contexte : certaines extensions que vous connaissez (WooCommerce, Yoast, WP Migrate…) ajoutent déjà leurs propres commandes `wp`. Ce TP montre comment elles font — et comment en écrire une.

## Le principe

Une commande WP-CLI, c'est une fonction (ou une classe) PHP enregistrée via `WP_CLI::add_command()`. Le code doit vivre dans un plugin (ou le thème), et ne se charger **qu'en contexte CLI** — sinon il s'exécute aussi sur le site web et peut le casser.

La garde indispensable :

```php
if ( defined( 'WP_CLI' ) && WP_CLI ) {
    // …enregistrement de la commande ici seulement
}
```

## Exemple 1 — une commande minimale

Un plugin `atelier-commande/atelier-commande.php` :

```php
<?php
/**
 * Plugin Name: Atelier Commande
 * Description: Exemple de commande WP-CLI personnalisée pour l'atelier.
 */

if ( defined( 'WP_CLI' ) && WP_CLI ) {

    /**
     * Dit bonjour.
     *
     * ## OPTIONS
     *
     * [<nom>]
     * : Le nom à saluer. Par défaut « contributeur ».
     *
     * ## EXAMPLES
     *
     *     wp atelier bonjour
     *     wp atelier bonjour Benjamin
     */
    WP_CLI::add_command( 'atelier bonjour', function ( $args ) {
        $nom = $args[0] ?? 'contributeur';
        WP_CLI::success( "Bonjour, {$nom} !" );
    } );
}
```

Test :

```bash
wp plugin activate atelier-commande
wp atelier bonjour
wp atelier bonjour Benjamin
wp help atelier bonjour        # la doc vient des commentaires ci-dessus
```

Point clé : le bloc de commentaires (`## OPTIONS`, `## EXAMPLES`) **est** la documentation de la commande. WP-CLI la lit pour générer `wp help`.

## Exemple 2 — une commande utile

Compter les contenus par type, avec une option de format. Ajouter dans le même bloc :

```php
    /**
     * Compte les contenus publiés par type.
     *
     * ## OPTIONS
     *
     * [--format=<format>]
     * : table (défaut), csv ou json.
     *
     * ## EXAMPLES
     *
     *     wp atelier stats
     *     wp atelier stats --format=json
     */
    WP_CLI::add_command( 'atelier stats', function ( $args, $assoc_args ) {
        $format = $assoc_args['format'] ?? 'table';
        $types  = get_post_types( array( 'public' => true ), 'names' );

        $lignes = array();
        foreach ( $types as $type ) {
            $compte = wp_count_posts( $type );
            $lignes[] = array(
                'type'    => $type,
                'publies' => (int) ( $compte->publish ?? 0 ),
            );
        }

        WP_CLI\Utils\format_items( $format, $lignes, array( 'type', 'publies' ) );
    } );
```

Test :

```bash
wp atelier stats
wp atelier stats --format=json
```

Ici on réutilise `format_items()` de WP-CLI : votre commande hérite gratuitement de `--format=table/csv/json`, comme les commandes natives.

## À discuter

- Pourquoi la garde `if ( defined( 'WP_CLI' ) && WP_CLI )` est-elle indispensable ? Que se passerait-il sans elle quand un visiteur charge le site ?
- D'où vient la sortie de `wp help atelier stats` ? Qu'est-ce que ça implique pour la façon d'écrire ses commentaires ?
- `$args` (positionnels) vs `$assoc_args` (options `--clé=valeur`) : quand utiliser l'un ou l'autre ?

## Pour aller plus loin (mention, pas exercice)

- Enregistrer une **classe** plutôt qu'une closure, avec une méthode par sous-commande (`wp atelier bonjour`, `wp atelier stats` regroupés).
- Empaqueter ses commandes dans un **package WP-CLI** distribuable via Composer.

Ce sont des pistes : le cœur du TP, c'est le mécanisme `add_command` + la garde `WP_CLI` + la doc auto. Le reste s'explore ensuite.

## Réserves techniques

- Code donné selon l'API WP-CLI connue ; à **tester** avant présentation (`WP_CLI\Utils\format_items` et la signature des closures peuvent varier selon la version).
- Dans Playground, l'enregistrement d'une commande custom via plugin est à vérifier au boot ; la mise au point se fait plus confortablement sur un vrai serveur.
