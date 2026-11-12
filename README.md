# 🫙 Bocal

**Le bocal à idées de sorties de ta bande d'amis.**

Chacun glisse des idées d'activités dans un bocal commun. Chaque semaine, le groupe en tire une au hasard, la réalise, puis en garde un souvenir.

> 🚧 **Projet en cours de conception.** Le périmètre du MVP et la stack technique sont définis, le développement n'a pas encore commencé.

---

## Le concept

```
Créer un groupe → Inviter ses amis → Remplir le bocal → Tirer une activité → La réaliser → Garder un souvenir
```

- **Un bocal mystère** : on voit combien d'idées attendent dans le bocal, avec leur type, leur budget et leur durée, mais leur contenu reste secret jusqu'au tirage.
- **Un tirage au sort**, filtrable par budget et par durée, pour les semaines où l'on est fauché ou pressé.
- **7 jours pour la réaliser.** Passé ce délai, le groupe dit si l'activité a été faite. Sinon, elle retourne dans le bocal.
- **Une relance** par tirage, si l'idée tirée ne plaît vraiment pas.
- **Des souvenirs** : les membres commentent les activités réalisées.

À terme, Bocal aura une identité visuelle inspirée du **scrapbooking**, des collages et des journaux de souvenirs, avec des **stickers** à débloquer en réalisant des activités.

## Fonctionnalités du MVP

- [ ] Inscription et connexion
- [ ] Création d'un groupe et invitation par lien
- [ ] Ajout d'idées (titre, description, budget, durée, type)
- [ ] Bocal mystère
- [ ] Tirage au sort avec filtres
- [ ] Relance unique
- [ ] Compte à rebours de 7 jours et confirmation de réalisation
- [ ] Commentaires sur les activités réalisées

Le détail des règles, du cycle de vie d'une idée et des critères d'acceptation est dans **[MVP.md](MVP.md)**.

## Stack technique

| | |
|---|---|
| **Frontend** | React · TypeScript · Vite · React Router · TanStack Query · CSS Modules |
| **Backend** | Node.js · Express · TypeScript · Zod |
| **Base de données** | PostgreSQL · Drizzle ORM |
| **Authentification** | Better Auth (sessions, cookies `httpOnly`) |
| **Tests** | Vitest · React Testing Library · Supertest |

Les choix et leurs raisons sont détaillés dans **[STACK.md](STACK.md)**.

## Feuille de route

- [x] Cadrage du périmètre du MVP
- [x] Backlog et critères d'acceptation
- [x] Choix de la stack technique
- [x] Modélisation des données
- [ ] Mise en place du projet et de l'outillage
- [ ] Développement du MVP
- [ ] Mise en ligne

**Après le MVP :** photos, stickers, identité visuelle scrapbooking, notifications, plusieurs groupes par utilisateur…

## Lancer le projet

*Instructions à venir, une fois le projet initialisé.*
