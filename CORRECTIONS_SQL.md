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

## 📌 Auteur

Projet réalisé par **Jocelyne Muller**, dans le cadre d’un projet scolaire sur les bases de données MySQL.

