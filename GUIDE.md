# Guide de mise en ligne — Cabinet Chartreuse

Ce projet est une application Next.js. La partie publique affiche vos bouteilles,
la partie `/admin` vous permet de les ajouter/modifier/supprimer (avec photo)
après connexion par mot de passe. Les données sont stockées dans **Vercel Blob**,
directement dans votre compte Vercel : pas de service externe à créer.

Suivez les étapes dans l'ordre. Comptez 20 à 30 minutes la première fois.

---

## Étape 1 — Mettre le code sur GitHub

Vercel déploie à partir d'un dépôt GitHub.

1. Créez un compte sur https://github.com si besoin.
2. Créez un nouveau dépôt (bouton **New repository**), nom libre, laissez-le vide (pas de README).
3. Dans un terminal, à la racine de ce projet :
   ```
   git init
   git add .
   git commit -m "Site initial Cabinet Chartreuse"
   git branch -M main
   git remote add origin https://github.com/VOTRE-COMPTE/VOTRE-DEPOT.git
   git push -u origin main
   ```

## Étape 2 — Créer le projet sur Vercel

1. Allez sur https://vercel.com, connectez-vous avec votre compte GitHub.
2. Cliquez sur **Add New** → **Project**, sélectionnez le dépôt que vous venez de créer.
3. Vercel détecte automatiquement Next.js. **Ne cliquez pas encore sur Deploy**,
   on doit d'abord préparer le stockage (étape suivante), sinon le premier
   déploiement plantera faute de variables.

   Si vous avez déjà cliqué sur Deploy et que ça a échoué : ce n'est pas grave,
   continuez le guide puis relancez un déploiement à la fin (étape 5).

## Étape 3 — Activer Vercel Blob (le stockage de vos bouteilles et photos)

1. Dans votre projet Vercel, allez dans l'onglet **Storage**.
2. Cliquez sur **Create Database** (ou **Browse Marketplace**) → choisissez **Blob**.
3. Donnez-lui un nom (ex: `chartreuse-data`), validez, puis **Connect** au projet
   `cabinet-chartreuse` si ce n'est pas automatique.
4. Une fois connecté, Vercel ajoute automatiquement la variable
   `BLOB_READ_WRITE_TOKEN` aux variables d'environnement de votre projet.
   Vous n'avez rien à copier-coller pour celle-ci.

## Étape 4 — Ajouter vos deux variables restantes

Toujours dans Vercel : **Settings** → **Environment Variables**, ajoutez :

| Nom | Valeur |
|---|---|
| `ADMIN_PASSWORD` | Le mot de passe de votre choix pour accéder à `/admin` |
| `ADMIN_SESSION_SECRET` | Une chaîne aléatoire longue (40 caractères suffisent) |
| `NEXT_PUBLIC_CONTACT_EMAIL` | `chartreusecabinet@gmail.com` |

`ADMIN_SESSION_SECRET` sert uniquement à sécuriser la session de connexion,
vous n'avez pas besoin de vous en souvenir : générez-la une fois, collez-la, oubliez-la.

## Étape 5 — Déployer

1. Si ce n'était pas déjà fait : cliquez sur **Deploy**.
2. Si un déploiement précédent avait échoué : allez dans **Deployments**,
   menu `...` sur le dernier déploiement → **Redeploy**.
3. Au bout d'une minute, vous obtenez une URL du type
   `https://cabinet-chartreuse.vercel.app`. Ouvrez-la : le site public doit
   s'afficher (collection vide au début, c'est normal).

## Étape 6 — Ajouter vos premières bouteilles

1. Allez sur `https://votre-site.vercel.app/admin`.
2. Entrez le mot de passe choisi à l'étape 4.
3. Cliquez sur **+ Ajouter une bouteille**, remplissez le formulaire (vous
   pouvez directement envoyer une photo depuis votre ordinateur ou téléphone).
4. Retournez sur la page d'accueil pour vérifier qu'elle apparaît dans la
   bonne catégorie (Rare / Récente).

---

## Tester en local sur votre ordinateur (optionnel)

Si vous voulez tester des modifications avant de les mettre en ligne :

1. Installez Node.js (version 18 ou plus) depuis https://nodejs.org
2. `npm install`
3. Copiez `.env.local.example` en `.env.local`, remplissez les mêmes valeurs
   que sur Vercel. Pour `BLOB_READ_WRITE_TOKEN`, récupérez-le dans Vercel :
   **Storage** → votre store Blob → onglet **.env.local** → copiez la ligne.
4. `npm run dev`, puis ouvrez http://localhost:3000

Notez que les bouteilles ajoutées en local et en production partagent le
même stockage Blob (c'est le même token) : pas besoin de dupliquer les données.

## Modifier le mot de passe admin plus tard

Allez dans Vercel → **Settings** → **Environment Variables**, modifiez
`ADMIN_PASSWORD`, puis redéployez (**Deployments** → `...` → **Redeploy**).

## En cas de problème

- **"Mot de passe incorrect" alors qu'il est correct** : vérifiez qu'il n'y a
  pas d'espace avant/après dans la variable `ADMIN_PASSWORD` sur Vercel, et
  que vous avez bien redéployé après l'avoir ajoutée.
- **Le site build échoue sur Vercel** : vérifiez que les 3 variables
  (`ADMIN_PASSWORD`, `ADMIN_SESSION_SECRET`, `NEXT_PUBLIC_CONTACT_EMAIL`) sont
  bien renseignées, et que le store Blob est bien connecté au projet (étape 3).
- **Les photos ou les bouteilles n'apparaissent pas** : vérifiez que le store
  Blob de l'étape 3 est bien "Connected" au projet dans l'onglet Storage.
