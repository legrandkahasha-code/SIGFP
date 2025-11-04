# 🔧 Dépannage Erreur 404 Netlify - SIGFP 5

**Erreur**: Site introuvable (404)  
**ID Netlify**: 01K98DQGXTEK4WNMCDM1ZE2XZJ  
**Date**: 4 Novembre 2025

---

## 🔍 Diagnostic du Problème

L'erreur 404 "Site introuvable" sur Netlify peut avoir **3 causes principales**:

1. ❌ **Build Failed** (le build a échoué)
2. ⚠️ **Variables d'environnement manquantes** (VITE_SUPABASE_URL, etc.)
3. ⚠️ **Configuration incorrecte** (publish directory)

---

## ✅ Solution Étape par Étape

### Étape 1: Vérifier les Logs de Build Netlify

**Sur Netlify Dashboard:**

1. Aller sur https://app.netlify.com
2. Cliquer sur votre site SIGFP
3. Aller dans **"Deploys"** (onglet)
4. Cliquer sur le dernier déploiement
5. **Regarder les logs de build**

**Chercher dans les logs:**

```bash
❌ ERREUR: "Build failed"
❌ ERREUR: "Command failed with exit code 1"
❌ ERREUR: "Module not found"
❌ ERREUR: "VITE_SUPABASE_URL is not defined"
```

---

### Étape 2: Ajouter les Variables d'Environnement (CRITIQUE)

**C'est LA cause la plus fréquente du problème !**

#### Sur Netlify:

1. **Site settings** → **Environment variables** (dans le menu gauche)
2. **Cliquer** "Add a variable"
3. **Ajouter ces 2 variables OBLIGATOIRES**:

```env
Key: VITE_SUPABASE_URL
Value: https://VOTRE_PROJET.supabase.co

Key: VITE_SUPABASE_ANON_KEY
Value: votre_cle_anonyme_supabase_ici
```

4. **Cliquer** "Save"
5. **Redéployer**: Aller dans "Deploys" → "Trigger deploy" → "Deploy site"

⚠️ **ATTENTION**: Les variables doivent commencer par `VITE_` pour Vite !

---

### Étape 3: Vérifier la Configuration Build

#### Dans Netlify Site Settings → Build & deploy → Build settings:

**Vérifier que ces paramètres sont corrects:**

```
Build command: npm run build
Publish directory: dist
```

**Si ce n'est pas le cas:**
1. Cliquer "Edit settings"
2. Corriger les valeurs
3. Sauvegarder
4. Redéployer

---

### Étape 4: Forcer un Nouveau Build

**Méthode 1: Via l'interface Netlify**

1. Aller dans **"Deploys"**
2. Cliquer **"Trigger deploy"**
3. Sélectionner **"Clear cache and deploy site"**
4. Attendre 3-4 minutes

**Méthode 2: Via un nouveau commit GitHub**

```powershell
cd "c:\Users\LEGRAND\OneDrive\Desktop\SIGFP 5\project"

# Faire un petit changement (ex: espace dans README)
git add .
git commit -m "fix: force redeploy pour Netlify"
git push origin master

# Netlify va automatiquement rebuild
```

---

## 🎯 Checklist de Vérification

Cocher au fur et à mesure:

### Variables d'Environnement
- [ ] `VITE_SUPABASE_URL` ajoutée dans Netlify
- [ ] `VITE_SUPABASE_ANON_KEY` ajoutée dans Netlify
- [ ] Les variables commencent bien par `VITE_`
- [ ] Les valeurs sont correctes (pas de guillemets, espaces, etc.)

### Configuration Build
- [ ] Build command: `npm run build`
- [ ] Publish directory: `dist`
- [ ] Node version: 18 (via netlify.toml)
- [ ] netlify.toml présent dans le repo

### Fichiers Requis
- [ ] `netlify.toml` dans la racine du projet
- [ ] `public/_redirects` existe
- [ ] `package.json` a le script `build`
- [ ] `index.html` à la racine

### GitHub
- [ ] Code poussé sur GitHub
- [ ] netlify.toml inclus dans le commit
- [ ] Aucun fichier manquant

---

## 📋 Erreurs Courantes et Solutions

### Erreur 1: "Build failed: Command not found"

**Cause**: npm ou node pas reconnu

**Solution**:
```toml
# Dans netlify.toml (déjà configuré)
[build.environment]
  NODE_VERSION = "18"
```

