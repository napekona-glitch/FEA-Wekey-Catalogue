# 🏗️ FEA-Catalog - Catalogue d'Architecture d'Entreprise

## 📋 Vue d'ensemble

Le **FEA-Catalog** est une plateforme web complète présentant les 15 offres de services de la Filière Études d'Architecture (FEA). Ce catalogue interactif inclut un chatbot intelligent avec intégration Groq API pour guider les clients vers les solutions adaptées à leurs besoins.

## ✨ Fonctionnalités Principales

### 🎯 Interface Utilisateur Moderne
- **Design responsive** adapté à tous les écrans
- **Navigation intuitive** avec filtres par catégorie
- **Fiches détaillées** pour chaque offre de service
- **Génération PDF** personnalisée pour chaque fiche
- **Mode impression** optimisé pour les flyers marketing

### 🤖 Chatbot Intelligent
- **Intégration Groq API** avec Llama 3.1
- **Backend proxy sécurisé** via Cloudflare Workers
- **Mode fallback** basé sur mots-clés
- **Réponses contextuelles** et personnalisées
- **Support multilingue** (français optimisé)

### 📊 Offres de Services (15 prestations)

#### 🏛️ **Architecture**
- **Architecture Data Mesh** - Modernisation de la gestion de données
- **Urbanisation SI** - Transformation du système d'information
- **Migration Cloud Hybride** - Transition nuage optimisée
- **Monolithe vers Microservices** - Modernisation applicative

#### ☁️ **Cloud & Infrastructure**
- **Infrastructure as Code** - Automatisation de l'infrastructure
- **SD-WAN & SASE** - Réseau étendu sécurisé
- **Observabilité & Monitoring** - Supervision complète

#### 🔧 **DevOps & Qualité**
- **CI/CD & Industrialisation** - Pipeline de livraison continue
- **Audit Qualité Logicielle** - Assurance qualité applicative
- **MLOps & IA Industrielle** - Industrialisation de l'IA

#### 🔒 **Sécurité**
- **Zero Trust & IAM** - Sécurité par conception
- **PCA/PRA & Continuité** - Plan de continuité d'activité

#### 📈 **Data & Workplace**
- **RAG & IA Générative Responsable** - IA éthique et responsable
- **Modernisation Digital Workplace** - Transformation du poste de travail

#### 🔍 **API & Integration**
- **API Management & Integration** - Gestion et intégration d'API

## 🚀 Démarrage Rapide

### Prérequis
- Navigateur web moderne (Chrome, Firefox, Safari, Edge)
- Connexion internet pour le chatbot intelligent

### Installation Locale

1. **Cloner le repository**
```bash
git clone https://github.com/votre-organization/FEA-Catalog.git
cd FEA-Catalog
```

2. **Ouvrir le catalogue**
```bash
# Simple double-clic sur index.html
# Ou utiliser un serveur local
python -m http.server 8000
# Puis ouvrir http://localhost:8000
```

3. **Configurer le chatbot (optionnel)**
- Le chatbot fonctionne déjà avec le backend existant
- Pour personnaliser, voir `README-BACKEND.md`

## 🏗️ Architecture Technique

### Frontend
- **HTML5** sémantique et accessible
- **CSS3** moderne avec animations et transitions
- **JavaScript ES6+** vanilla (pas de framework lourd)
- **Responsive Design** mobile-first

### Backend & API
- **Cloudflare Workers** comme proxy sécurisé
- **Groq API** pour les fonctionnalités LLM
- **RESTful API** pour l'intégration chatbot
- **Rate limiting** et sécurité intégrée

### Fichiers Principaux
```
├── index.html                    # Page principale du catalogue
├── CSS/
│   ├── catalog-style.css         # Styles principaux
│   ├── service-detail.css        # Styles fiches détaillées
│   └── offre-commercial.css      # Styles marketing
├── JS/
│   ├── catalog-script.js         # Logique principale
│   ├── chatbot-responses.js      # Réponses basiques chatbot
│   ├── fiche-script.js           # Logique fiches détaillées
│   └── html-to-docx.js           # Export Word/Docx
├── groq-config.js               # Configuration Groq
├── README-BACKEND.md            # Documentation backend
└── fiche-*.html                  # Fiches détaillées (15 fichiers)
```

## 🎨 Personnalisation

### Modifier les Couleurs
Dans `CSS/catalog-style.css` :
```css
:root {
    --primary-color: #FF6B35;      /* Orange Wekey */
    --secondary-color: #2C3E50;    /* Bleu marine */
    --accent-color: #E74C3C;       /* Rouge accent */
}
```

### Ajouter une Nouvelle Offre
1. Créer `fiche-nouvelle-offre.html`
2. Ajouter l'entrée dans `catalog-script.js`
3. Mettre à jour les filtres et catégories

### Personnaliser le Chatbot
Voir la documentation complète dans `README-BACKEND.md`

