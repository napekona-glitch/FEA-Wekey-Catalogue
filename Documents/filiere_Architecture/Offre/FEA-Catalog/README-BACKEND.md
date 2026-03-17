# FEA-Catalog - Documentation Backend et Intégration Groq

## 📋 Vue d'ensemble

Le catalogue FEA (Filière Études d'Architecture) dispose d'un backend proxy pour l'intégration avec l'API Groq, permettant des fonctionnalités de chatbot intelligentes tout en garantissant la sécurité des clés API.

## 🏗️ Architecture Backend

### Configuration Actuelle

Le frontend utilise un backend proxy hébergé sur Cloudflare Workers :
```
Frontend → Cloudflare Workers Proxy → Groq API → LLM Response
```

**URL du backend :** `https://groq-api-proxy-public.napekona.workers.dev/api/groq`

### Fichiers de Configuration

1. **`groq-config.js`** - Configuration centralisée Groq
2. **`JS/catalog-script.js`** - Logique du chatbot avec appel backend
3. **Backend Cloudflare Workers** - Proxy sécurisé pour les appels API

## 🔧 Configuration du Token Groq

### Étape 1: Obtenir une clé API Groq

1. Créez un compte sur [https://groq.com/](https://groq.com/)
2. Allez dans la section API Dashboard
3. Générez une nouvelle clé API
4. Copiez votre clé (commence par `gsk_`)

### Étape 2: Configuration du Backend

#### Option A: Utiliser le backend existant (Recommandé)

Le backend Cloudflare Workers est déjà configuré et prêt à l'emploi. Aucune configuration supplémentaire n'est nécessaire.

#### Option B: Déployer votre propre backend

Si vous préférez déployer votre propre backend proxy :

1. **Créer un worker Cloudflare**
```bash
# Installer Wrangler
npm install -g wrangler

# Se connecter à Cloudflare
wrangler login

# Créer un nouveau worker
wrangler generate groq-proxy
cd groq-proxy
```

2. **Code du worker (index.js)**
```javascript
export default {
  async fetch(request, env, ctx) {
    if (request.method === 'POST' && new URL(request.url).pathname === '/api/groq') {
      try {
        const { model, messages, temperature, max_tokens } = await request.json();
        
        const response = await fetch('https://api.groq.com/openai/v1/chat/completions', {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${env.GROQ_API_KEY}`,
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            model: model || 'llama-3.1-8b-instant',
            messages: messages,
            temperature: temperature || 0.7,
            max_tokens: max_tokens || 500
          })
        });
        
        const data = await response.json();
        return new Response(JSON.stringify(data), {
          headers: { 'Content-Type': 'application/json' },
          status: response.status
        });
      } catch (error) {
        return new Response(JSON.stringify({ error: error.message }), {
          status: 500,
          headers: { 'Content-Type': 'application/json' }
        });
      }
    }
    
    return new Response('Not Found', { status: 404 });
  }
};
```

3. **Configuration des variables d'environnement (wrangler.toml)**
```toml
name = "groq-proxy"
main = "src/index.js"
compatibility_date = "2023-10-30"

[vars]
# La clé API sera configurée via wrangler secret
```

4. **Déployer et configurer la clé API**
```bash
# Déployer le worker
wrangler deploy

# Configurer la clé API comme secret
wrangler secret put GROQ_API_KEY
# Entrez votre clé API Groq quand prompted
```

### Étape 3: Mettre à jour le Frontend

Modifiez `JS/catalog-script.js` ligne 469 pour pointer vers votre backend :

```javascript
const BACKEND_URL = 'https://votre-worker.votre-subdomain.workers.dev/api/groq';
```

## 🔐 Sécurité

### Bonnes Pratiques

1. **Ne jamais exposer la clé API côté client**
2. **Utiliser toujours un backend proxy**
3. **Limiter les taux d'appels**
4. **Surveiller l'utilisation de l'API**

### Variables d'Environnement

- `GROQ_API_KEY`: Votre clé API Groq (stockée comme secret)
- `RATE_LIMIT`: Limite d'appels par minute (optionnel)

## 📊 Modèles Disponibles

- **llama-3.1-8b-instant**: Rapide et économique (recommandé)
- **llama-3.1-70b-versatile**: Plus puissant pour réponses complexes
- **mixtral-8x7b-32768**: Excellent pour le français
- **gemma-7b-it**: Léger et rapide

## 🚀 Déploiement

### Production

1. Utilisez le backend Cloudflare Workers existant
2. Configurez votre clé API via les secrets
3. Testez avec des requêtes pilotes

### Développement Local

Pour tester en développement local :

```bash
# Installer Node.js dépendances
npm install

# Démarrer le serveur de développement
npx wrangler dev

# Tester localement sur http://localhost:8787
```

## 🔧 Personnalisation

### Modifier les Paramètres du Chatbot

Dans `JS/catalog-script.js`, vous pouvez ajuster :

```javascript
// Paramètres Groq
{
    model: 'llama-3.1-8b-instant',  // Modèle utilisé
    temperature: 0.7,                // Créativité (0.0-1.0)
    max_tokens: 500,                 // Longueur max réponse
}
```

### Messages Système

Personnalisez le prompt système dans `catalog-script.js` :

```javascript
{
    role: 'system',
    content: `Vous êtes un assistant expert pour le catalogue d'architecture...`
}
```

## 📈 Monitoring

### Métriques à Surveiller

- **Taux de réussite des appels API**
- **Temps de réponse moyen**
- **Nombre de tokens utilisés**
- **Erreurs 429 (rate limiting)**

### Outils

- **Cloudflare Analytics**: Pour surveiller le backend
- **Groq Dashboard**: Pour l'utilisation API
- **Console navigateur**: Pour le debugging frontend

## 🐛 Dépannage

### Problèmes Communs

1. **Erreur 401/403**
   - Vérifiez que votre clé API est valide
   - Assurez-vous que le secret est bien configuré

2. **Erreur CORS**
   - Vérifiez que les headers CORS sont configurés dans le worker
   - Ajoutez `Access-Control-Allow-Origin: *`

3. **Erreur 429**
   - Trop de requêtes simultanées
   - Implémentez un rate limiting côté backend

4. **Réponses basiques uniquement**
   - Vérifiez que le backend est accessible
   - Consultez la console pour les erreurs réseau

### Logs et Debug

```javascript
// Dans le frontend
console.log('URL appelée:', BACKEND_URL);
console.error('Erreur API:', error);

// Dans le worker Cloudflare
console.log('Requête reçue:', request.url);
console.log('Réponse Groq:', response.status);
```

## 🔄 Mises à Jour

### Mettre à Jour le Modèle

1. Modifiez le modèle dans `catalog-script.js`
2. Redéployez le worker si nécessaire
3. Testez les nouvelles capacités

### Ajouter de Nouvelles Fonctionnalités

1. **Cache des réponses**: Implémentez Redis/KV
2. **Analytics**: Ajoutez le suivi des questions
3. **Multimodal**: Support des images/documents

## 📞 Support

- **Documentation Groq**: [https://console.groq.com/docs](https://console.groq.com/docs)
- **Documentation Cloudflare Workers**: [https://developers.cloudflare.com/workers](https://developers.cloudflare.com/workers)
- **Issues du projet**: Signalez les problèmes dans le repository

---

**Architecte**: Napé Kona  
**Dernière mise à jour**: Mars 2025  
**Version**: 1.0.0
