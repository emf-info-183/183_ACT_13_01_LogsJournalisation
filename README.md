# Activité – Logs techniques et journal d’audit

## Durée
120 minutes

## Sujet
Mise en place de la journalisation technique et d’un journal d’audit dans une application web sécurisée.

---

## Objectifs

À la fin de l'activité, l'élève est en mesure de :

- Comprendre l’importance de la journalisation dans une application web
- Mettre en place un logger technique avec Winston
- Journaliser les requêtes HTTP dans un fichier de logs
- Observer et analyser les logs générés
- Faire la différence entre un log technique et un journal d’audit
- Créer un journal d’audit en base de données MySQL
- Tracer automatiquement les actions sensibles des utilisateurs
- Afficher les événements d’audit dans une interface d’administration

---

## Modalités

- Travail individuel  
- Application Node.js / Express fournie  
- Base de données MySQL existante  

---

## Ressources

- Structure de projet fournie (archive ZIP)
- Documentation Winston : https://github.com/winstonjs/winston
- Documentation Express
- Accès à MySQL (local ou conteneur)

---

## Introduction

Dans une application réelle, il est indispensable de garder des traces :
- pour comprendre ce que fait l’application,
- pour diagnostiquer des erreurs,
- pour détecter des comportements anormaux,
- et pour savoir **qui a fait quoi**.

Cette activité vous propose de mettre en place **deux types de journalisation complémentaires** :
1. une journalisation technique (logs),
2. un journal d’audit applicatif.

---

## Travail à réaliser

---

### Étape 1 – Prise en main du projet (10 min)

1. Décompressez l’archive du projet fournie.
2. Installez les dépendances :
   ```bash
   npm install
   ```
3. Démarrez l’application :
   ```bash
   npm start
   ```
4. Vérifiez que l’API fonctionne correctement avant toute modification.

---

### Étape 2 – Installation de Winston (15 min)

1. Installez Winston dans le projet :
   ```bash
   npm install winston
   ```
2. Créez un dossier `logs/` à la racine du projet.
3. Créez un fichier `logger.js` (ou équivalent) chargé de configurer Winston.
4. Configurez un logger qui :
   - écrit dans un fichier `logs/access.log`
   - utilise le niveau `info`
   - ajoute la date et le niveau de log dans chaque entrée

---

### Étape 3 – Journalisation des requêtes HTTP (20 min)

1. Importez votre logger dans le fichier principal du serveur Express.
2. Créez un middleware Express qui journalise chaque requête reçue.
3. Pour chaque requête, enregistrez une ligne de log contenant :
   - la méthode HTTP
   - l’URL appelée
   - l’utilisateur (ou `anonymous` si non connecté)

Exemple attendu :
```js
logger.info(`${req.method} ${req.url} by ${user}`);
```

4. Testez plusieurs routes de l’API.
5. Ouvrez le fichier `logs/access.log` et observez son contenu.

---

### Étape 4 – Analyse et réflexion (3`0 min)

📌 **Question de réflexion (à faire par écrit)** :

- À quoi servent les logs techniques que vous venez de créer ?
- En quoi ces logs sont-ils utiles pour :
  - le développeur ?
  - l’administrateur système ?
  - la sécurité ?

💡 **Discutez ensuite avec l’enseignant de la différence entre :**
- un log technique,
- un journal d’audit.

> ⚠️ Aucune implémentation n’est demandée à cette étape, seulement une réflexion.

---

### Étape 5 – Création du journal d’audit (MySQL) (15 min)

1. Créez une table `audit_log` dans la base de données MySQL avec au minimum :
   - un identifiant
   - l’utilisateur
   - l’action effectuée
   - la date / heure

2. Exemple de requête attendue :
```sql
INSERT INTO audit_log (user, action, timestamp)
VALUES (...);
```

3. Vérifiez que la table est correctement créée.

---

### Étape 6 – Enregistrement automatique des actions (20 min)

1. Identifiez dans l’application **au moins une action sensible**, par exemple :
   - création,
   - modification,
   - suppression d’une ressource.

2. Lors de cette action :
   - insérez automatiquement une ligne dans la table `audit_log`
   - indiquez clairement :
     - l’utilisateur
     - l’action réalisée

3. Testez plusieurs actions et vérifiez le contenu de la table `audit_log`.

---

### Étape 7 – Page d’audit /admin/audit (15 min)

1. Créez une route `/admin/audit`.
2. Cette page doit :
   - récupérer les dernières entrées de la table `audit_log`
   - afficher la liste des actions récentes (utilisateur, action, date)

3. L’affichage peut être simple (HTML brut ou JSON).

---

## Livraison attendue

- Un projet fonctionnel avec :
  - un fichier `access.log` alimenté automatiquement
  - une table `audit_log` remplie
  - une route `/admin/audit` accessible
- Une réponse écrite expliquant la différence entre :
  - log technique
  - journal d’audit

---

## Bonus

- Ajouter un niveau `warn` pour des actions suspectes
- Ajouter un niveau `error` pour les erreurs serveur
- Limiter l’accès à `/admin/audit` à un rôle administrateur

---

## Conclusion

Cette activité illustre un principe fondamental de la sécurité applicative :

> *Ce qui n’est pas tracé ne peut ni être compris, ni sécurisé.*

Les logs et les journaux d’audit sont des outils essentiels pour développer des applications fiables, maintenables et sécurisées.
