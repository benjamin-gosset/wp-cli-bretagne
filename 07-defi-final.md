# Défi final — Vous avez 10 minutes

## Scénario

Vous arrivez sur **La Maison des Contributeurs** après une intervention d’un autre développeur.

On vous dit :

- le site doit rester accessible ;
- une extension de test a été installée ;
- un nouvel utilisateur doit être créé ;
- le contenu doit être enrichi ;
- une procédure reproductible doit être laissée pour les vérifications.

## Mission

Sans utiliser le back-office :

### 1. Faire l’état des lieux

Trouvez :

- version de WordPress ;
- thème actif ;
- plugins actifs ;
- nombre d’utilisateurs ;
- nombre d’articles.

### 2. Corriger

Identifiez une extension inutile ou inactive et décidez quoi en faire.

### 3. Administrer

Créez un utilisateur de test avec le rôle approprié.

### 4. Enrichir

Créez un article publié décrivant une contribution à WordPress.

### 5. Automatiser

Construisez une commande ou une courte suite de commandes qui reproduit votre état des lieux.

## Critères de réussite

Vous avez réussi si :

- vous n’avez pas utilisé le back-office ;
- vous avez vérifié chaque action ;
- vous pouvez expliquer vos choix ;
- votre procédure peut être rejouée par quelqu’un d’autre.

## Bonus

Trouvez une opération utile non abordée dans les TPs et présentez-la au groupe.

Quelques pistes :

```bash
wp media regenerate
wp post generate
wp plugin update --all
wp transient delete --expired
```
