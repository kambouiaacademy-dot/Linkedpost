# 🚀 LINKEDPost - Application SaaS de Génération de Posts LinkedIn

## 📋 Description

LINKEDPost est une application web SaaS qui permet aux dirigeants d'entreprises de générer des posts LinkedIn percutants et professionnels à partir d'une simple description de leurs activités.

## ✨ Fonctionnalités

- 🎨 Landing page moderne et responsive
- 🔐 Authentification complète (inscription/connexion/déconnexion)
- 📝 Générateur de posts LinkedIn avec IA (simulation)
- 📊 Estimation de performance des posts (engagement, clarté, viralité)
- 💾 Sauvegarde des posts dans Supabase
- ✏️ Modification et suppression de posts
- 📱 Design 100% responsive (mobile, tablette, desktop)

## 📂 Structure du Projet

```
linkedpost/
├── index.html          # Fichier HTML principal (renommer en index.html)
├── styles.css          # Fichier CSS (renommer en styles.css)
├── script.js           # Fichier JavaScript (renommer en script.js)
└── README.md           # Ce fichier
```

## 🔧 Configuration Supabase

### 1. Créer un Projet Supabase

1. Rendez-vous sur https://supabase.com
2. Créez un compte (gratuit)
3. Cliquez sur "New Project"
4. Remplissez les informations :
   - Nom du projet : linkedpost
   - Database Password : choisissez un mot de passe fort
   - Region : choisissez la région la plus proche

### 2. Créer la Table `posts`

Une fois votre projet créé :

1. Allez dans l'onglet **SQL Editor** (dans le menu de gauche)
2. Cliquez sur **New Query**
3. Copiez-collez le SQL suivant :

```sql
-- Créer la table posts
CREATE TABLE posts (
  id BIGSERIAL PRIMARY KEY,
  user_id UUID REFERENCES auth.users NOT NULL,
  nom_entreprise TEXT NOT NULL,
  description TEXT NOT NULL,
  secteur TEXT NOT NULL,
  cible TEXT NOT NULL,
  style TEXT NOT NULL,
  objectif TEXT,
  longueur TEXT,
  texte_genere TEXT NOT NULL,
  date_creation TIMESTAMPTZ DEFAULT NOW(),
  CONSTRAINT posts_user_id_fkey FOREIGN KEY (user_id) REFERENCES auth.users(id) ON DELETE CASCADE
);

-- Activer RLS (Row Level Security)
ALTER TABLE posts ENABLE ROW LEVEL SECURITY;

-- Politique : Les utilisateurs ne peuvent voir que leurs posts
CREATE POLICY "Users can view own posts"
  ON posts FOR SELECT
  USING (auth.uid() = user_id);

-- Politique : Les utilisateurs peuvent insérer leurs posts
CREATE POLICY "Users can insert own posts"
  ON posts FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- Politique : Les utilisateurs peuvent supprimer leurs posts
CREATE POLICY "Users can delete own posts"
  ON posts FOR DELETE
  USING (auth.uid() = user_id);
```

4. Cliquez sur **Run** pour exécuter la requête

### 3. Récupérer les Clés d'API

