# Suivi TGBT ligne B · CLAM BIM

Application de suivi de la commande DGEMA : vectorisation des schémas de 21 TGBT (379 folios, 4 990 € HT), avancement par folio, contrôle qualité E1 à E6, lots, livraisons et facturation.

| Fichier | Rôle |
| --- | --- |
| `index.html` | L'application, en un seul fichier, à publier sur GitHub Pages |
| `schema.sql` | La base : tables, sécurité, temps réel et données initiales de la commande |
| `README.md` | Ce guide |

Projet Supabase : `https://ovtjfchwufljmdlzhzem.supabase.co`

## 1. Préparer Supabase (une seule fois)

1. **SQL Editor** > **New query** : collez tout le contenu de `schema.sql`, puis **Run**.
   Le résultat final doit afficher `21 | 379 | 4 | 4990.00` (TGBT, folios, lots, montant HT).
   Le script peut être relancé sans risque : il ne remplace aucune donnée existante.
2. **Authentication** > **Sign In / Providers** > **Email** : désactivez **Allow new users to sign up**.
   Seuls les comptes que vous créez pourront se connecter.
3. **Authentication** > **URL Configuration** :
   - **Site URL** : `https://clambim.github.io/Suivi-TGBT/`
   - **Redirect URLs** : ajoutez la même adresse.
   Ces adresses servent aux liens d'invitation et de réinitialisation du mot de passe.
4. **Authentication** > **Users** > **Add user** > **Create new user** : adresse e-mail, mot de passe provisoire, cochez **Auto Confirm User**. Répétez pour chaque membre de l'équipe.

> Le service d'e-mail fourni par défaut par Supabase n'envoie qu'aux membres de votre organisation Supabase et avec un débit limité. Pour les invitations par e-mail ou le lien « Mot de passe oublié » vers l'équipe de Rabat, configurez un serveur SMTP dans **Authentication** > **Emails** > **SMTP Settings**. Sans SMTP, créez les comptes avec un mot de passe (étape 4) et transmettez-le.

## 2. Publier sur GitHub Pages

1. Créez le dépôt `ClamBim/Suivi-TGBT` (GitHub Pages sur un dépôt privé demande un abonnement payant ; un dépôt public ne pose pas de problème de confidentialité, les données restant dans Supabase derrière la connexion).
2. **Add file** > **Upload files** : déposez `index.html`, `schema.sql` et `README.md` à la racine, puis **Commit changes**.
3. **Settings** > **Pages** > **Build and deployment** : **Deploy from a branch**, branche `main`, dossier `/ (root)`, **Save**.
4. Après une à deux minutes, l'application est en ligne à `https://clambim.github.io/Suivi-TGBT/`.

Si vous choisissez un autre nom de dépôt, reportez la nouvelle adresse dans **Site URL** et **Redirect URLs** (étape 1.3).

## 3. Utilisation

- Connexion par e-mail et mot de passe ; la session reste ouverte sur le poste jusqu'à la déconnexion.
- **Production** : choisissez un statut puis cliquez ou glissez sur les folios. Les autres onglets suivent le contrôle qualité, les lots, les livraisons, les factures et les paramètres (montant HT, temps unitaires, équipe).
- Les modifications s'affichent en temps réel chez tous les utilisateurs connectés.
- **Point d'avancement** prépare le texte du mail du vendredi ; **Exporter en CSV** télécharge l'état par TGBT (séparateur `;`, ouvrable dans Excel).

## 4. Sécurité

- La clé `sb_publishable_…` écrite dans `index.html` est une clé publique, prévue pour être visible dans une page web.
- Ce qui protège les données, ce sont les règles de la base (RLS) posées par `schema.sql` : un visiteur non connecté n'a accès à rien ; un utilisateur connecté peut lire et modifier le suivi, mais pas créer ni supprimer de TGBT ou de lot.
- Chaque modification enregistre sa date (`updated_at`) et son auteur (`updated_by`, identifiant du compte Supabase).

## 5. Mettre à jour

- Application : redéposez le nouveau `index.html` dans le dépôt.
- Base : un nouveau `schema.sql` se relance dans SQL Editor sans perte de données.
