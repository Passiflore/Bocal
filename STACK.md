# Bocal — Stack technique

Ce document récapitule les choix techniques du MVP et leurs raisons.
Les règles produit sont dans [MVP.md](MVP.md).

## Vue d'ensemble

```
[Navigateur]                    [API]                         [Base de données]
React + TypeScript   ── HTTP ──►  Node.js + Express + TS  ──►  PostgreSQL
(Vite)               ◄── JSON ──  (règles métier)              (via Drizzle)
```

Le frontend et l'API sont **deux programmes séparés**, écrits tous les deux en TypeScript. React ne parle jamais directement à la base : il demande, et l'API décide.

## Les choix

### Architecture

| Choix | Pourquoi |
|---|---|
| **Frontend React et API séparée** (plutôt qu'un BaaS comme Supabase ou un framework fullstack comme Next.js) | La frontière entre front et back est claire et visible. Les règles métier sont écrites en TypeScript lisible et testable. Un seul langage partout |

### Frontend

| Besoin | Choix | Pourquoi |
|---|---|---|
| Build et serveur de dev | **Vite** | Le standard actuel, rapide et simple |
| Navigation | **React Router** | Le plus utilisé et le plus documenté |
| Données serveur | **`fetch` + `useEffect`** sur 1 ou 2 écrans, puis **TanStack Query** | Rencontrer d'abord les problèmes de l'état serveur (chargement, erreurs, cache, données périmées), puis adopter l'outil qui les résout |
| État global | **Aucun.** Un Context React pour l'utilisateur connecté | YAGNI : TanStack Query gère l'état serveur, il reste très peu d'état client partagé |
| Style | **CSS Modules** et **variables CSS** (*design tokens*) | Liberté créative pour l'identité scrapbooking. Changer les tokens suffit à faire évoluer toute l'app |
| Pop-ups | Élément HTML natif **`<dialog>`** | Accessibilité de base incluse (touche Échap, focus), sans dépendance |

### Backend

| Besoin | Choix | Pourquoi |
|---|---|---|
| Framework d'API | **Express** | Montre les fondations (routes, middlewares) sans rien cacher. Énormément de ressources |
| Validation | **Zod**, avec des schémas **partagés entre le front et le back** | L'API ne fait jamais confiance aux données reçues. Une seule description sert au formulaire, à l'API et aux types TypeScript |
| Base de données | **PostgreSQL** | Des données très relationnelles, et des contraintes qui garantissent certaines règles dans la base même (une seule idée `tirée` par groupe) |
| Accès à la base | **Drizzle** (ORM) et ses **migrations** | Typé, et proche du SQL : ce qu'on apprend se transfère directement au SQL |
| Authentification | **Better Auth** : email et mot de passe, sessions en base, cookies `httpOnly` | La partie sensible (hachage, sessions, cookies) est gérée de façon sûre, et les utilisateurs restent dans notre base |

### Tests

| Type | Outil | Priorité |
|---|---|---|
| Unitaires (logique métier pure) | **Vitest** | 1 : expiration, tirage avec filtres, relance |
| Intégration (routes de l'API) | **Vitest + Supertest**, sur une vraie base PostgreSQL de test | 2 : permissions (`403`), bocal mystère, double tirage |
| Composants React | **Vitest + React Testing Library** | 3 : formulaires et pop-ups |

Les critères d'acceptation de `MVP.md` servent de base aux tests.

## Principes techniques

- **Le serveur est la source de vérité.** Toutes les règles métier et les permissions sont vérifiées par l'API. Ce que l'utilisateur n'a pas le droit de voir n'est jamais envoyé au navigateur.
- **Authentification ≠ autorisation.** L'authentification répond à « qui es-tu ? » (`401`), l'autorisation à « as-tu le droit ? » (`403`).
- **L'état dérivé plutôt que l'état stocké.** Exemple : l'expiration d'un tirage est calculée, pas stockée.
- **Les règles métier vivent dans des fonctions pures**, séparées d'Express et de la base. Elles reçoivent la date actuelle en paramètre, pour pouvoir être testées.
- **Mobile-first.**
- **Le plus gratuit possible** pour l'hébergement et les services.

## Décidé plus tard

| Sujet | Quand |
|---|---|
| Hébergement (frontend, API, base de données) | Au moment de la mise en ligne, en comparant les offres gratuites du moment |
| Tests E2E (Playwright) | Une fois le MVP fonctionnel |
| Temps réel (WebSockets) | Hors MVP, il se branchera par-dessus TanStack Query |