---

### Erreur 2: "Module not found: Error: Can't resolve '@supabase/supabase-js'"

**Cause**: Dépendances pas installées

**Solution**: Netlify fait automatiquement `npm install`. Si ça échoue:
```bash
# Vérifier package.json localement
npm ci
npm run build
```

---

### Erreur 3: "VITE_SUPABASE_URL is not defined"

**Cause**: Variables d'environnement manquantes

**Solution**: Voir Étape 2 ci-dessus

---

### Erreur 4: "Page not found" après le build réussi

**Cause**: Routing SPA mal configuré

**Solution**: Vérifier que `public/_redirects` contient:
```
/*    /index.html   200
```

---

## 🔧 Test en Local Avant Redéploiement

**Pour être sûr que tout fonctionne:**

```powershell
cd "c:\Users\LEGRAND\OneDrive\Desktop\SIGFP 5\project"

# 1. Nettoyer
Remove-Item -Recurse -Force dist, node_modules

# 2. Réinstaller
npm install

# 3. Tester le build
npm run build

# 4. Tester en local
npm run preview
```

**Si ça marche en local → le problème vient de Netlify (variables env probablement)**

---

## 📞 Obtenir de l'Aide de Netlify

### Voir les Logs Détaillés

1. **Netlify Dashboard** → Votre site
2. **Deploys** → Dernier déploiement
3. **Cliquer sur le log** pour voir tous les détails
4. **Copier l'erreur exacte**

### Support Netlify

Si le problème persiste:
- **Forum**: https://answers.netlify.com
- **Support**: https://www.netlify.com/support
- **Docs**: https://docs.netlify.com

---

## ✅ Solution Rapide (Très Probable)

**Dans 90% des cas, le problème est lié aux variables d'environnement.**

**Action immédiate:**

1. Aller sur https://app.netlify.com
2. Sélectionner votre site SIGFP
3. **Site settings** → **Environment variables**
4. Ajouter:
   ```
   VITE_SUPABASE_URL = https://VOTRE_PROJET.supabase.co
   VITE_SUPABASE_ANON_KEY = votre_cle_anon_ici
   ```
5. **Deploys** → **Trigger deploy** → **Clear cache and deploy site**
6. Attendre 3-4 minutes
7. ✅ Site fonctionnel !

---

## 🎬 Que Faire Maintenant ?

### Option A: Variables Manquantes (90% des cas)

```
1. Ajouter VITE_SUPABASE_URL et VITE_SUPABASE_ANON_KEY
2. Redéployer (Trigger deploy)
3. Attendre 3 min
4. Tester le site
```

### Option B: Build Failed (logs montrent des erreurs)

```
1. Lire les logs de build Netlify
2. Copier l'erreur exacte
3. Corriger le problème en local
4. Commit + Push
5. Netlify rebuild automatiquement
```

### Option C: Configuration Incorrecte

```
1. Vérifier Build command = "npm run build"
2. Vérifier Publish directory = "dist"
3. Sauvegarder
4. Redéployer
```

---

## 📊 Après Correction - Vérifications

Une fois le site déployé avec succès:

- [ ] URL Netlify accessible (ex: https://VOTRE-SITE.netlify.app)
- [ ] Page d'accueil charge correctement
- [ ] Navigation entre pages fonctionne
- [ ] Aucune erreur dans la console navigateur
- [ ] Modules SIGFP chargent correctement
- [ ] Supabase connecté (vérifier dans DevTools)

---

## 💡 Prévention Future

Pour éviter ce problème à l'avenir:

1. ✅ **Toujours ajouter les variables env AVANT le premier deploy**
2. ✅ **Tester `npm run build` localement avant de pusher**
3. ✅ **Vérifier les logs de build Netlify après chaque deploy**
4. ✅ **Garder netlify.toml à jour dans le repo**
5. ✅ **Documenter les variables env nécessaires dans .env.example**

---

## 🎯 Résumé Rapide

**Problème**: 404 sur Netlify  
**Cause probable**: Variables d'environnement manquantes  
**Solution rapide**: Ajouter `VITE_SUPABASE_URL` et `VITE_SUPABASE_ANON_KEY`  
**Temps**: 5 minutes  
**Résultat**: Site fonctionnel ✅

---

**Besoin d'aide ? Partagez les logs de build Netlify pour diagnostic plus précis !**