## 📱 Fonctionnalités Avancées

### 🖨️ Génération PDF
- **Export individuel** de chaque fiche en PDF
- **Mise en page professionnelle** optimisée pour l'impression
- **Mode flyer marketing** onepager ou multi-pages

### 🔍 Recherche et Filtrage
- **Filtres par catégorie** (Architecture, Cloud, DevOps, etc.)
- **Recherche textuelle** instantanée
- **Tags et mots-clés** pour chaque offre

### 📊 Analytics et Monitoring
- **Suivi des interactions** chatbot
- **Métriques d'utilisation** des fiches
- **Logs d'erreurs** et performance

## 🔧 Configuration Backend

### Token Groq API

Le catalogue utilise un backend proxy sécurisé pour les appels Groq API :

1. **Backend existant** (recommandé) :
   - URL : `https://groq-api-proxy-public.napekona.workers.dev/api/groq`
   - Déjà configuré et fonctionnel

2. **Backend personnalisé** :
   - Déployez votre propre proxy Cloudflare Workers
   - Configurez votre token Groq en tant que secret
   - Mettez à jour l'URL dans `JS/catalog-script.js`

**Documentation complète** : Voir `README-BACKEND.md`

## 🌐 Déploiement

### Production
1. **Hébergement statique** (Netlify, Vercel, GitHub Pages)
2. **Configurer le domaine** personnalisé
3. **Activer HTTPS** (obligatoire pour l'API Groq)
4. **Tester l'intégration** chatbot

### Docker (Optionnel)
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## 🔒 Sécurité

### Mesures Implémentées
- **Backend proxy** pour protéger les clés API
- **CORS configuré** pour les appels légitimes
- **Rate limiting** contre les abus
- **HTTPS obligatoire** en production

### Bonnes Pratiques
- Ne jamais exposer les clés API côté client
- Utiliser des secrets pour les variables sensibles
- Surveiller les logs d'utilisation
- Maintenir les dépendances à jour

## 🐛 Dépannage

### Problèmes Communs

**Chatbot ne répond pas intelligemment**
- Vérifiez la connexion internet
- Consultez la console navigateur (F12)
- Testez le backend : `curl https://groq-api-proxy-public.napekona.workers.dev/api/groq`

**PDF ne se génère pas**
- Vérifiez que le navigateur supporte l'API Print
- Désactivez les bloqueurs de publicité
- Essayez un autre navigateur

**Styles mobiles incorrects**
- Videz le cache navigateur
- Vérifiez le viewport meta tag
- Testez sur différents appareils

### Logs et Debug
```javascript
// Activer le mode debug dans catalog-script.js
const DEBUG_MODE = true;
console.log('Debug:', { variable, data });
```

## 📈 Roadmap

### Version 2.0 (Prévue)
- [ ] **Interface admin** pour modifier les offres
- [ ] **Base de données** dynamique (SQLite/PostgreSQL)
- [ ] **Authentification** utilisateurs
- [ ] **Dashboard analytics** avancé
- [ ] **API REST** complète

### Améliorations Continues
- [ ] **Voix synthétisée** pour le chatbot
- [ ] **Support multimodal** (images, documents)
- [ ] **Chat multilingue** avancé
- [ ] **Intégration CRM** existant
- [ ] **Mode offline** PWA

## 🤝 Contribution

### Comment Contribuer
1. **Fork** le repository
2. **Créer une branche** feature/nouvelle-fonctionnalité
3. **Commit** avec messages clairs
4. **Push** vers votre fork
5. **Pull request** détaillée

### Standards de Code
- **Indentation** : 4 espaces
- **Commentaires** : JavaScript JSDoc
- **Nommage** : camelCase pour JS, kebab-case pour CSS
- **Accessibilité** : WCAG 2.1 AA minimum

## 📞 Support & Contact

### Documentation Complémentaire
- `README-BACKEND.md` - Configuration backend Groq
- `README-GROQ.md` - Guide d'intégration Groq
- `STORYLINE.md` - Scénario utilisateur détaillé

### Assistance Technique
- **Issues GitHub** : Pour les bugs et demandes
- **Documentation** : Consultez les fichiers README
- **Community** : Partagez vos retours et améliorations

---

**Architecte Technique** : Napé Kona  
**Dernière Mise à Jour** : Mars 2025  
**Version** : 1.2.0  
**Licence** : MIT

---

## 🎯 Quick Start (5 minutes)

1. **Clonez** : `git clone [repository-url| https://github.com/napekona-glitch/FEA-Catalogue-Wekey] && cd FEA-Catalog`
2. **Ouvrez** : Double-cliquez sur `index.html`
3. **Explorez** : Naviguez dans le catalogue
4. **Testez** : Posez une question au chatbot
5. **Exportez** : Générez un PDF d'une fiche

**Le catalogue est prêt à l'emploi !** 🚀
