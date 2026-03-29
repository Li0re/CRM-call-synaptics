# Brief — CRM Synaptic pour Claude Code

## Contexte

Tu vas améliorer et finaliser un CRM de prospection commerciale pour une équipe de vente.
Le fichier de base est `crm-synaptic.html` — un fichier HTML unique, sans framework, sans backend, sans base de données.
Tout tourne en local dans le navigateur. Le fichier sera hébergé sur **GitHub Pages**.

---

## Ce que fait déjà le CRM

- **Pipeline Sankey** : visualisation en rubans qui rétrécissent à chaque étape (Nouveaux → Contactés → Proposition → Négociation → Gagnés). Les rubans sont cliquables et affichent un tooltip au survol.
- **KPI cards** : 5 cartes en haut avec le nombre de prospects par étape + taux de conversion.
- **Filtres période** : Ce mois / Trimestre / Année — met à jour le Sankey et les KPI.
- **Tableau des prospects** : colonnes Prospect, Entreprise, Statut, Source, Valeur, Score, Responsable, Date, Note, Actions.
- **Filtres tableau** : recherche texte + filtre statut + filtre responsable.
- **Modal ajout/modification** : formulaire complet pour créer ou éditer un prospect.
- **Suppression** : avec confirmation.
- **Taux de conversion par étape** : barre de progression colorée.
- **Performance par commercial** : Sophie L., Paul M., Léa T.
- **Dark mode** : détection automatique via `prefers-color-scheme`.
- **Données de test** : 8 prospects pré-chargés en mémoire.

---

## Stack technique

| Élément | Choix |
|---|---|
| Framework | Aucun — HTML/CSS/JS vanilla |
| Base de données | Aucune pour l'instant (localStorage possible plus tard) |
| Auth | Aucune pour l'instant (Firebase Auth à brancher plus tard) |
| Hébergement | GitHub Pages |
| Dépendances externes | Aucune — tout est inline |

---

## Ce qu'il faut améliorer / ajouter

### 1. Design général
- Rendre l'interface plus **moderne et attractive**
- Améliorer la typographie, l'espacement, les micro-détails
- Le pipeline Sankey doit rester la pièce maîtresse visuelle
- Garder le support dark mode

### 2. Tableau des prospects
- Ajouter une colonne **Téléphone** et **Email**
- Ajouter la possibilité de **changer le statut directement** depuis le tableau (dropdown inline)
- Ajouter un indicateur visuel de **relance urgente** si le dernier contact date de plus de 7 jours
- Trier les colonnes au clic sur l'en-tête

### 3. Pipeline
- Afficher la **valeur totale en € par étape** sous le nombre de prospects
- Rendre le Sankey plus lisible sur mobile (responsive)

### 4. Persistance locale
- Utiliser **localStorage** pour sauvegarder les prospects entre les sessions
- Au premier chargement, utiliser les données de test si localStorage est vide

### 5. Export
- Bouton **"Exporter CSV"** qui télécharge tous les prospects visibles (selon les filtres actifs)

### 6. Vue Kanban (optionnel si le temps le permet)
- Un onglet "Kanban" en plus du tableau, avec une colonne par statut
- Drag & drop pour changer le statut d'un prospect

---

## Structure des données

Chaque prospect est un objet JS avec ces champs :

```javascript
{
  id: '1',                        // string unique
  nom: 'Martin Dupont',
  initiales: 'MD',
  av: '#B5D4F4:#0C447C',          // couleurs avatar (bg:fg)
  entreprise: 'TechSoft SA',
  statut: 'nego',                 // new | contact | prop | nego | gagne | perdu
  source: 'LinkedIn',             // LinkedIn | Salon | Référence | Site web | Email | Appel sortant
  valeur: 12500,                  // nombre en €
  score: 82,                      // 0-100
  resp: 'Sophie L.',              // Sophie L. | Paul M. | Léa T.
  date: '2026-03-27',             // ISO date du dernier contact
  note: 'RDV prévu semaine prochaine',
  // à ajouter :
  telephone: '+33 6 00 00 00 00',
  email: 'martin.dupont@techsoft.fr'
}
```

---

## Palette de couleurs (à respecter)

```css
/* Statuts */
new     → bleu    #E6F1FB / #185FA5
contact → violet  #EEEDFE / #534AB7
prop    → amber   #FAEEDA / #854F0B
nego    → teal    #EAF3DE / #0F6E56
gagne   → vert    #EAF3DE / #27500A
perdu   → rouge   #FCEBEB / #A32D2D

/* Pipeline Sankey */
Nouveaux   → #B5D4F4 / stroke #185FA5
Contactés  → #CECBF6 / stroke #534AB7
Proposition→ #FAC775 / stroke #854F0B
Négociation→ #9FE1CB / stroke #0F6E56
```

---

## Équipe commerciale

- **Sophie L.** — avatar `#B5D4F4` / `#0C447C`
- **Paul M.**   — avatar `#C0DD97` / `#27500A`
- **Léa T.**    — avatar `#CECBF6` / `#3C3489`

---

## Intégration Firebase (plus tard — ne pas faire maintenant)

Quand on sera prêt à brancher Firebase :

1. Ajouter les 3 scripts SDK Firebase en haut du `<head>`
2. Dans le JS, remplacer `loadProspects()` par `db.collection('prospects').onSnapshot(...)`
3. Remplacer les mutations locales (push, splice) par `db.collection('prospects').add/update/delete`
4. Activer `auth.signInWithEmailAndPassword` pour la page de login

Le fichier est conçu pour que cette migration soit simple — toutes les mutations passent par des fonctions isolées (`saveProspect`, `deleteProspect`, `loadProspects`).

---

## Contraintes importantes

- **Un seul fichier HTML** — pas de fichiers JS ou CSS séparés
- **Aucune dépendance CDN** — tout doit fonctionner hors ligne et sans import
- **Pas de framework** — vanilla JS uniquement
- **Compatible** : Chrome, Firefox, Safari, Edge — mobile inclus
- **GitHub Pages ready** : pas de serveur, pas de build step

---

## Fichier de départ

Le fichier `crm-synaptic.html` est le point de départ. Toutes les modifications se font dessus.
