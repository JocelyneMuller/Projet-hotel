# Tâche 1 : Debugging

# Tâche 1.1 – Expliquer votre démarche

Lors de l’importation de données dans une base MySQL, il faut être particulièrement vigilant à la cohérence avec les contraintes définies dans la base, notamment :

# Corrections apportées au fichier reservation_hotel_exercice.sql

Ce fichier décrit les corrections manuelles effectuées pour corriger les erreurs SQL dans le fichier `reservation_hotel_exercice.sql`, afin de permettre une importation correcte dans phpMyAdmin avec MAMP/MySQL 8.

## ✅ Liste des corrections

### 1. Remplacement d'un nom de colonne erroné
- **Ancien** : `numChambre`
- **Corrigé** : `numero`
- 📌 Raison : la colonne `numChambre` n'existait pas dans la table `chambres`, le bon nom était `numero`.

---

### 2. Correction des contraintes de clés étrangères (`FOREIGN KEY`)

Plusieurs lignes contenaient une mauvaise syntaxe :

- ❌ `ADD CONSTRAINT ... KEY (...) REFERENCES ...`
- ✅ `ADD CONSTRAINT ... FOREIGN KEY (...) REFERENCES ...`

Les corrections ont été appliquées dans les tables suivantes :
- `chambres`
- `chambre_type_couchage`
- `tarifs`

---

### 3. Suppression d’un doublon `ALTER TABLE`

- ❌ Problème : deux fois `ALTER TABLE tarifs`
- ✅ Solution : une seule ligne `ALTER TABLE` suivie des `ADD CONSTRAINT`

---

## 💡 Astuce

Pour vérifier les erreurs avant import :
- Utiliser **phpMyAdmin** sur `http://localhost:8888/phpMyAdmin`
- Importer dans une base propre nommée `pp_hotel`
- Corriger chaque erreur et versionner via Git

---

# Corrections apportées au fichier reservation_hotel-peuplement.sql

Ce fichier contient les données d’insertion pour peupler la base de données `reservation_hotel`.

## ✅ Correction appliquée

### ❌ Problème détecté :
Une erreur SQL apparaissait lors de l’importation à cause d’une **violation de contrainte de clé étrangère** :

```sql
INSERT INTO `chambres` ... (29, 1, 5, '5', 'Reservée aux VIP');
```

### 🛠 Explication :
La ligne tentait d’insérer une chambre avec :
- `id_hotel = 1`
- `id_type = 5`

Or, cette combinaison (`id_hotel = 1` et `id_type = 5`) **n'était pas présente dans la table `tarifs`**, ou bien `id_type = 5` n'existait pas dans `chambre_types`.

### ✅ Solution :
La ligne fautive a été **supprimée** du script pour permettre l’importation complète sans erreur.

---

## 📁 Fichier concerné

- `reservation_hotel-peuplement_corrige.sql` : version modifiée pour import réussi

---

# Tâche 1.2 – Vigilance lors de l'importation de données

Lors de l’importation de données dans une base MySQL, il faut être particulièrement vigilant à la cohérence avec les contraintes définies dans la base, notamment :

> Pour importer des données avec succès, il faut **respecter l’ordre logique d’insertion**, vérifier que **toutes les clés étrangères pointent vers des enregistrements existants**, que **les types de données sont cohérents**, et qu’il n’y a pas de **doublons** interdits.


# Tâche 1.3 – Cas particulier : chambre indisponible

## 🧩 Contexte

La chambre 2 de l’hôtel **“Ski Hôtel”** est actuellement **en travaux** et **ne doit pas être disponible à la réservation**.

---

## 🎯 Objectif 
c'est de traiter ce cas **sans supprimer** la chambre de la base de données, tout en **empêchant sa réservation**.

---

## ✅ Solution : ajouter une colonne `disponible` à la table `chambres`

### 1. Ajouter une colonne de disponibilité :

```sql
ALTER TABLE chambres ADD disponible BOOLEAN DEFAULT TRUE;
```

Cette colonne permettra d’indiquer si une chambre est disponible (`TRUE`) ou non (`FALSE`).

---

### 2. Mettre la chambre concernée comme indisponible :

```sql
UPDATE chambres
SET disponible = FALSE, commentaire = 'En travaux'
WHERE numero = '2'
  AND id_hotel = (
    SELECT id FROM hotels WHERE nom = 'Ski Hôtel'
  );
```

---
# Tâche 2.1 – Effet de l'option ON DELETE sur la suppression d'un enregistrement

## 🔍 Expérience réalisée

Lorsque j’ai exécuté la requête suivante dans phpMyAdmin :

```sql
DELETE FROM hotels WHERE id = 1;
```

L’enregistrement a bien été supprimé.


Cela signifie que les contraintes de clé étrangère ont été définies avec l’option :

```sql
ON DELETE CASCADE
```

Grâce à cette option, **toutes les lignes liées à l’hôtel** dans d'autres tables (comme `chambres`, `tarifs`, ou `reservations`) ont été **supprimées automatiquement**.

---

## J'ai refait l'exercice avec l’option sur `RESTRICT`

> Comportement par défaut dans MySQL

La suppression a été **bloquée** avec un message d’erreur de type :
```
Cannot delete or update a parent row: a foreign key constraint fails
```

---

## 💡 Conclusion

Cette expérience montre l’**impact direct du choix de l’option `ON DELETE`** sur la **logique de suppression dans une base relationnelle** :
- `CASCADE` : suppression automatique des dépendances
- `RESTRICT` : protection stricte contre la suppression accidentelle

## 📌 Auteur

Projet réalisé par **Jocelyne Muller**, dans le cadre d’un projet scolaire sur les bases de données MySQL.

