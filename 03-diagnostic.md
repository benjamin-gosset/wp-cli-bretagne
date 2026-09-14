# TP3 — Le site est cassé !

## Mission

Un utilisateur vous dit :

> « Le site ne fonctionne plus depuis la dernière manipulation. »

Vous n’avez pas accès au back-office. Diagnostiquez le problème avec WP-CLI.

## 1. Observer avant d’agir

```bash
wp plugin list
wp core version
wp theme list
```

**Question :** quelles informations voulez-vous obtenir avant de désactiver quoi que ce soit ?

## 2. Rechercher l’extension suspecte

Une extension nommée `atelier-incident` est prévue pour cet exercice.

```bash
wp plugin list
wp plugin get atelier-incident
```

## 3. Reproduire le problème

Activez-la :

```bash
wp plugin activate atelier-incident
```

Le scénario utilise une option pour déclencher volontairement l’incident.

## 4. Diagnostiquer

```bash
wp option get atelier_incident_enabled
```

Si besoin :

```bash
wp help option
```

**Question :** que vous apprend cette valeur ?

## 5. Rétablir le site

Remettez le site dans un état fonctionnel :

```bash
wp plugin deactivate atelier-incident
wp plugin list
wp option get atelier_incident_enabled
```

## 6. Aller plus loin

Si vous ne saviez pas quel plugin était responsable, construisez une méthode :

1. observer les plugins ;
2. identifier les suspects ;
3. isoler ;
4. tester ;
5. réactiver progressivement.

**Défi :** comment désactiver plusieurs extensions en une seule commande ?

```bash
wp help plugin deactivate
```

## À retenir

WP-CLI peut aussi servir à **reprendre la main sur un site lorsque l’interface d’administration n’est plus exploitable**.