1. Allez dans **Settings** > **API** (dans le menu de gauche)
2. Copiez les informations suivantes :
   - **Project URL** (exemple: https://abcdefghijklmnop.supabase.co)
   - **anon public key** (clé publique anonyme)

### 4. Configurer l'Application

1. Ouvrez le fichier **script.js**
2. Remplacez les lignes suivantes (lignes 6-7) :

```javascript
const SUPABASE_URL = 'https://votre-projet.supabase.co';
const SUPABASE_ANON_KEY = 'votre-cle-anon-publique';
```

Par vos vraies valeurs :

```javascript
const SUPABASE_URL = 'https://abcdefghijklmnop.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

### 5. Configuration Email (Optionnel mais Recommandé)

Par défaut, Supabase envoie des emails de confirmation. Pour personnaliser :

1. Allez dans **Authentication** > **Email Templates**
2. Personnalisez les templates de confirmation

Pour désactiver la confirmation email en développement :
1. Allez dans **Authentication** > **Providers** > **Email**
2. Décochez "Confirm email"

## 🚀 Installation et Lancement

### Renommer les Fichiers

1. Renommez `linkedpost-index.html` en `index.html`
2. Renommez `linkedpost-styles.css` en `styles.css`
3. Renommez `linkedpost-script.js` en `script.js`

### Lancement Local

**Option 1 : Ouvrir directement**
- Double-cliquez sur `index.html`

**Option 2 : Serveur Local (Recommandé)**

Avec Python 3 :
```bash
python -m http.server 8000
```

Avec Node.js (npx) :
```bash
npx serve
```

Puis ouvrez http://localhost:8000 dans votre navigateur

## 🌐 Déploiement en Production

### Option 1 : Netlify (Recommandé)

1. Créez un compte sur https://netlify.com
2. Glissez-déposez le dossier du projet
3. Votre site est en ligne instantanément !

### Option 2 : Vercel

1. Créez un compte sur https://vercel.com
2. Importez votre projet depuis GitHub ou uploadez les fichiers
3. Déployez en un clic

### Option 3 : GitHub Pages

1. Créez un dépôt GitHub
2. Uploadez vos fichiers
3. Allez dans Settings > Pages
4. Sélectionnez la branche `main`
5. Votre site sera accessible à `https://username.github.io/linkedpost`

## 📱 Utilisation de l'Application

### 1. Inscription

1. Cliquez sur "Créer mon premier Post"
2. Remplissez le formulaire d'inscription
3. Vérifiez votre email (si la confirmation est activée)

### 2. Connexion

1. Utilisez vos identifiants
2. Vous êtes redirigé vers le Dashboard

### 3. Créer un Post

1. Remplissez le formulaire :
   - Nom de l'entreprise
   - Description de l'activité
   - Cible
   - Secteur
   - Style de communication
   - Objectif
   - Longueur
2. Cliquez sur "Générer le Post"
3. Visualisez le post et les scores de performance
4. Modifiez si nécessaire
5. Enregistrez le post

### 4. Gérer vos Posts

1. Allez dans "Mes Posts"
2. Visualisez tous vos posts sauvegardés
3. Modifiez ou supprimez selon vos besoins

## 🎨 Personnalisation

### Modifier les Couleurs

Dans `styles.css`, modifiez les variables CSS (lignes 12-20) :

```css
:root {
    --primary: #0A66C2;        /* Bleu LinkedIn */
    --secondary: #7C3AED;      /* Violet */
    --success: #10B981;        /* Vert */
    --danger: #EF4444;         /* Rouge */
    /* ... */
}
```

### Modifier les Templates de Posts

Dans `script.js`, fonction `generateMockPost()` (ligne 214), modifiez les templates pour chaque style.

## 🔒 Sécurité

- ✅ RLS (Row Level Security) activé sur Supabase
- ✅ Chaque utilisateur ne voit que ses propres posts
- ✅ Protection des routes (Dashboard accessible uniquement si connecté)
- ✅ Validation côté client et serveur

## 🐛 Dépannage

### Erreur "Failed to fetch"

- Vérifiez que vos clés Supabase sont correctes
- Vérifiez votre connexion Internet
- Vérifiez que la table `posts` existe bien

### Les posts ne s'affichent pas

- Vérifiez que les RLS Policies sont bien créées
- Vérifiez dans Supabase > Table Editor que les données existent
- Ouvrez la console du navigateur (F12) pour voir les erreurs

### Problème de connexion

- Vérifiez que l'authentification email est activée dans Supabase
- Vérifiez votre email pour la confirmation de compte
- Réinitialisez votre mot de passe si nécessaire

## 📝 Licence

Ce projet est fourni à titre éducatif et peut être librement utilisé et modifié.

## 🤝 Support

Pour toute question ou problème :
- Consultez la documentation Supabase : https://supabase.com/docs
- Vérifiez les erreurs dans la console du navigateur (F12)

## 🎯 Prochaines Améliorations Possibles

- [ ] Intégration de vraie IA (OpenAI, Claude, etc.)
- [ ] Export des posts en PDF
- [ ] Programmation de publications
- [ ] Analyse de posts existants
- [ ] Suggestions de hashtags
- [ ] Intégration LinkedIn API
- [ ] Statistiques et analytics
- [ ] Templates de posts personnalisables

---

Développé avec ❤️ pour améliorer votre présence LinkedIn !
